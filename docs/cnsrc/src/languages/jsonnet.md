# Jsonnet

Zed 对 Jsonnet 语言的支持由社区维护的 [Jsonnet 扩展](https://github.com/narqo/zed-jsonnet)提供。

- 语法分析器：[sourcegraph/tree-sitter-jsonnet](https://github.com/sourcegraph/tree-sitter-jsonnet)
- 语言服务器：[grafana/jsonnet-language-server](https://github.com/grafana/jsonnet-language-server)

## 配置

工作区配置选项可通过 `settings.json` 的 `lsp` 设置传递给语言服务器。

以下示例启用了在 `jsonnet-language-server` 中解析 [tanka](https://tanka.dev) 导入路径的支持：

```json [settings]
{
  "lsp": {
    "jsonnet-language-server": {
      "settings": {
        "resolve_paths_with_tanka": true
      }
    }
  }
}
```