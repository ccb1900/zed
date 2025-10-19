# Elixir

Elixir 支持可通过 [Elixir 扩展](https://github.com/zed-extensions/elixir) 获得。

- Tree-sitter：
  - [elixir-lang/tree-sitter-elixir](https://github.com/elixir-lang/tree-sitter-elixir)
  - [phoenixframework/tree-sitter-heex](https://github.com/phoenixframework/tree-sitter-heex)
- 语言服务器：
  - [elixir-lang/expert](https://github.com/elixir-lang/expert)
  - [elixir-lsp/elixir-ls](https://github.com/elixir-lsp/elixir-ls)
  - [elixir-tools/next-ls](https://github.com/elixir-tools/next-ls)
  - [lexical-lsp/lexical](https://github.com/lexical-lsp/lexical)

## 选择语言服务器

Elixir 扩展为 `expert`、`elixir-ls`、`next-ls` 和 `lexical` 提供语言服务器支持。

默认启用 `elixir-ls`。

### Expert

要切换到 `expert`，请将以下内容添加到你的 `settings.json` 中：

### 切换到 Next LS

要切换到 `next-ls`，请将以下内容添加到您的 `settings.json` 中：

```json [settings]
  "languages": {
    "Elixir": {
      "language_servers": ["next-ls", "!expert", "!elixir-ls", "!lexical", "..."]
    },
    "HEEX": {
      "language_servers": ["next-ls", "!expert", "!elixir-ls", "!lexical", "..."]
    }
  }
```

### 切换到 Lexical

要切换到 `lexical`，请将以下内容添加到您的 `settings.json` 中：

```json [settings]
  "languages": {
    "Elixir": {
      "language_servers": ["lexical", "!expert", "!elixir-ls", "!next-ls", "..."]
    },
    "HEEX": {
      "language_servers": ["lexical", "!expert", "!elixir-ls", "!next-ls", "..."]
    }
  }
```

## 设置 `elixir-ls`

1. 安装 `elixir`：

```sh
brew install elixir
```

2. 安装 `elixir-ls`：

```sh
brew install elixir-ls
```

3. 重启 Zed

> 若 `elixir-ls` 未在 Elixir 项目中运行，请通过命令面板操作 `zed: open log` 查看错误日志。若发现提及 `invalid LSP message header "Shall I install Hex? (if running non-interactively, use \"mix local.hex --force\") [Yn]` 的错误信息，您可能需要安装 [`Hex`](https://hex.pm)。请在命令行中运行 `elixir-ls` 并确认提示以安装 `Hex`。

### 使用 Mix 进行格式化

若您更倾向于使用 [Mix](https://hexdocs.pm/mix/Mix.html) 格式化代码，请在 `settings.json` 文件中添加以下配置片段，将其设为外部格式化工具。格式化将在文件保存时自动执行。

```json [settings]
{
  "languages": {
    "Elixir": {
      "format_on_save": "on",
      "formatter": {
        "external": {
          "command": "mix",
          "arguments": ["format", "--stdin-filename", "{buffer_path}", "-"]
        }
      }
    }
  }
}
```

### 额外的工作区配置选项

你可以通过 `settings.json` 中的 LSP 设置传递额外的 elixir-ls 工作区配置选项。

以下示例禁用了 dialyzer：

```json [settings]
  "lsp": {
    "elixir-ls": {
      "settings": {
        "dialyzerEnabled": false
      }
    }
  }
```

更多选项请参阅 [ElixirLS 配置设置](https://github.com/elixir-ls/elixir-ls#elixirls-configuration-settings)。

### HEEx

Zed 同样支持 HEEx 模板。HEEx 是 [EEx](https://hexdocs.pm/eex/1.12.3/EEx.html)（嵌入式 Elixir）和 HTML 的结合，常用于 Phoenix LiveView 应用程序中。

- Tree-sitter 支持：[phoenixframework/tree-sitter-heex](https://github.com/phoenixframework/tree-sitter-heex)