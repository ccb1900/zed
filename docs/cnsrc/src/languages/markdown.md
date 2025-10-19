# Markdown

Zed 原生支持 Markdown 格式。

- 语法分析器：[tree-sitter-markdown](https://github.com/tree-sitter-grammars/tree-sitter-markdown)
- 语言服务器：暂未提供

## 代码块语法高亮

Zed 通过集成 [tree-sitter 语法分析器](../extensions/languages.md#grammar) 实现对 Markdown 代码块的特定语言语法高亮。所有 [Zed 支持的语言](../languages.md)（包括官方或社区扩展提供的语言）均可用于 Markdown 代码块。您只需在起始标记 <kbd>```</kbd> code fence like so:

````python
[[CODE_BLOCK_1]]
```` 后注明语言名称即可。

## 配置

### 格式化

Zed 支持使用 Prettier 自动重新格式化 Markdown 文档。您可以通过 {#action editor::Format} 操作或 {#kb editor::Format} 键盘快捷键手动触发此功能。或者，您可以在 settings.json 中启用 [`format_on_save`](../configuring-zed.md#format-on-save) 以自动格式化：

```json [settings]
  "languages": {
    "Markdown": {
      "format_on_save": "on"
    }
  },
```

### 尾部空格

默认情况下，Zed 会在保存时移除尾部空格。如果您依赖 Markdown 文件中不可见的尾部空格转换为 `<br />`，可以通过以下设置禁用此行为：

```json [settings]
  "languages": {
    "Markdown": {
      "remove_trailing_whitespace_on_save": false
    }
  },
```