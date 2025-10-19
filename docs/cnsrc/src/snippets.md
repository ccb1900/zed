# 代码片段

使用 {#action snippets::ConfigureSnippets} 操作来创建新的代码片段文件，或编辑指定[作用域](#scopes)的现有代码片段文件。

代码片段位于 `~/.config/zed/snippets` 目录中，您可以通过 {#action snippets::OpenFolder} 操作导航至该目录。

## 配置示例

```json [settings]
{
  // Each snippet must have a name and body, but the prefix and description are optional.
  // The prefix is used to trigger the snippet, but when omitted then the name is used.
  // Use placeholders like $1, $2 or ${1:defaultValue} to define tab stops.
  // The $0 determines the final cursor position.
  // Placeholders with the same value are linked.
  "Log to console": {
    "prefix": "log",
    "body": ["console.info(\"Hello, ${1:World}!\")", "$0"],
    "description": "Logs to console"
  }
}
```

## 作用域

作用域由小写的语言名称决定，例如 `python.json` 对应 Python，`shell script.json` 对应 Shell 脚本，但存在以下例外情况：

| 作用域     | 文件名           |
| ---------- | ---------------- |
| 全局       | snippets.json    |
| JSX        | javascript.json  |
| 纯文本     | plaintext.json   |

创建 JSX 代码片段需使用 `javascript.json` 片段文件，而非 `jsx.json`，但此规则不适用于遵循上述约定的 TSX 和 TypeScript。

## 已知限制

- 当传入前缀列表时，仅会使用第一个前缀。
- 目前仅支持 `json` 片段文件格式，尽管 `simple-completion-language-server` 同时支持 `json` 和 `toml` 文件格式。

## 相关说明

`simple-completion-language-server` 中的 `feature_paths` 选项默认处于关闭状态。

如需启用该功能，请在 `settings.json` 中添加以下配置：

更多配置信息，请参阅 [`simple-completion-language-server` 说明](https://github.com/zed-industries/simple-completion-language-server/tree/main)。