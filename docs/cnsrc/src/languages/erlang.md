# Erlang

Erlang 支持可通过 [Erlang 扩展](https://github.com/zed-extensions/erlang)实现。

- Tree-sitter 解析器：[WhatsApp/tree-sitter-erlang](https://github.com/WhatsApp/tree-sitter-erlang)
- 语言服务器：
  - [erlang-ls/erlang_ls](https://github.com/erlang-ls/erlang_ls)
  - [WhatsApp/erlang-language-platform](https://github.com/WhatsApp/erlang-language-platform)

## 选择语言服务器

Erlang 扩展为 `erlang_ls` 和 `erlang-language-platform` 提供语言服务器支持。

默认启用 `erlang_ls`。

若要切换至 `erlang-language-platform`，请在 `settings.json` 中添加以下配置：

```json [settings]
{
  "languages": {
    "Erlang": {
      "language_servers": ["elp", "!erlang-ls", "..."]
    }
  }
}
```

## 另请参阅：

- [Elixir](./elixir.md)
- [Gleam](./gleam.md)