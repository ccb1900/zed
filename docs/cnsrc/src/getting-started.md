# 快速开始

欢迎使用 Zed！我们非常高兴您的加入。以下是为您准备的入门指南。

## 下载 Zed

### macOS 系统

通过[下载页面](https://zed.dev/download)获取最新稳定版本。如需下载预览版本，请访问[发布页面](https://zed.dev/releases/preview)。首次手动安装后，Zed 将自动定期检查更新。

您也可以通过 Homebrew 安装稳定版：

[[代码块_0]]

以及预览版：

[[代码块_1]]

### Windows 系统

通过[下载页面](https://zed.dev/download)获取最新稳定版本。如需下载预览版本，请访问[发布页面](https://zed.dev/releases/preview)。首次手动安装后，Zed 将自动定期检查更新。

### Linux 系统

对于大多数 Linux 用户，最简单的安装方式是通过我们的安装脚本：

如果你想帮助我们测试新功能，也可以安装我们的预览版本：

```sh
curl -f https://zed.dev/install.sh | ZED_CHANNEL=preview sh
```

该脚本支持 `x86_64` 和 `AArch64`，以及常见的 Linux 发行版：Ubuntu、Arch、Debian、RedHat、CentOS、Fedora 等。

如果 Zed 是通过此安装脚本安装的，你可以随时运行 shell 命令 `zed --uninstall` 来卸载。随后，shell 会提示你是否保留偏好设置或删除它们。做出选择后，你应该会看到 Zed 已成功卸载的消息。

如果此脚本无法满足你的使用需求，你在运行 Zed 时遇到问题，或者在卸载 Zed 时出现错误，请参阅我们的 [Linux 专用文档](./linux.md)。

## 命令面板

命令面板是访问Zed中几乎所有功能的主要方式。其快捷键绑定是您首先应该熟悉的操作。要打开它，请按下：{#kb command_palette::Toggle}。

![已打开的命令面板](https://zed.dev/img/features/command-palette.jpg)

试试看！打开命令面板并输入`new file`。您会看到命令列表被筛选至`workspace: new file`。按回车键即可创建一个新缓冲区。

当您看到包含`zed: ...`或`editor: ...`等形式命令的说明时，即表示您需要在命令面板中执行这些操作。

## 命令行界面

Zed 提供命令行界面，在 Linux 系统中该功能通常随发行版的 Zed 软件包提供（不同发行版的二进制文件名称可能有所差异，后文为简洁起见将统一使用 `zed` 指代）。
在 macOS 系统上，命令行工具与编辑器二进制文件捆绑在同一安装包中，可通过执行 `cli: install` 命令将 Zed 集成至系统环境，该操作会创建指向 `/usr/local/bin/zed` 的符号链接。
用户亦可从代码库中的 `cli` 组件自行编译构建命令行工具。

执行 `zed --help` 可查看完整功能列表。
核心功能概览：

- 开启新空白 Zed 窗口：`zed`

- 在 Zed 中打开文件或目录：`zed /path/to/entry`（添加 `-n` 参数可在新窗口打开）

- 从标准输入读取内容：`ps axf | zed -`

- 在终端显示日志的情况下启动 Zed：`zed --foreground`

- 卸载 Zed 及其所有相关文件：`zed --uninstall`

## 配置 Zed

要打开自定义设置来调整字体、格式设置、按语言设置等内容，请使用 {#kb zed::OpenSettings} 快捷键。

要查看所有可用设置，请使用 {#kb command_palette::Toggle} 打开命令面板并搜索 `zed: open default settings`。
您也可以在 [配置 Zed](./configuring-zed.md) 文档中查看所有设置。

## 配置 Zed 中的 AI 功能

Zed 在编辑器中通过多种方式无缝集成大语言模型。
访问 [AI 功能概览页](./ai/overview.md) 了解如何在 Zed 中快速开始使用大语言模型。

## 设置快捷键

要编辑自定义快捷键映射并添加或重新映射快捷键，您可以使用 {#kb zed::OpenKeymap} 启动 Zed 快捷键编辑器 ({#action zed::OpenKeymap})，或直接通过 {#action zed::OpenKeymap} 打开 Zed 快捷键 JSON 文件 (`~/.config/zed/keymap.json`)。

要访问默认按键绑定设置，请使用 {#kb command_palette::Toggle} 打开命令面板，然后搜索“zed: 打开默认按键映射”。更多信息请参阅[按键绑定](./key-bindings.md)。