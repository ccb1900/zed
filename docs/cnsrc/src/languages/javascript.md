# JavaScript

Zed 原生支持 JavaScript。

- Tree-sitter：[tree-sitter/tree-sitter-javascript](https://github.com/tree-sitter/tree-sitter-javascript)
- 语言服务器：[typescript-language-server/typescript-language-server](https://github.com/typescript-language-server/typescript-language-server)
- 调试适配器：[vscode-js-debug](https://github.com/microsoft/vscode-js-debug)

## 代码格式化

JavaScript 默认在保存时启用格式化，使用 TypeScript 内置的代码格式化功能。
但许多 JavaScript 项目会使用其他命令行代码格式化工具，例如 [Prettier](https://prettier.io/)。
您可以在设置中为 JavaScript 指定*外部*代码格式化工具来使用这些工具。
更多信息请参阅[配置文档](../configuring-zed.md)。

例如，如果您已安装 Prettier 并将其添加到您的`PATH`，可以通过在`settings.json`中添加以下配置来使用它格式化 JavaScript 文件：

## JSX

Zed 原生支持 JSX 语法高亮。

在 JSX 字符串中，[`tailwindcss-language-server`](./tailwindcss.md) 用于为 Tailwind CSS 类提供自动补全功能。

## JSDoc

Zed 支持符合 JSDoc 语法的 JavaScript 和 TypeScript 注释。
Zed 使用 [tree-sitter/tree-sitter-jsdoc](https://github.com/tree-sitter/tree-sitter-jsdoc) 进行 JSDoc 的解析和高亮显示。

## ESLint

您可以通过在格式化时运行 ESLint 代码操作，配置 Zed 使用 `eslint --fix` 来格式化代码：

```json [settings]
{
  "languages": {
    "JavaScript": {
      "code_actions_on_format": {
        "source.fixAll.eslint": true
      }
    }
  }
}
```

您还可以在使用 `fixAll` 时仅执行单个 ESLint 规则：

> **注意：** 你配置的其他格式化程序仍会在 ESLint 之后运行。
> 因此，如果你的语言服务器或 Prettier 配置未按照 ESLint 的规则进行格式化，
> 它们将覆盖 ESLint 修复的内容，最终导致错误。

如果你**仅**希望在保存时运行 ESLint，可以将代码操作配置为格式化程序：

```json [settings]
{
  "languages": {
    "JavaScript": {
      "code_actions_on_format": {
        "source.fixAll.eslint": true
      }
    }
  }
}
```

### 配置 ESLint 的 `nodePath`：

你可以配置 ESLint 的 `nodePath` 设置：

### 配置 ESLint 的解析器选项：

您可以通过配置 ESLint 的解析器选项来实现。

例如，以下是如何设置解析器选项：

```json
{
  "parserOptions": {
    "ecmaVersion": 2020,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  }
}
```

### 配置 ESLint 的环境：

您可以配置 ESLint 的环境设置：

```json
{
  "env": {
    "browser": true,
    "node": true,
    "es6": true
  }
}
```

### 配置 ESLint 的规则：

您可以配置 ESLint 的规则设置：

## 调试

Zed 默认支持使用 `vscode-js-debug` 调试 JavaScript 代码。
以下内容无需编写额外配置即可调试：

- 来自 `package.json` 的任务
- 使用多个流行框架（Jest、Mocha、Vitest、Jasmine、Bun、Node）编写的测试

运行 {#action debugger::Start} ({#kb debugger::Start}) 可查看这些预定义调试任务的上下文列表。

> **注意：** 当 `package.json` 中存在 `@types/bun` 时，Bun 测试会自动被检测到。
>
> **注意：** 当 `package.json` 中存在 `@types/node` 时，Node 测试会自动被检测到（需要 Node.js 20 或更高版本）。

与所有语言一样，来自 `.vscode/launch.json` 的配置也可在 Zed 中用于调试。

若以上场景均不满足您的需求，您可以通过向`.zed/debug.json`添加调试配置来实现完全掌控。下方提供配置示例参考。

### 配置 JavaScript 调试任务

JavaScript 调试相较其他语言更为复杂，因其存在两种不同运行环境：Node.js 与浏览器。`vscode-js-debug`提供了`type`字段，可用于指定`node`或`chrome`环境。

- [vscode-js-debug 配置文档](https://github.com/microsoft/vscode-js-debug/blob/main/OPTIONS.md)

### 在 Node 环境中调试当前文件

```json [debug]
[
  {
    "adapter": "JavaScript",
    "label": "Debug JS file",
    "type": "node",
    "request": "launch",
    "program": "$ZED_FILE",
    "skipFiles": ["<node_internals>/**"]
  }
]
```

### 在 Chrome 中启动网页应用调试

## 相关链接

- [Yarn 文档](./yarn.md)：配置项目使用 Yarn 的详细指南。
- [TypeScript 文档](./typescript.md)