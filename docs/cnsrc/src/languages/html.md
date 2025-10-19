# HTML

HTML 支持通过 [HTML 扩展](https://github.com/zed-industries/zed/tree/main/extensions/html)实现。

- 语法分析器：[tree-sitter/tree-sitter-html](https://github.com/tree-sitter/tree-sitter-html)
- 语言服务器：[microsoft/vscode-html-languageservice](https://github.com/microsoft/vscode-html-languageservice)

该扩展会自动安装，若需禁用，请在设置中添加以下配置：

```json [settings]
{
  "auto_install_extensions": {
    "html": false
  }
}
```

## 格式化

Zed 默认使用 [Prettier](https://prettier.io/) 进行 HTML 格式化。

您可以通过在 Zed `settings.json` 中添加以下配置来禁用 `format_on_save`：

```json [settings]
  "languages": {
    "HTML": {
      "format_on_save": "off",
    }
  }
```

你仍可通过 {#kb editor::Format} 手动触发格式化，或打开[命令面板](..//getting-started.md#command-palette) ({#kb command_palette::Toggle}) 并选择“格式化文档”。

### LSP 格式化

若想使用 `vscode-html-language-server` 语言服务器自动格式化功能替代 Prettier，请将以下配置添加到 Zed 设置中：

```json [settings]
  "languages": {
    "HTML": {
      "formatter": "language_server",
    }
  }
```

你可以通过 Zed 的 `settings.json` 自定义 `vscode-html-language-server` 的各种[格式化选项](https://code.visualstudio.com/docs/languages/html#_formatting)。

## 相关链接

- [CSS](./css.md)
- [JavaScript](./javascript.md)
- [TypeScript](./typescript.md)