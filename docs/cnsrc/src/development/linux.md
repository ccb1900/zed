# 为 Linux 构建 Zed

## 代码仓库

克隆 [Zed 代码仓库](https://github.com/zed-industries/zed)。

## 依赖项

- 安装 [rustup](https://www.rust-lang.org/tools/install)

- 安装必要的系统库：

  ```sh
  script/linux
  ```

  如果您希望手动安装系统库，可以在 `script/linux` 文件中找到所需软件包列表。

### 后端依赖项（可选）{#backend-dependencies}

如果您希望使用本地协作服务器开发 Zed 协作功能，请参阅：[本地协作](./local-collaboration.md) 文档。

### 链接器 {#linker}

在 Linux 系统上，Rust 的默认链接器是 [LLVM 的 `lld`](https://blog.rust-lang.org/2025/09/18/Rust-1.90.0/)。使用替代链接器，特别是 [Wild](https://github.com/davidlattimore/wild) 和 [Mold](https://github.com/rui314/mold)，可以显著提升全新构建和增量构建的速度。

目前Zed在持续集成中使用Mold，因为它更为成熟。对于本地开发，推荐使用Wild，因为它比Mold快5%到20%。

这些链接器可以通过`script/install-mold`和`script/install-wild`安装。

要将Wild设为默认链接器，请在`~/.cargo/config.toml`中添加以下内容：

```toml
[target.x86_64-unknown-linux-gnu]
linker = "clang"
rustflags = ["-C", "link-arg=--ld-path=wild"]

[target.aarch64-unknown-linux-gnu]
linker = "clang"
rustflags = ["-C", "link-arg=--ld-path=wild"]
```

要将Mold设为默认链接器：

```toml
[target.'cfg(target_os = "linux")']
rustflags = ["-C", "link-arg=-fuse-ld=mold"]
```

## 从源码构建

安装完依赖项后，您可以使用[Cargo](https://doc.rust-lang.org/cargo/)构建Zed。

构建编辑器调试版本：

```sh
cargo run
```

运行测试：

```sh
cargo test --workspace
```

在发布模式下，主要用户界面是`cli`包。您可以通过以下命令在开发环境中运行：

```sh
cargo run -p cli
```

## 安装开发版本

您可以通过以下命令在本地计算机上安装构建版本：

```sh
./script/install-linux
```

该命令将以发布模式构建zed和cli，并将其安装到`~/.local/bin/zed`路径，同时将.desktop文件安装到`~/.local/share`目录。

> **_注意_**: 如果遇到类似以下链接器错误：

> ```bash
> error: linking with `cc` failed: exit status: 1 ...
> = note: /usr/bin/ld: /tmp/rustcISMaod/libaws_lc_sys-79f08eb6d32e546e.rlib(f8e4fd781484bd36-bcm.o): in function `aws_lc_0_25_0_handle_cpu_env':
>           /aws-lc/crypto/fipsmodule/cpucap/cpu_intel.c:(.text.aws_lc_0_25_0_handle_cpu_env+0x63): undefined reference to `__isoc23_sscanf'
>           /usr/bin/ld: /tmp/rustcISMaod/libaws_lc_sys-79f08eb6d32e546e.rlib(f8e4fd781484bd36-bcm.o): in function `pkey_rsa_ctrl_str':
>           /aws-lc/crypto/fipsmodule/evp/p_rsa.c:741:(.text.pkey_rsa_ctrl_str+0x20d): undefined reference to `__isoc23_strtol'
>           /usr/bin/ld: /aws-lc/crypto/fipsmodule/evp/p_rsa.c:752:(.text.pkey_rsa_ctrl_str+0x258): undefined reference to `__isoc23_strtol'
>           collect2: error: ld returned 1 exit status
>   = note: some `extern` functions couldn't be found; some native libraries may need to be installed or have their path specified
>   = note: use the `-l` flag to specify native libraries to link
>   = note: use the `cargo:rustc-link-lib` directive to specify the native libraries to link with Cargo (see https://doc.rust-lang.org/cargo/reference/build-scripts.html#rustc-link-lib)
> error: could not compile `remote_server` (bin "remote_server") due to 1 previous error
> ```

> **原因**：
> 这是由 aws-lc-rs 的已知缺陷导致（不支持 GCC >= 14）：[FIPS 无法使用 GCC >= 14 构建](https://github.com/aws/aws-lc-rs/issues/569)
> 与 [GCC-14 - FIPS 模块构建失败](https://github.com/aws/aws-lc/issues/2010)

> 更多信息可参考：[linux: 使用 script/install-linux 时 remote_server 出现链接器错误](https://github.com/zed-industries/zed/issues/24880)

> **临时解决方案**：
> 将远程服务器目标设置为 `x86_64-unknown-linux-gnu`，具体操作如下 `export REMOTE_SERVER_TARGET=x86_64-unknown-linux-gnu; script/install-linux`

## Wayland 与 X11

Zed 同时支持 X11 和 Wayland 显示协议。默认情况下，系统会在运行时自动选择可用的显示协议。若您在 Wayland 环境下希望切换至 X11 模式，请设置环境变量 `WAYLAND_DISPLAY=''`。

## Zed 软件打包指南

感谢您承担 Zed 的打包工作！

### 技术要求

Zed 包含两个主要可执行文件：

- 你需要构建`crates/cli`，并将其二进制文件以`zed`的名称存放在`$PATH`路径下。
- 你需要构建`crates/zed`，并将其放置在`$PATH/to/cli/../../libexec/zed-editor`路径。例如，若计划将命令行工具放在`~/.local/bin/zed`，则应将zed置于`~/.local/libexec/zed-editor`。由于部分Linux发行版（特别是Arch）不推荐使用`libexec`，你也可以将此二进制文件改为存放在`$PATH/to/cli/../../lib/zed/zed-editor`（例如`~/.local/lib/zed/zed-editor`）。
- 如需提供`.desktop`文件，可在`crates/zed/resources/zed.desktop.in`中找到模板，并使用`envsubst`填入所需数值。该文件需重命名为`$APP_ID.desktop`以符合FreeDesktop标准。同时请确保此桌面文件设置为可执行（`chmod 755`）。
- 需要确保已安装必要的依赖库。可通过在系统中检查已构建的二进制文件获取当前依赖列表。
- 完整构建脚本示例可参考script/bundle-linux。
- 可通过设置环境变量`ZED_UPDATE_EXPLANATION`来禁用Zed的自动更新，并为尝试手动更新Zed的用户提供指引。例如：`ZED_UPDATE_EXPLANATION="Please use flatpak to update zed."`。
- 请确保将`crates/zed/RELEASE_CHANNEL`文件内容更新为不含换行符的'nightly'、'preview'或'stable'。这将使Zed使用凭证管理器来记录用户登录状态。

### 其他注意事项

在Zed，我们的首要任务是快速迭代，将最新技术带给用户。长久以来，我们一直对运行缓慢、版本过时或配置繁琐的软件感到困扰，因此我们基于这些痛点打造了这款编辑器。

但我们也深知，许多Linux发行版有着不同的优先考量。我们期待与各方合作，将Zed推向更多用户青睐的平台。不过当前仍任重道远：

- Zed 是一个快速迭代的早期项目。我们通常每周发布 2-3 个版本来修复用户反馈的问题并推出主要功能。
- Linux 系统上可能存在其他 `zed` 二进制文件（[1](https://openzfs.github.io/openzfs-docs/man/v2.2/8/zed.8.html)、[2](https://zed.brimdata.io/docs/commands/zed)）。若因命名冲突需要重命名我们的 CLI 二进制文件，建议使用 `zedit`、`zeditor` 或 `zed-cli`。
- Zed 会自动安装正确版本的常用开发工具，其机制类似于 rustup/rbenv/pyenv 等。我们理解这种做法存在争议，[详见此处](https://github.com/zed-industries/zed/issues/12589)。
- 我们允许用户安装本地扩展和来自 [zed-industries/extensions](https://github.com/zed-industries/extensions) 的扩展。这些扩展可能会根据需要安装更多工具（例如语言服务器）。长期来看，我们希望提升此过程的安全性，[详见此处](https://github.com/zed-industries/zed/issues/12358)。
- Zed 默认会连接多个在线服务（AI、遥测、协作功能）。用户可通过 Zed 设置或修改我们的[默认配置文件](https://github.com/zed-industries/zed/blob/main/assets/settings/default.json)来禁用 AI 和遥测功能。
- 受上述因素影响，Zed 目前与沙盒环境的兼容性不佳，[详见此处](https://github.com/zed-industries/zed/pull/12006#issuecomment-2130421220)。

## Flatpak

> Zed 当前的 Flatpak 集成会在启动时退出沙箱。依赖 Flatpak 沙箱功能的工作流程可能无法按预期运行。

如需在本地构建并安装 Flatpak 软件包，请按以下步骤操作：

1. 根据[此指南](https://flathub.org/setup)为您的发行版安装 Flatpak
2. 运行 `script/flatpak/deps` 脚本以安装所需依赖项
3. 执行 `script/flatpak/bundle-flatpak`
4. 此时软件包已完成安装，可在 `target/release/{app-id}.flatpak` 获取可用套件

## 内存分析

[`heaptrack`](https://github.com/KDE/heaptrack) 对诊断内存泄漏非常有用。安装方法：

```sh
$ sudo apt install heaptrack heaptrack-gui
$ cargo install cargo-heaptrack
```

随后通过以下命令在附加分析器的情况下构建并运行 Zed：

```sh
$ cargo heaptrack -b zed
```

当此zed实例退出时，终端输出将包含运行`heaptrack_interpret`的命令，以将`*.raw.zst`配置文件转换为`*.zst`文件，该文件可传递给`heaptrack_gui`进行查看。

## 故障排除

### Cargo报错称依赖项使用了不稳定功能

尝试`cargo clean`和`cargo build`。