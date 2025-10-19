# 使用调试器

> **免责声明**：本文档并非 [配置 Zed 调试器](../debugger.md) 的说明文档，而是旨在为 Zed 员工和外部贡献者提供在开发 Zed 时如何使用调试器的相关信息。

## 使用 Zed 内置调试器

当 Zed 项目处于打开状态时，您可以打开 `New Process Modal` 并选择 `Debug` 标签页。在此处可以看到用于调试 Zed 的调试配置，其中包含 GDB 和 LLDB 两种配置选项。选择所需配置后，Zed 将自动构建并启动二进制文件。

请注意，arm 架构的 Macbook 不支持 GDB 调试器

## 发行版构建配置注意事项

默认情况下，使用发行版配置（该配置用于生产环境构建，即正式版、预览版和稳定版）构建的程序包含有限的调试信息。

这是通过将根目录 `Cargo.toml` 文件中的 `profile.(release).debug` 字段设置为 `"limited"` 来实现的。

`debug` 字段的官方文档可在[此处](https://doc.rust-lang.org/cargo/reference/profiles.html#debug)查阅。
简单来说，`"limited"` 会剥离类型和变量层级的调试信息。

在发布版本中，这样做是为了减小二进制文件体积，因为类型和变量层级的调试信息并非必需，且不会影响生成栈追踪的可用性。

但需要注意的是，虽然优质栈追踪不需要类型和变量层级的调试信息，这些信息对于获得良好的调试器使用体验却至关重要。
若缺少类型和变量层级的调试信息，调试器将无法解析局部变量、检查变量值或通过美化打印功能格式化显示变量内容。

因此，若要在调试发布版本时充分发挥调试器功能，您必须重新编译Zed二进制文件，并保留完整的调试信息。

最简单的方法是，在运行`cargo run`或`cargo build`时，使用`--config`标志来覆盖根目录`Cargo.toml`文件中的`debug`字段，如下所示：

```sh
cargo run --config 'profile.release.debug="full"'
cargo build --config 'profile.release.debug="full"'
```

> 如果希望避免每次调用`cargo`时都传递`--config`标志，你也可以修改[根目录`Cargo.toml`](https://github.com/zed-industries/zed/blob/main/Cargo.toml)中的相关部分：
>
> 将
>
> ```toml
> [profile.release]
> debug = "limited"
> ```
>
> 改为
>
> ```toml
> [profile.release]
> debug = "full"
> ```
>
> 这将确保所有`cargo run --release`或`cargo build --release`的调用都会编译完整的调试信息。
>
> **警告：** 请确保不要提交这些更改！

## 使用 Shell 调试器 GDB/LLDB 运行 Zed

### 背景

通过rustup安装Rust时（这是开发Zed时的推荐安装方式，具体平台入门指南请参阅[此处](../development.md)文档），系统会额外安装几个辅助脚本并添加到环境变量中，这些脚本用于调试Rust编译的二进制文件。

它们分别是`rust-gdb`和`rust-lldb`。

若感兴趣，您可在此处了解这些脚本的详细信息及其作用原理：https://michaelwoerister.github.io/2015/03/27/rust-xxdb.html

简而言之，这些是封装了标准`gdb`和`lldb`命令的简易shell脚本，通过注入相关命令和标志来启用Rust专属功能，例如美化打印和类型信息显示。

因此，要使用`rust-gdb`或`rust-lldb`，您的系统必须已安装`gdb`或`lldb`。若尚未安装，请根据您的操作系统平台选择合适的方式进行安装。

根据[之前链接的文章](https://michaelwoerister.github.io/2015/03/27/rust-xxdb.html)所述，“最低支持的调试器版本为GDB 7.7和LLDB 310。但通用原则是：版本越新越好。”因此，建议尽可能安装最新版本的`gdb`或`lldb`。

> **注意**：`rust-gdb`在Windows上默认不安装，因为`gdb`对Windows的支持尚不稳定。建议在Windows上改用`lldb`配合`rust-lldb`进行调试。

如果您不熟悉`gdb`或`lldb`，可分别通过[此链接](https://www.gnu.org/software/gdb/)和[此链接](https://lldb.llvm.org/)了解更多信息。

### Zed调试指南

完成上述编译Zed时包含完整调试信息的步骤后，您可以通过以下任一命令，对使用`cargo build`编译的Zed二进制文件运行`rust-gdb`或`rust-lldb`调试：

```
rust-gdb target/debug/zed
rust-lldb target/debug/zed
```

或者，您可以通过运行以下任一命令附加到正在运行的 Zed 实例（例如通过 `cargo run` 启动的 Zed 实例）：

```
rust-gdb -p <pid>
rust-lldb -p <pid>
```

其中 `<pid>` 表示您要附加的 Zed 实例的进程 ID。

要获取正在运行的 Zed 实例的进程 ID，您可以使用系统进程管理工具，例如 Windows 系统的 `Task Manager` 或 macOS 系统的 `Activity Monitor`。

此外，您也可以在 macOS 和 Linux 上运行 `ps aux | grep zed` 命令，或在 Windows 的 PowerShell 实例中运行 `Get-Process | Select-Object Id, ProcessName`。

#### 调试崩溃与异常

调试器是诊断所有程序（包括 Zed）中崩溃和异常原因的强大工具。

默认情况下，当附加了 `gdb` 或 `lldb` 的进程遇到异常（例如崩溃）时，调试器会自动在异常发生点暂停，并允许您检查程序状态。

调试器最可能停止的位置会深入 Rust 标准库的 panic 或异常处理代码中，因此您需要沿着堆栈回溯来定位 panic 的实际原因。

这可以通过在 `lldb` 中使用 `backtrace` 命令结合 `frame select` 命令来实现，`gdb` 中也有类似的命令可用。

程序停止后，您将无法像在异常触发前那样继续执行。但您可以跳转到不同的堆栈帧，并检查每个帧中的变量和表达式的值，这对于确定崩溃的根本原因非常有帮助。

有关调试 Zed 崩溃的更多信息，请参阅[此文档](./debugging-crashes.md)。