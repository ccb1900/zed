# TypeScript

Zed 原生支持 TypeScript 与 TSX。

- 语法分析器：[tree-sitter/tree-sitter-typescript](https://github.com/tree-sitter/tree-sitter-typescript)
- 语言服务器：[yioneko/vtsls](https://github.com/yioneko/vtsls)
- 备用语言服务器：[typescript-language-server/typescript-language-server](https://github.com/typescript-language-server/typescript-language-server)
- 调试适配器：[vscode-js-debug](https://github.com/microsoft/vscode-js-debug)

<!--
TBD: Document the difference between Language servers
-->

## 语言服务器

默认情况下，Zed 使用 [vtsls](https://github.com/yioneko/vtsls) 处理 TypeScript、TSX 和 JavaScript 文件。
您可以在配置文件中按语言配置使用 [typescript-language-server](https://github.com/typescript-language-server/typescript-language-server)：

默认情况下，Prettier 也会用于 TypeScript 文件。要禁用此功能：

## 大型项目

在超大型项目中，`vtsls` 可能会出现内存不足的情况。我们将内存限制默认设置为 8092（8 GiB），而非默认的 3072，但这可能仍无法满足您的需求：

## 内联提示

Zed 设置了以下初始化选项，以便语言服务器返回内联提示（即当 Zed 在设置中启用了内联提示时）。

您可以在使用 `typescript-language-server` 时，通过 Zed 的 `settings.json` 覆盖这些设置：

```json [settings]
{
  "lsp": {
    "typescript-language-server": {
      "initialization_options": {
        "preferences": {
          "includeInlayParameterNameHints": "all",
          "includeInlayParameterNameHintsWhenArgumentMatchesName": true,
          "includeInlayFunctionParameterTypeHints": true,
          "includeInlayVariableTypeHints": true,
          "includeInlayVariableTypeHintsWhenTypeMatchesName": true,
          "includeInlayPropertyDeclarationTypeHints": true,
          "includeInlayFunctionLikeReturnTypeHints": true,
          "includeInlayEnumMemberValueHints": true
        }
      }
    }
  }
}
```

更多信息请参阅 [TypeScript 语言服务器嵌入提示文档](https://github.com/typescript-language-server/typescript-language-server?tab=readme-ov-file#inlay-hints-textdocumentinlayhint)。

当使用 `vtsls` 时：

[[代码块_0]]

## 调试功能

Zed 内置支持使用 `vscode-js-debug` 调试 TypeScript 代码。
以下情况无需额外配置即可直接调试：

- 来自 `package.json` 的任务
- 使用主流测试框架编写的测试（Jest、Mocha、Vitest、Jasmine、Bun、Node）

运行 {#action debugger::Start}（快捷键 {#kb debugger::Start}）可查看这些预定义调试任务的上下文列表。

> **注意：** 当 `@types/bun` 存在于 `package.json` 中时，Bun 测试将被自动识别。
>
> **注意：** 当 `@types/node` 存在于 `package.json` 中时，Node 测试将被自动识别（需要 Node.js 20+ 版本）。

与所有语言相同，来自 `.vscode/launch.json` 的配置同样可在 Zed 中用于调试。

若现有方案无法满足您的需求，可通过向 `.zed/debug.json` 添加调试配置来实现完整控制。下方提供配置示例参考。

### 配置 JavaScript 调试任务

JavaScript 调试比其他语言更复杂，因为它涉及两种不同环境：Node.js 和浏览器。`vscode-js-debug` 暴露了 `type` 字段，可用于指定运行环境，可选值为 `node` 或 `chrome`。

- [vscode-js-debug 配置文档](https://github.com/microsoft/vscode-js-debug/blob/main/OPTIONS.md)

### 为浏览器中运行的服务器附加调试器 (`npx serve`)

对于通过外部命令启动的 Web 服务器（例如使用 `npx serve` 或 `npx live-server`），可以附加调试器并通过浏览器打开。

```json [debug]
[
  {
    "label": "Launch Chrome (TypeScript)",
    "adapter": "JavaScript",
    "type": "chrome",
    "request": "launch",
    "url": "http://localhost:5500",
    "program": "$ZED_FILE",
    "webRoot": "${ZED_WORKTREE_ROOT}",
    "build": {
      "command": "npx",
      "args": ["tsc"]
    },
    "skipFiles": ["<node_internals>/**"]
  }
]
```

## 相关参考

- [Zed Yarn 文档](./yarn.md) —— 逐步指导如何配置项目以使用 Yarn。
- [Zed Deno 文档](./deno.md)