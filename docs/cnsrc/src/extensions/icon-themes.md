# 图标主题

扩展程序可以提供图标主题，用于更改 Zed 中文件夹和文件所使用的图标。

## 示例扩展

[Material 图标主题](https://github.com/zed-extensions/material-icon-theme)可作为包含图标主题的扩展程序结构范例。

## 目录结构

图标主题扩展包含两个重要目录：

- `themes`：该目录包含一个或多个定义图标主题的 JSON 文件
- `icons`：该目录包含随扩展分发的图标资源文件。您可以根据需要在此目录中创建子目录

每个图标主题文件都应遵循 `https://zed.dev/schema/icon_themes/v0.3.0.json` 指定的 JSON 架构规范。

以下是图标主题结构的示例：

[[代码块_0]]

每个图标路径都是相对于扩展根目录解析的。

在此示例中，扩展的文件结构如下所示：

```
extension.toml
icon_themes/
  my-icon-theme.json
icons/
  audio.svg
  chevron-down.svg
  chevron-right.svg
  file.svg
  folder-open.svg
  folder.svg
  rust.svg
```