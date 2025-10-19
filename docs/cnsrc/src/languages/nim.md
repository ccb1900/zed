# Nim

Zed 中的 Nim 语言支持由社区维护的 [Nim 扩展](https://github.com/foxoman/zed-nim)提供。
问题反馈请至：[https://github.com/foxoman/zed-nim/issues](https://github.com/foxoman/zed-nim/issues)

- 语法分析器：[alaviss/tree-sitter-nim](https://github.com/alaviss/tree-sitter-nim)
- 语言服务器：[nim-lang/langserver](https://github.com/nim-lang/langserver)

## 代码格式化

若要使用 [arnetheduck/nph](https://github.com/arnetheduck/nph) 作为格式化工具，请遵循 [nph 安装指南](https://github.com/arnetheduck/nph?tab=readme-ov-file#installation)并将以下配置添加到 Zed 的 `settings.json` 中：

```json [settings]
  "languages": {
    "Nim": {
      "formatter": {
        "external": {
          "command": "nph",
          "arguments": ["-"]
        }
      }
    }
  }
```