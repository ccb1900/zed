# 为 Linux 构建 Zed

## 代码仓库

克隆 [Zed 代码仓库](https://github.com/zed-industries/zed)。

## 依赖项

- 安装 [rustup](https://www.rust-lang.org/tools/install)

- 安装必要的系统库：

  ```sh
  script/linux
  ```

  如果希望手动安装系统库，可以在 `script/linux` 文件中找到所需软件包列表。

### 后端依赖项（可选）{#backend-dependencies}

如需使用本地协作服务器开发 Zed 协作功能，请参阅：[本地协作](./local-collaboration.md) 文档。

### 链接器 {#linker}

在 Linux 系统中，Rust 默认使用 [LLVM 的 `lld`](https://blog.rust-lang.org/2025/09/18/Rust-1.90.0/) 作为链接器。使用替代链接器（特别是 [Wild](https://github.com/davidlattimore/wild) 和 [Mold](https://github.com/rui314/mold)）可显著提升全新构建与增量构建的速度。

目前 Zed 在持续集成环境中使用 Mold 链接器，因其更为成熟稳定。对于本地开发环境，我们推荐采用 Wild 链接器，其编译速度比 Mold 快 5%-20%。

可通过执行 `script/install-mold` 和 `script/install-wild` 命令安装这两款链接器。

若要将 Wild 设为默认链接器，请在 `~/.cargo/config.toml` 配置文件中添加以下配置：

```toml
[target.x86_64-unknown-linux-gnu]
linker = "clang"
rustflags = ["-C", "link-arg=--ld-path=wild"]

[target.aarch64-unknown-linux-gnu]
linker = "clang"
rustflags = ["-C", "link-arg=--ld-path=wild"]
```

若要将 Mold 设为默认链接器：

```toml
[target.'cfg(target_os = "linux")']
rustflags = ["-C", "link-arg=-fuse-ld=mold"]
```

## 从源码构建

安装完依赖项后，您可以使用 [Cargo](https://doc.rust-lang.org/cargo/) 构建 Zed。

执行调试版本构建：

```sh
cargo run
```

运行测试套件：

```sh
cargo test --workspace
```

在发布模式下，主要用户界面由 `cli` 组件库提供。开发时可通过以下命令运行：

## 安装开发版本

您可以通过以下命令在本地机器上安装构建版本：

```sh
./script/install-linux
```

该命令将以发布模式构建 zed 和命令行工具，并将其安装至 `~/.local/bin/zed` 路径，同时将 .desktop 文件安装至 `~/.local/share`。

> **_注意_**: 如果遇到类似以下链接器错误：

> [[代码块_0]]

> **原因**：
> 这是由 aws-lc-rs 的已知缺陷导致（不支持 GCC >= 14）：[FIPS 无法在 GCC >= 14 环境下构建](https://github.com/aws/aws-lc-rs/issues/569)
> 与 [GCC-14 - FIPS 模块构建失败](https://github.com/aws/aws-lc/issues/2010)

> 更多信息可参考 [linux：使用 script/install-linux 时 remote_server 出现链接器错误](https://github.com/zed-industries/zed/issues/24880)

> **临时解决方案**：
> 将远程服务器目标设置为 [[代码块_1]]，具体操作如下 [[代码块_2]]

## Wayland 与 X11

Zed 同时支持 X11 和 Wayland 显示协议。默认情况下，系统会在运行时自动选择可用的显示协议。若您在 Wayland 环境下希望切换至 X11 模式，请设置环境变量 `WAYLAND_DISPLAY=''`。

## Zed 软件打包指南

感谢您承担 Zed 的打包工作！

### 技术要求

Zed 包含两个主要可执行文件：

- 你需要构建 `crates/cli`，并将其二进制文件放置在 `$PATH` 目录下，命名为 `zed`。
- 你需要构建 `crates/zed`，并将其安装到 `$PATH/to/cli/../../libexec/zed-editor` 路径。例如，若计划将命令行工具放在 `~/.local/bin/zed`，则应将 Zed 主程序置于 `~/.local/libexec/zed-editor`。由于部分 Linux 发行版（如 Arch）不推荐使用 `libexec`，你也可以将该二进制文件放在 `$PATH/to/cli/../../lib/zed/zed-editor`（例如 `~/.local/lib/zed/zed-editor`）路径下。
- 如需提供 `.desktop` 桌面文件，可在 `crates/zed/resources/zed.desktop.in` 找到模板，并使用 `envsubst` 填充所需参数。该文件需重命名为 `$APP_ID.desktop` 以符合 [FreeDesktop 标准](https://github.com/zed-industries/zed/issues/12707#issuecomment-2168742761)。同时需确保该桌面文件具有可执行权限（`chmod 755`）。
- 需确保系统已安装必要的依赖库。可通过[检查已构建的二进制文件](https://github.com/zed-industries/zed/blob/935cf542aebf55122ce6ed1c91d0fe8711970c82/script/bundle-linux#L65-L67)获取当前所需的库列表。
- 完整构建脚本示例可参考 [script/bundle-linux](https://github.com/zed-industries/zed/blob/935cf542aebf55122ce6ed1c91d0fe8711970c82/script/bundle-linux)。
- 可通过设置环境变量 `ZED_UPDATE_EXPLANATION` 禁用 Zed 自动更新，并为手动更新用户提供指引。例如：`ZED_UPDATE_EXPLANATION="Please use flatpak to update zed."`。
- 请将 `crates/zed/RELEASE_CHANNEL` 文件内容更新为不含换行符的 'nightly'、'preview' 或 'stable'，这将使 Zed 使用凭证管理器保存用户登录状态。

### 其他注意事项

在Zed，我们的首要任务是快速迭代，将最新技术带给用户。长期以来，我们对运行缓慢、版本落后或配置繁琐的软件深感困扰，因此我们基于这些痛点打造了这款编辑器。

但我们深知，许多Linux发行版有着不同的发展重点。我们期待与各方合作，将Zed推向更多主流平台。不过要实现这一目标，我们仍有很长的路要走：

- Zed 是一个快速迭代的早期项目。我们通常每周发布 2-3 个版本来修复用户反馈的问题并推出主要功能。
- Linux 系统中可能存在其他名为 `zed` 的可执行文件（[1](https://openzfs.github.io/openzfs-docs/man/v2.2/8/zed.8.html)、[2](https://zed.brimdata.io/docs/commands/zed)）。若需为此重命名我们的 CLI 工具，建议使用 `zedit`、`zeditor` 或 `zed-cli`。
- Zed 会自动安装正确版本的常用开发工具，其方式类似于 rustup/rbenv/pyenv 等。我们理解这一设计存在争议，[详见此处](https://github.com/zed-industries/zed/issues/12589)。
- 我们允许用户安装本地扩展及来自 [zed-industries/extensions](https://github.com/zed-industries/extensions) 的扩展。这些扩展可能会按需安装更多工具（如语言服务器）。长期来看，我们希望提升此过程的安全性，[详见此处](https://github.com/zed-industries/zed/issues/12358)。
- Zed 默认会连接多个在线服务（AI、遥测、协作功能）。用户可通过 Zed 设置或修改我们的[默认配置文件](https://github.com/zed-industries/zed/blob/main/assets/settings/default.json)来禁用 AI 和遥测功能。
- 受上述因素影响，Zed 目前与沙盒环境存在兼容性问题，[详见此处](https://github.com/zed-industries/zed/pull/12006#issuecomment-2130421220)。

## Flatpak

> 当前 Zed 的 Flatpak 集成会在启动时退出沙箱环境。依赖 Flatpak 沙箱功能的工作流程可能无法按预期运行。

请按以下步骤在本地构建并安装 Flatpak 软件包：

1. 根据[此指南](https://flathub.org/setup)为您的发行版安装 Flatpak
2. 运行 `script/flatpak/deps` 脚本安装必要依赖
3. 执行 `script/flatpak/bundle-flatpak` 命令
4. 此时软件包已完成安装，可在 `target/release/{app-id}.flatpak` 路径获取安装包

## 内存分析

`heaptrack`(https://github.com/KDE/heaptrack) 是诊断内存泄漏的实用工具。安装方法：

```sh
$ sudo apt install heaptrack heaptrack-gui
$ cargo install cargo-heaptrack
```

若需在构建运行 Zed 时启用性能分析器：

```sh
$ cargo heaptrack -b zed
```

当此zed实例退出时，终端输出将包含运行`heaptrack_interpret`的命令，以将`*.raw.zst`配置文件转换为`*.zst`文件，该文件可传递给`heaptrack_gui`进行查看。

## 故障排除

### Cargo错误提示依赖项使用了不稳定功能

尝试`cargo clean`和`cargo build`。