# 入门指南

欢迎使用 Zed！我们非常高兴您的加入。以下是为您准备的快速入门指引。

## 下载 Zed

### macOS 系统

通过[下载页面](https://zed.dev/download)获取最新稳定版本。如需下载预览版本，请访问[预览版发布页面](https://zed.dev/releases/preview)。首次手动安装后，Zed 将定期检查并安装更新。

您也可以通过 Homebrew 安装 Zed 稳定版：

```sh
brew install --cask zed
```

以及 Zed 预览版：

```sh
brew install --cask zed@preview
```

### Windows 系统

通过[下载页面](https://zed.dev/download)获取最新稳定版本。如需下载预览版本，请访问[预览版发布页面](https://zed.dev/releases/preview)。首次手动安装后，Zed 将定期检查并安装更新。

### Linux 系统

对于大多数 Linux 用户，最简单的安装方式是通过我们的安装脚本：

```sh
curl -f https://zed.dev/install.sh | sh
```

若您希望协助我们测试新功能，也可安装预览版本：

```sh
curl -f https://zed.dev/install.sh | ZED_CHANNEL=preview sh
```

本脚本支持`x86_64`与`AArch64`，同时兼容常见Linux发行版：Ubuntu、Arch、Debian、RedHat、CentOS、Fedora等。

若通过此安装脚本安装Zed，可随时执行Shell命令`zed --uninstall`进行卸载。随后终端将询问是否保留偏好设置。完成选择后，您将看到Zed已成功卸载的提示信息。

若此脚本无法满足您的使用需求、运行Zed时遇到问题或卸载过程中出现错误，请参阅我们的[Linux专属文档](./linux.md)。

## 命令面板

命令面板是访问Zed中几乎所有功能的主要方式。它的快捷键是你首先应该熟悉的。要打开它，请按下：{#kb command_palette::Toggle}。

![打开的命令面板](https://zed.dev/img/features/command-palette.jpg)

试试看！打开命令面板并输入`new file`。你会看到命令列表被筛选为`workspace: new file`。按回车键，你将获得一个新缓冲区。

任何时候当你看到包含`zed: ...`或`editor: ...`等形式命令的说明时，这意味着你需要在命令面板中执行它们。

## 命令行界面

Zed 提供命令行界面，在 Linux 系统中该功能通常随发行版的 Zed 软件包提供（不同发行版的二进制文件名可能有所差异，后文为简洁起见将统一使用 `zed` 指代）。
对于 macOS 系统，命令行工具与编辑器二进制文件包含在同一个安装包中，可通过执行 `cli: install` 命令将 Zed 集成至系统环境，该命令会创建指向 `/usr/local/bin/zed` 的符号链接。
用户也可通过编译源码获取命令行工具，相关源码位于本代码库的 `cli` 组件中。

执行 `zed --help` 可查看完整功能列表。
主要功能概览：

- 开启新空白 Zed 窗口：`zed`

- 在 Zed 中打开文件或目录：`zed /path/to/entry`（添加 `-n` 参数可在新窗口打开）

- 从标准输入读取内容：`ps axf | zed -`

- 在终端显示日志的情况下启动 Zed：`zed --foreground`

- 卸载 Zed 及所有相关文件：`zed --uninstall`

## 配置 Zed

要打开自定义设置以调整字体、格式选项、多语言配置等内容，请使用 {#kb zed::OpenSettings} 快捷键。

如需查看所有可用设置，可通过 {#kb command_palette::Toggle} 打开命令面板并搜索 `zed: open default settings`。
您也可以在[配置 Zed](./configuring-zed.md) 文档中查阅完整设置选项。

## 配置 Zed 的 AI 功能

Zed 在编辑器中通过多种方式无缝集成大语言模型。
请访问 [AI 功能概览页](./ai/overview.md) 了解如何在 Zed 中快速启用大语言模型。

## 设置快捷键绑定

要编辑自定义快捷键映射并添加或重新分配绑定，您可以使用以下任一方式：
- 通过 {#kb zed::OpenKeymap} 快捷键启动 Zed 快捷键编辑器 ({#action zed::OpenKeymap})
- 直接使用 {#action zed::OpenKeymap} 打开 Zed 快捷键 JSON 文件 (`~/.config/zed/keymap.json`)

要访问默认按键绑定设置，请使用 {#kb command_palette::Toggle} 打开命令面板，然后搜索“zed: 打开默认按键映射”。更多信息请参阅[按键绑定](./key-bindings.md)。