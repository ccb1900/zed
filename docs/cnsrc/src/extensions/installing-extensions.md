# 安装扩展

您可以通过以下方式搜索扩展：按下 {#kb zed::Extensions} 启动 Zed 扩展商店、打开命令面板并选择 {#action zed::Extensions}，或从菜单栏选择 "Zed > Extensions"。

在此您可以查看当前已安装的扩展，或搜索并安装新扩展。

## 安装位置

- 在 macOS 系统中，扩展将安装在 `~/Library/Application Support/Zed/extensions` 目录。
- 在 Linux 系统中，扩展将安装在 `$XDG_DATA_HOME/zed/extensions` 或 `~/.local/share/zed/extensions` 目录。

该目录包含两个子目录：

- `installed`：存放各扩展的源代码
- `work`：存放扩展自行创建的文件（如下载的语言服务器）

## 自动安装

如需实现扩展的自动安装/卸载功能，请参阅 [auto_install_extensions](../configuring-zed.md#auto-install-extensions) 配置文档。