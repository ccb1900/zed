# 在 Windows 上构建 Zed

> 以下命令可在任意终端中执行。

## 代码仓库

克隆 [Zed 代码仓库](https://github.com/zed-industries/zed)。

## 依赖项

- 安装 [rustup](https://www.rust-lang.org/tools/install)

- 安装 [Visual Studio](https://visualstudio.microsoft.com/downloads/)，并勾选可选组件 `MSVC v*** - VS YYYY C++ x64/x86 build tools` 和 `MSVC v*** - VS YYYY C++ x64/x86 Spectre-mitigated libs (latest)`（`v***` 对应你的 VS 版本，`YYYY` 对应 VS 发布年份。注意架构类型，必要时根据你的系统调整）。
- 若希望仅安装精简版 MSVC 编译器工具，可安装 [生成工具](https://visualstudio.microsoft.com/visual-cpp-build-tools/)（需额外添加上述库文件）并勾选“使用 C++ 的桌面开发”工作负载。
  但请注意：此安装方式不会被 rustup 自动识别。你需要先通过开始菜单或 Windows 终端启动该安装包提供的“开发人员”命令提示符（cmd/PowerShell）初始化环境变量，再进行编译。
- 根据系统版本安装 Windows 11 或 10 SDK，确保设备上至少安装 `Windows 10 SDK version 2104 (10.0.20348.0)` 版本。可从 [Windows SDK 存档](https://developer.microsoft.com/windows/downloads/windows-sdk/)下载
- 安装 [CMake](https://cmake.org/download)（为 [某个依赖项](https://docs.rs/wasmtime-c-api-impl/latest/wasmtime_c_api/)所需）。也可通过 Visual Studio 安装器安装，然后手动将 `bin` 目录添加至 `PATH`，例如：`C:\Program Files\Microsoft Visual Studio\2022\Community\Common7\IDE\CommonExtensions\Microsoft\CMake\CMake\bin`

如果无法编译Zed，请确保在安装Visual Studio的情况下至少安装了以下组件：

```json [settings]
{
  "version": "1.0",
  "components": [
    "Microsoft.VisualStudio.Component.CoreEditor",
    "Microsoft.VisualStudio.Workload.CoreEditor",
    "Microsoft.VisualStudio.Component.VC.Tools.x86.x64",
    "Microsoft.VisualStudio.ComponentGroup.WebToolsExtensions.CMake",
    "Microsoft.VisualStudio.Component.VC.CMake.Project",
    "Microsoft.VisualStudio.Component.Windows11SDK.26100",
    "Microsoft.VisualStudio.Component.VC.Runtimes.x86.x64.Spectre"
  ],
  "extensions": []
}
```

如果仅安装Build Tools，则需包含以下组件：

该列表可按以下方式获取：

- 打开 Visual Studio 安装程序  
- 在 `Installed` 选项卡中点击 `More`  
- 点击 `Export configuration`  

### 后端依赖项（可选） {#backend-dependencies}

若需使用本地协作服务器开发 Zed 协作功能，请参阅：[本地协作](./local-collaboration.md) 文档。

### 注意事项

需修改 `data` 目录下的 `pg_hba.conf` 文件，将 `host` 方法中的 `scram-sha-256` 替换为 `trust`，否则将出现 `password authentication failed` 错误导致连接失败。`pg_hba.conf` 文件通常位于 `C:\Program Files\PostgreSQL\17\data\pg_hba.conf`。修改后的文件内容应如下所示：

```conf
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
# IPv6 local connections:
host    all             all             ::1/128                 trust
```

此外，若您使用的是非拉丁版本的Windows系统，必须将`data`目录下`postgresql.conf`文件中的`lc_messages`参数修改为`English_United States.1252`（或您当前使用的任意UTF8兼容编码）。否则数据库将出现异常。修改后的`postgresql.conf`文件应如下所示：

```conf
# lc_messages = 'Chinese (Simplified)_China.936' # locale for system error message strings
lc_messages = 'English_United States.1252'
```

完成此操作后，请重启`postgresql`服务。按下`win`键+`R`键调出`Run`窗口，输入`services.msc`并点击`OK`按钮打开服务管理器。找到`postgresql-x64-XX`服务后右键点击，选择`Restart`选项。

## 从源码构建

安装完依赖项后，您可以使用[Cargo](https://doc.rust-lang.org/cargo/)构建Zed。

调试版本构建：

```sh
cargo run
```

发行版本构建：

```sh
cargo run --release
```

运行测试套件：

## 通过 MSYS2 安装

Zed 不支持为 Mingw-w64 构建的非官方 MSYS2 Zed 软件包。若在使用 [mingw-w64-zed](https://packages.msys2.org/base/mingw-w64-zed) 过程中遇到任何问题，请向 [msys2/MINGW-packages/issues](https://github.com/msys2/MINGW-packages/issues?q=is%3Aissue+is%3Aopen+zed) 提交报告。

请先参阅 [MSYS2 文档](https://www.msys2.org/docs/ides-editors/#zed)。

## 故障排除

### 设置 `RUSTFLAGS` 环境变量会导致构建失败

若设置了 `RUSTFLAGS` 环境变量，它将覆盖 `.cargo/config.toml` 中的 `rustflags` 设置，而这些设置是正确构建 Zed 所必需的。

由于这些设置可能随时间变化，您遇到的构建错误可能包括链接器错误或其他更奇怪的错误。

若需添加额外的 Rust 标志，您可以在 `.cargo/config.toml` 中执行以下操作之一：

在构建部分添加您的标志

在 Windows 目标平台部分添加你的编译选项

或者，你可以在 Zed 代码库的同级目录下新建一个配置文件（如下所示）。这种方式在进行持续集成构建时特别有用，因为你无需修改原始的配置文件。

在新的配置文件中，如果我们想要将特定编译选项添加到 rustflags 中，配置内容将如下所示

### 依赖项使用不稳定功能导致的 Cargo 错误

尝试 `cargo clean` 和 `cargo build`。

### `STATUS_ACCESS_VIOLATION`

如果使用 "rust-lld.exe" 链接器可能出现此错误。建议尝试更换其他链接器。

若使用全局配置，可将 Zed 代码库移至嵌套目录，并在父目录中添加 `.cargo/config.toml` 文件并配置自定义链接器。

更多信息请参阅此议题 [#12041](https://github.com/zed-industries/zed/issues/12041)

### 选择了无效的 RC 路径

根据笔记本电脑的安全策略设置，编译 Zed 时可能遇到以下错误：

```
error: failed to run custom build command for `zed(C:\Users\USER\src\zed\crates\zed)`

原因：
进程未成功退出：`C:\Users\USER\src\zed\target\debug\build\zed-b24f1e9300107efc\build-script-build`（退出代码：1）
--- 标准输出
cargo:rerun-if-changed=../../.git/logs/HEAD
cargo:rustc-env=ZED_COMMIT_SHA=25e2e9c6727ba9b77415588cfa11fd969612adb7
cargo:rustc-link-arg=/stack:8388608
cargo:rerun-if-changed=resources/windows/app-icon.ico
package.metadata.winresource 不存在
已选择 RC 路径：'bin\x64\rc.exe'

--- 标准错误
系统找不到指定的路径。（操作系统错误 3）
警告：构建失败，正在等待其他任务完成...
```

In order to fix this issue, you can manually set the `ZED_RC_TOOLKIT_PATH` environment variable to the RC toolkit path. Usually, you can set it to:
`C:\Program Files (x86)\Windows Kits\10\bin\<SDK_version>\x64`。

更多信息请参阅此[问题](https://github.com/zed-industries/zed/issues/18393)。

### 构建失败：路径过长

构建时可能出现如下错误：

为了解决这个问题，你可以为 Git 和 Windows 启用长路径支持。

对于 Git：`git config --system core.longpaths true`

对于 Windows，使用以下 PowerShell 命令：

```powershell
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```

如需了解更多信息，请参阅 [win32 文档](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=powershell)

（注意：启用长路径支持后需要重启系统）

### 图形问题

#### Zed 启动失败

目前 Zed 在 Windows 系统使用 Vulkan 作为图形接口。但由于 Vulkan 在 Windows 平台的稳定性存在波动，若 Zed 无法启动，很可能是 Vulkan 相关的问题。

可通过以下路径查看 Zed 日志：
`C:\Users\YOU\AppData\Local\Zed\logs\Zed.log`

若出现以下提示：

- `Zed failed to open a window: NoSupportedDeviceFound`
- `ERROR_INITIALIZATION_FAILED`
- `GPU Crashed`
- `ERROR_SURFACE_LOST_KHR`

则说明 Vulkan 在您系统中运行异常。多数情况下，更新显卡驱动可解决此问题。

若日志中未出现 Vulkan 相关报错，且您安装了 Bandicam，请尝试卸载该软件。当前 Zed 与 Bandicam 存在兼容性问题。