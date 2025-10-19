# 主题

Zed 自带多种内置主题，还有更多主题可通过扩展获取。

## 选择主题

通过主题选择器查看已安装的主题并预览效果。你可以通过命令面板中的 "theme selector: Toggle"（在 macOS 上快捷键为 `cmd-k cmd-t`，Linux 上为 `ctrl-k ctrl-t`）打开它。

在主题列表中上下移动时，主题会实时切换，按下回车键即可将所选主题保存至设置文件。

## 安装更多主题

更多主题可在扩展页面获取，通过命令面板中的 "zed: Extensions" 或访问 [Zed 官网](https://zed.dev/extensions)即可进入该页面。

许多热门主题已移植到 Zed。若难以抉择，可访问第三方主题库 [zed-themes.com](https://zed-themes.com)，该网站提供了大量主题的可视化预览。

## 配置主题

您选择的主题存储在设置文件中。您可以通过命令面板中的"zed: 打开设置"（macOS系统快捷键为`cmd-,`，Linux系统为`ctrl-,`）打开设置文件。

默认情况下，Zed维护两套主题：一套用于浅色模式，一套用于深色模式。您可以将模式设置为`"dark"`或`"light"`来忽略当前系统模式。

```json [settings]
{
  "theme": {
    "mode": "system",
    "light": "One Light",
    "dark": "One Dark"
  }
}
```

## 主题覆盖设置

如需覆盖主题的特定属性，请使用`experimental.theme_overrides`设置。

例如，若您希望覆盖编辑器的背景颜色并将注释和文档注释显示为斜体，请在`settings.json`中添加以下内容：

```json [settings]
{
  "experimental.theme_overrides": {
    "editor.background": "#333",
    "syntax": {
      "comment": {
        "font_style": "italic"
      },
      "comment.doc": {
        "font_style": "italic"
      }
    }
  }
}
```

要查看完整的捕获列表（如 ``comment`` 和 ``comment.doc``），请参阅：[语言扩展：语法高亮](./extensions/languages.md#syntax-highlighting)。

要查看可用的主题属性列表，请查看您所用主题的 JSON 文件。例如，默认的 One Dark 和 One Light 主题的配置文件位于 [assets/themes/one/one.json](https://github.com/zed-industries/zed/blob/main/assets/themes/one/one.json)。

## 本地主题

将新主题存储在本地，只需将其放置在 ``~/.config/zed/themes`` 目录（macOS 和 Linux）或 ``%USERPROFILE%\AppData\Roaming\Zed\themes\`` 目录（Windows）中。

例如，要创建一个名为 ``my-cool-theme`` 的新主题，请在该目录中创建一个名为 ``my-cool-theme.json`` 的文件。下次 Zed 加载时，该主题将在主题选择器中可用。

更多主题可在 [zed-themes.com](https://zed-themes.com) 找到。

## 主题开发

请参阅：[开发 Zed 主题](./extensions/themes.md)