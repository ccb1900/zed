# 远程开发

远程开发功能让你即使代码库不在本地设备上，也能保持思维同步的编码速度。你在本地运行Zed界面确保即时响应，同时将繁重计算任务交由开发服务器处理，从而实现高效工作。

## 功能概览

远程开发需要两台设备协同工作：运行Zed界面的本地机器，以及运行Zed无界面服务器的远程服务器。二者通过SSH协议进行通信，因此使用此功能需确保能从本地设备通过SSH接入远程服务器。

![Zed远程开发架构示意图](https://zed.dev/img/remote-development/diagram.png)

在本地设备上，Zed负责运行用户界面、与语言模型交互、使用Tree-sitter进行代码解析与语法高亮，并存储未保存的更改和近期项目记录。而源代码、语言服务器、任务执行以及终端环境则全部运行在远程服务器上。

> **注意：** 远程开发的初始版本通过 Zed 服务器传输流量。自 Zed v0.157 版本起，该模式已不再可用。

## 设置

1. 下载并安装最新版 [Zed](https://zed.dev/releases)，版本需不低于 v0.159。
1. 使用 {#kb projects::OpenRemote} 快捷键打开“远程项目”对话框。
1. 点击“连接新服务器”，输入用于 SSH 连接服务器的命令。可配置参数请参阅[支持的 SSH 选项](#supported-ssh-options)。
1. 系统将使用本地环境变量路径中的 `ssh` 程序尝试连接远程服务器。若连接成功，Zed 将自动在远程主机下载并启动服务器程序。
1. Zed 服务器启动后，系统将提示选择远程服务器上的路径。
   > **注意：** 当前版本对超大型目录（例如包含超过 10 万个文件的 `/` 或 `~`）的支持尚不完善。我们正在持续优化该功能，建议现阶段仅打开特定项目或大型单体仓库中的子文件夹。

对于无需SSH参数的简单场景，您可以直接运行`zed ssh://[<user>@]<host>[:<port>]/<path>`来打开远程文件夹或文件。若希望通过链接快速访问SSH项目，请使用此格式的链接：`zed://ssh/[<user>@]<host>[:<port>]/<path>`。

## 支持平台

远程主机需能运行Zed服务端程序。以下平台理论上兼容（注意我们尚未对所有Linux发行版进行完整测试）：

- macOS Catalina或更高版本（Intel或Apple Silicon芯片）
- Linux系统（x86_64或arm64架构，暂不支持32位平台）
- Windows平台暂未支持

## 配置说明

远程服务器列表存储于您的设置文件中{#kb zed::OpenSettings}。您可通过"远程项目"对话框{#kb projects::OpenRemote}编辑该列表——此方式更具稳定性，例如在写入配置文件前会验证连接是否可建立。

Zed 会调用您路径上的 `ssh`，因此它将继承您在 `~/.ssh/config` 中为指定主机配置的任何设置。不过，如果您需要覆盖某些配置，可以在每个连接上额外设置以下选项：

```json [settings]
{
  "ssh_connections": [
    {
      "host": "192.168.1.10",
      "projects": [{ "paths": ["~/code/zed/zed"] }],
      // any argument to pass to the ssh master process
      "args": ["-i", "~/.ssh/work_id_file"],
      "port": 22, // defaults to 22
      // defaults to your username on your local machine
      "username": "me"
    }
  ]
}
```

每个连接还有两个 Zed 特有的选项：`upload_binary_over_ssh` 和 `nickname`：

如果你使用命令行通过 `zed ssh://192.168.1.10/~/.vimrc` 连接到主机，那么系统会通过查找与命令行 URL 中的主机/用户名/端口匹配的第一个连接，从你的设置文件中读取额外选项。

此外值得注意的是，虽然您可以在命令行中传递密码`zed ssh://user:password@host/~`，但我们不支持将密码写入配置文件。如需重复连接同一主机，建议配置基于密钥的身份验证。

## 端口转发

若需从本地计算机访问远程服务器的端口，可在配置文件中设置端口转发功能。这对网站开发尤其实用，让您在工作时能通过浏览器直接访问站点。

```json [settings]
{
  "ssh_connections": [
    {
      "host": "192.168.1.10",
      "port_forwards": [{ "local_port": 8080, "remote_port": 80 }]
    }
  ]
}
```

此设置会将本地机器`localhost:8080`的请求转发至远程机器的80端口。该功能底层通过ssh的`-L`参数实现。

默认情况下，这些端口绑定到本地主机，因此与开发计算机在同一网络中的其他计算机无法访问它们。您可以设置 local_host 绑定到不同的接口，例如，0.0.0.0 将绑定到所有本地接口。

```json [settings]
{
  "ssh_connections": [
    {
      "host": "192.168.1.10",
      "port_forwards": [
        {
          "local_port": 8080,
          "remote_port": 80,
          "local_host": "0.0.0.0"
        }
      ]
    }
  ]
}
```

这些端口在远程主机上默认也使用 `localhost` 接口。如果需要更改，您也可以设置远程主机：

```json [settings]
{
  "ssh_connections": [
    {
      "host": "192.168.1.10",
      "port_forwards": [
        {
          "local_port": 8080,
          "remote_port": 80,
          "remote_host": "docker-host"
        }
      ]
    }
  ]
}
```

## Zed 设置

打开远程项目时，有三个相关的设置位置：

- 本地计算机上的本地Zed设置（在macOS上的`~/.zed/settings.json`或Linux上的`~/.config/zed/settings.json`中）。
- 远程服务器上的服务器Zed设置（位于相同位置）。
- 项目设置（在项目的`.zed/settings.json`或`.editorconfig`中）。

本地Zed和服务器Zed都会读取项目设置，但它们不会感知到对方的主要`settings.json`。

根据您想要进行的设置类型，应使用以下设置文件：

- 项目设置应用于影响项目的配置：缩进设置、使用的格式化工具/语言服务器等。
- 服务器设置应用于影响服务器的配置：语言服务器的路径等。
- 本地设置应用于影响用户界面的配置：字体大小等。

此外，您在本地安装的任何扩展都将传播到远程服务器。这意味着语言服务器等将正确运行。

## 初始化远程服务器

一旦您提供了SSH选项，Zed便会调用本地机器上的`ssh`，根据您提供的选项创建ControlMaster连接。

SSH所需的任何提示都将在界面中显示，方便您验证主机密钥、输入密钥密码等。

主连接建立后，Zed会检查远程服务器的`~/.zed_server`目录中是否存在远程服务器二进制文件，并确认其版本与您当前使用的Zed版本是否匹配。

若文件不存在或版本不匹配，Zed将尝试下载最新版本。默认情况下，它会直接从`https://zed.dev`下载；但如果您在该服务器的设置中配置了`{"upload_binary_over_ssh":true}`，则会先将二进制文件下载至本地机器，再上传至远程服务器。

如果您希望自行维护服务器二进制文件，完全可以实现。您可以选择从[GitHub](https://github.com/zed-industries/zed/releases)下载我们预编译的版本，或是通过`cargo build -p remote_server --release`[自行构建](https://zed.dev/docs/development)。采用此方式时，请务必将文件上传至服务器的`~/.zed_server/zed-remote-server-{RELEASE_CHANNEL}-{VERSION}`路径，例如`~/.zed_server/zed-remote-server-stable-0.181.6`。请注意，二进制文件版本必须与您使用的Zed主程序版本完全一致。

## 保持SSH连接

服务器初始化完成后，Zed将创建新的SSH连接（复用现有ControlMaster配置）来运行远程开发服务器。

每个连接都会尝试以代理模式启动开发服务器。该模式会在服务器未运行时自动启动守护进程，并在服务器运行时重新建立连接。通过这种机制，当您的网络连接中断后重新恢复时，可以无缝衔接继续工作而不会中断。

若重连失败，守护进程将不再被复用。不过请放心，未保存的更改默认会存储在本地，您的工作内容不会丢失。您可以随时重新连接项目，Zed将自动恢复未保存的修改内容。

若遇到连接问题，您可以通过Zed日志`cmd-shift-p Open Log`查看详细信息。若发现异常情况，请提交[GitHub工单](https://github.com/zed-industries/zed/issues/new)或通过[Zed Discord](https://zed.dev/community-links)的#remoting-feedback频道联系我们。

## 支持的SSH选项

在底层，Zed通过调用`ssh`二进制文件来连接远程服务器。我们为每个项目创建一个SSH控制主连接，并利用该连接实现多路复用——既承载Zed协议通信，也支持您开启的终端会话和执行的任务。系统会读取您的SSH配置文件参数，若需为SSH控制主连接添加额外选项，可通过Zed配置进行设置。

在"连接新服务器"对话框中输入时，可使用bash风格的引号来传递含空格的参数。成功创建服务器后，该配置将自动加入您设置文件的`"ssh_connections": []`数组中。您也可直接编辑设置文件来调整SSH连接配置。

支持配置的选项包括：

- `-p` / `-l` - 这相当于在主机字符串中传递端口和用户名。
- `-L` / `-R` 用于端口转发
- `-i` - 用于指定密钥文件
- `-o` - 用于设置自定义选项
- `-J` / `-w` - 用于代理 SSH 连接
- `-F` 用于指定 `ssh_config`
- 此外还有... `-4`, `-6`, `-A`, `-B`, `-C`, `-D`, `-I`, `-K`, `-P`, `-X`, `-Y`, `-a`, `-b`, `-c`, `-i`, `-k`, `-l`, `-m`, `-o`, `-p`, `-w`, `-x`, `-y`

请注意，我们有意禁止了某些选项（例如 `-t` 或 `-T`），这些选项将由 Zed 自动为您设置。

## 已知限制

- 无法通过在远程终端中输入 `zed` 命令来打开文件。

## 反馈

请加入 [Zed Discord](https://zed.dev/community-links) 的 #remoting-feedback 频道。