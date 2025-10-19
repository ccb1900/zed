# Luau

[Luau](https://luau.org/) 是一种源自 Lua 的快速、小巧、安全、渐进式类型、可嵌入的脚本语言。Luau 由 Roblox 开发，基于 MIT 许可证开放使用。

Zed 编辑器中的 Luau 语言支持由社区维护的 [Luau 扩展](https://github.com/4teapo/zed-luau)提供。
问题反馈请提交至：[https://github.com/4teapo/zed-luau/issues](https://github.com/4teapo/zed-luau/issues)

- 语法分析器：[4teapo/tree-sitter-luau](https://github.com/4teapo/tree-sitter-luau)
- 语言服务器：[JohnnyMorganz/luau-lsp](https://github.com/JohnnyMorganz/luau-lsp)

## 配置说明

具体配置说明请参阅 [Luau Zed 扩展说明文档](https://github.com/4teapo/zed-luau)。

## 代码格式化

如需支持代码自动格式化功能，可使用 Lua 代码格式化工具 [JohnnyMorganz/StyLua](https://github.com/JohnnyMorganz/StyLua)。

安装方式：

然后，将以下内容添加到你的 Zed `settings.json` 中：

```json [settings]
  "languages": {
    "Luau": {
      "formatter": {
        "external": {
          "command": "stylua",
          "arguments": ["-"]
        }
      }
    }
  }
```