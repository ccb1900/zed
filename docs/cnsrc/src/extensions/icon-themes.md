# 图标主题

扩展程序可以提供图标主题，用于更改 Zed 中文件夹和文件所使用的图标。

## 示例扩展

[Material 图标主题](https://github.com/zed-extensions/material-icon-theme)可作为包含图标主题的扩展程序结构范例。

## 目录结构

图标主题扩展包含两个重要目录：

- `icon_themes`：该目录包含一个或多个定义图标主题的 JSON 文件
- `icons`：该目录包含随扩展分发的图标资源文件。您可以根据需要在此目录中创建子目录

每个图标主题文件都应遵循 `https://zed.dev/schema/icon_themes/v0.3.0.json`(https://zed.dev/schema/icon_themes/v0.3.0.json) 中指定的 JSON 模式规范。

以下是图标主题结构的示例：

```json [icon-theme]
{
  "$schema": "https://zed.dev/schema/icon_themes/v0.3.0.json",
  "name": "My Icon Theme",
  "author": "Your Name",
  "themes": [
    {
      "name": "My Icon Theme",
      "appearance": "dark",
      "directory_icons": {
        "collapsed": "./icons/folder.svg",
        "expanded": "./icons/folder-open.svg"
      },
      "named_directory_icons": {
        "stylesheets": {
          "collapsed": "./icons/folder-stylesheets.svg",
          "expanded": "./icons/folder-stylesheets-open.svg"
        }
      },
      "chevron_icons": {
        "collapsed": "./icons/chevron-right.svg",
        "expanded": "./icons/chevron-down.svg"
      },
      "file_stems": {
        "Makefile": "make"
      },
      "file_suffixes": {
        "mp3": "audio",
        "rs": "rust"
      },
      "file_icons": {
        "audio": { "path": "./icons/audio.svg" },
        "default": { "path": "./icons/file.svg" },
        "make": { "path": "./icons/make.svg" },
        "rust": { "path": "./icons/rust.svg" }
        // ...
      }
    }
  ]
}
```

每个图标路径都是相对于扩展目录的根目录进行解析的。

在这个示例中，扩展的目录结构如下所示：

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