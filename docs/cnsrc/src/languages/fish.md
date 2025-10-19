# Fish

Zed 中的 Fish 语言支持由社区维护的 [Fish 扩展](https://github.com/hasit/zed-fish)提供。  
问题反馈请至：[https://github.com/hasit/zed-fish/issues](https://github.com/hasit/zed-fish/issues)

- Tree-sitter 解析器：[ram02z/tree-sitter-fish](https://github.com/ram02z/tree-sitter-fish)

### 代码格式化

Zed 支持通过外部工具（如随 fish 内置的 [`fish_indent`](https://fishshell.com/docs/current/cmds/fish_indent.html)）自动格式化 fish 代码。

1. 确保 `fish_indent` 位于您的环境路径中，并检查版本：

```sh
which fish_indent
fish_indent --version
```

2. 配置 Zed 以使用 `fish_indent` 自动格式化 fish 代码：

```json [settings]
  "languages": {
    "Fish": {
      "formatter": {
        "external": {
          "command": "fish_indent"
        }
      }
    }
  },
```