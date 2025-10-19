# 为 macOS 构建 Zed

## 代码仓库

克隆 [Zed 代码仓库](https://github.com/zed-industries/zed)。

## 依赖项

- 安装 [rustup](https://www.rust-lang.org/tools/install)

- 从 macOS App Store 或 [Apple Developer](https://developer.apple.com/download/all/) 网站安装 [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)。注意这需要一个开发者账户。

> 请确保安装后启动 Xcode，并安装 macOS 组件，这是默认选项。

- 安装 [Xcode 命令行工具](https://developer.apple.com/xcode/resources/)

  ```sh
  xcode-select --install
  ```

- 确保 Xcode 命令行工具使用新安装的 Xcode 版本：

  ```sh
  sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
  sudo xcodebuild -license accept
  ```

- 安装 `cmake`（由 [某个依赖项](https://docs.rs/wasmtime-c-api-impl/latest/wasmtime_c_api/) 要求）

  ```sh
  brew install cmake
  ```

### 后端依赖项（可选）{#backend-dependencies}

若需使用本地协作服务器开发 Zed 协作功能，请参阅：[本地协作](./local-collaboration.md)文档。

## 从源码构建 Zed

安装依赖项后，可通过 [Cargo](https://doc.rust-lang.org/cargo/) 构建 Zed。

调试版本构建：

```sh
cargo run
```

发行版本构建：

```sh
cargo run --release
```

运行测试：

```sh
cargo test --workspace
```

## 故障排除

### Metal 着色器编译错误

```sh
error: failed to run custom build command for gpui v0.1.0 (/Users/path/to/zed)`**

xcrun: error: unable to find utility "metal", not a developer tool or in PATH
```

尝试执行 `sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer`

若使用 macOS 26 系统，请尝试 `xcodebuild -downloadComponent MetalToolchain`

### Cargo 报错提示依赖项使用不稳定功能

请依次尝试 `cargo clean` 与 `cargo build`

### 错误：找不到 'dispatch/dispatch.h' 文件

若遇到类似错误：

```sh
src/platform/mac/dispatch.h:1:10: fatal error: 'dispatch/dispatch.h' file not found

Caused by:
  process didn't exit successfully

  --- stdout
  cargo:rustc-link-lib=framework=System
  cargo:rerun-if-changed=src/platform/mac/dispatch.h
  cargo:rerun-if-env-changed=TARGET
  cargo:rerun-if-env-changed=BINDGEN_EXTRA_CLANG_ARGS_aarch64-apple-darwin
  cargo:rerun-if-env-changed=BINDGEN_EXTRA_CLANG_ARGS_aarch64_apple_darwin
  cargo:rerun-if-env-changed=BINDGEN_EXTRA_CLANG_ARGS
```

该文件属于 Xcode 组件。请确保已安装 Xcode 命令行工具并设置正确路径：

```sh
xcode-select --install
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

同时配置 `BINDGEN_EXTRA_CLANG_ARGS` 环境变量：

```sh
export BINDGEN_EXTRA_CLANG_ARGS="--sysroot=$(xcrun --show-sdk-path)"
```

最后清理并重新构建项目：

### 测试因 `Too many open files (os error 24)` 而失败

此错误似乎是由操作系统资源限制引起的。使用 `cargo-nextest` 安装并运行测试应能解决问题。

- `cargo install cargo-nextest --locked`
- `cargo nextest run --workspace --no-fail-fast`

## 技巧与窍门

### 避免持续重新构建

如果您发现 Zed 持续重新构建根 crate，可能是因为您将开发环境中的 Zed 指向了代码库本身。

这会导致问题，因为 `cargo run` 导出了一系列环境变量，这些变量会被开发构建的 Zed 中运行的 `rust-analyzer` 捕获。这些环境变量随后传递给 `cargo check`，从而使得我们依赖的部分 crate 的构建缓存失效。

您可以使用 `cargo run
~/path/to/other/project` 轻松避免在已检出的 Zed 代码库上运行构建后的二进制文件，以确保不会遇到此问题。

### 加速验证流程

若您频繁构建 Zed，可能会发现 macOS 会持续验证新构建版本，这将延长每次迭代周期数秒时间。

可通过以下步骤解决：

- 执行 `sudo spctl developer-mode enable-terminal` 以启用系统设置中的开发者工具面板
- 在系统设置中搜索“开发者工具”，将您的终端程序（如 iTerm 或 Ghostty）添加到“允许应用使用开发者工具”列表
- 重启终端

感谢 nextest 开发团队发布的[此解决方案](https://nexte.st/docs/installation/macos/#gatekeeper)。