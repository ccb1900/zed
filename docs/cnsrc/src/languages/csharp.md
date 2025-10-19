# C#

请注意，在设置中语言名称应为 "CSharp"，而非 "C#"。

C# 支持通过 [C# 扩展](https://github.com/zed-extensions/csharp) 提供。

- 语法分析器：[tree-sitter/tree-sitter-c-sharp](https://github.com/tree-sitter/tree-sitter-c-sharp)
- 语言服务器：[OmniSharp/omnisharp-roslyn](https://github.com/OmniSharp/omnisharp-roslyn)

## 配置

可在 Zed 配置文件中通过以下方式配置 `OmniSharp` 二进制文件：

```json [settings]
{
  "lsp": {
    "omnisharp": {
      "binary": {
        "path": "/path/to/OmniSharp",
        "arguments": ["optional", "additional", "args", "-lsp"]
      }
    }
  }
}
```