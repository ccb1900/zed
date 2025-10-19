# Zed 在 Windows 系统

## 安装 Zed

通过[下载页面](https://zed.dev/download)获取最新稳定版本。如需下载预览版本，请访问其[发布页面](https://zed.dev/releases/preview)。首次手动安装后，Zed 将定期检查并安装更新。

您也可以从源码构建 Zed，具体操作说明请参阅[相关文档](https://zed.dev/docs/development/windows)。

## 卸载程序

- 通过安装程序安装：使用`Settings` → `Apps` → `Installed apps`，搜索 Zed 并点击卸载。
- 从源码构建：删除您创建的构建输出目录（例如您的 target/install 文件夹）。

您的设置和扩展插件存储于用户配置文件中。卸载时，您可以选择保留或删除这些数据。

## 远程开发 (SSH)

Zed 支持 Windows 系统的 SSH 远程连接，并在需要时提示输入凭据。

如果遇到身份验证问题，请确认您的SSH密钥代理正在运行（例如ssh-agent或Git客户端的代理），并确保ssh.exe位于PATH环境变量中。

## WSL支持

Zed原生支持在WSL内直接打开文件夹。

若要在WSL容器中打开本地文件夹，请使用`projects: open in wsl`操作并选择目标文件夹，系统将显示可用WSL发行版列表供您选择。

若要打开已位于WSL容器内的文件夹，请使用`projects: open wsl`操作并选择WSL发行版，该发行版将添加到`Remote Projects`窗口，您即可在此打开文件夹，详见[远程开发](./remote-development.md)

## 故障排除

### Zed启动失败或显示空白窗口

- 从您的显卡供应商（Intel/AMD/NVIDIA）更新显卡驱动程序。
- 确保在Windows中启用了硬件加速，且未被第三方软件阻止。
- 尝试在不加载扩展或自定义设置的情况下启动Zed，以排查冲突。

### 终端问题

如果激活脚本未运行，请更新至最新版本，并检查您的Shell配置文件是否提前退出。对于Git操作，请确认Git Bash或PowerShell可用且在PATH中。

### SSH远程连接问题

当提示输入凭据时，请使用图形化askpass对话框。如果未显示，请检查凭据管理器是否存在冲突，并确保终端未阻止图形界面提示。

### 图形问题

#### Zed无法启动/性能下降

Zed需要兼容DX11的显卡才能运行。如果无法启动，可能是您的显卡未达到最低要求。

要检查您的显卡是否支持DX11，可使用以下命令：

```
dxdiag
```

这将打开诊断工具，在`System` → `System Information` → `DirectX Version`下显示您的GPU支持的最低DirectX版本。

您可能也尝试在虚拟机中运行Zed，这种情况下它将使用虚拟机提供的模拟适配器，虽然Zed可以运行，但性能会有所下降。