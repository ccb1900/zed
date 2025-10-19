# Elm

通过 [Elm 扩展](https://github.com/zed-extensions/elm)可支持 Elm 语言。

- 语法分析器：[elm-tooling/tree-sitter-elm](https://github.com/elm-tooling/tree-sitter-elm)
- 语言服务器：[elm-tooling/elm-language-server](https://github.com/elm-tooling/elm-language-server)

## 环境配置

Zed 对 Elm 的支持需要安装 `elm`、`elm-format` 和 `elm-review`。

1. [安装 Elm](https://guide.elm-lang.org/install/elm.html)（macOS 系统可运行 `brew install elm`）
2. 安装 `elm-review` 以支持代码检查：
   ```sh
   npm install elm-review --save-dev
   ```
3. 安装 `elm-format` 以支持自动格式化：
   ```sh
   npm install -g elm-format
   ```

## 配置 `elm-language-server`

可通过 `settings.json` 文件配置 Elm 语言服务器，例如：

```json [settings]
{
  "lsp": {
    "elm-language-server": {
      "initialization_options": {
        "disableElmLSDiagnostics": true,
        "onlyUpdateDiagnosticsOnSave": false,
        "elmReviewDiagnostics": "warning"
      }
    }
  }
}
```、`elm-format`、`elm-review` 和 `elm` 需要安装并在环境中可用，或在设置中进行配置。请参阅[此处的完整服务器设置列表](https://github.com/elm-tooling/elm-language-server?tab=readme-ov-file#server-settings)。