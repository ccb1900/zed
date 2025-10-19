# TypeScript

Zed 原生支持 TypeScript 和 TSX。

- 语法分析器：[tree-sitter/tree-sitter-typescript](https://github.com/tree-sitter/tree-sitter-typescript)
- 语言服务器：[yioneko/vtsls](https://github.com/yioneko/vtsls)
- 备用语言服务器：[typescript-language-server/typescript-language-server](https://github.com/typescript-language-server/typescript-language-server)
- 调试适配器：[vscode-js-debug](https://github.com/microsoft/vscode-js-debug)

<!--
TBD: Document the difference between Language servers
-->

## 语言服务器

默认情况下，Zed 对 TypeScript、TSX 和 JavaScript 文件使用 [vtsls](https://github.com/yioneko/vtsls)。
您可以在设置文件中按语言配置使用 [typescript-language-server](https://github.com/typescript-language-server/typescript-language-server)：

```json [settings]
{
  "languages": {
    "TypeScript": {
      "language_servers": ["typescript-language-server", "!vtsls", "..."]
    },
    "TSX": {
      "language_servers": ["typescript-language-server", "!vtsls", "..."]
    },
    "JavaScript": {
      "language_servers": ["typescript-language-server", "!vtsls", "..."]
    }
  }
}
```

默认情况下，Prettier 也将用于 TypeScript 文件。如需禁用此功能：

```json [settings]
{
  "languages": {
    "TypeScript": {
      "prettier": { "allowed": false }
    }
    //...
  }
}
```

## 大型项目

`vtsls` 在超大型项目中可能会耗尽内存。我们将内存限制默认设置为 8092（8 GiB），而非默认的 3072，但这可能仍无法满足您的需求：

```json [settings]
{
  "lsp": {
    "vtsls": {
      "settings": {
        // For TypeScript:
        "typescript": { "tsserver": { "maxTsServerMemory": 16184 } },
        // For JavaScript:
        "javascript": { "tsserver": { "maxTsServerMemory": 16184 } }
      }
    }
  }
}
```

## 内联提示

Zed 设置了以下初始化选项，使语言服务器能够返回内联提示（即当 Zed 在设置中启用了内联提示功能时）。

您可以在使用 `typescript-language-server` 时，通过 Zed 的 `settings.json` 文件覆盖这些设置：

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

```json [settings]
{
  "lsp": {
    "vtsls": {
      "settings": {
        // For JavaScript:
        "javascript": {
          "inlayHints": {
            "parameterNames": {
              "enabled": "all",
              "suppressWhenArgumentMatchesName": false
            },
            "parameterTypes": {
              "enabled": true
            },
            "variableTypes": {
              "enabled": true,
              "suppressWhenTypeMatchesName": true
            },
            "propertyDeclarationTypes": {
              "enabled": true
            },
            "functionLikeReturnTypes": {
              "enabled": true
            },
            "enumMemberValues": {
              "enabled": true
            }
          }
        },
        // For TypeScript:
        "typescript": {
          "inlayHints": {
            "parameterNames": {
              "enabled": "all",
              "suppressWhenArgumentMatchesName": false
            },
            "parameterTypes": {
              "enabled": true
            },
            "variableTypes": {
              "enabled": true,
              "suppressWhenTypeMatchesName": true
            },
            "propertyDeclarationTypes": {
              "enabled": true
            },
            "functionLikeReturnTypes": {
              "enabled": true
            },
            "enumMemberValues": {
              "enabled": true
            }
          }
        }
      }
    }
  }
}
```

## 调试

Zed 内置支持使用 `vscode-js-debug` 调试 TypeScript 代码。
以下内容无需编写额外配置即可进行调试：

- 来自 `package.json` 的任务
- 使用多个流行框架（Jest、Mocha、Vitest、Jasmine、Bun、Node）编写的测试

运行 {#action debugger::Start} ({#kb debugger::Start}) 可查看这些预定义调试任务的上下文列表。

> **注意：** 当 `@types/bun` 存在于 `package.json` 中时，Bun 测试会被自动检测到。
>
> **注意：** 当 `@types/node` 存在于 `package.json` 中时，Node 测试会被自动检测到（需要 Node.js 20+ 版本）。

与所有语言一样，来自 `.vscode/launch.json` 的配置也可在 Zed 中用于调试。

如果您的使用场景未被上述任何一项覆盖，可以通过向 `.zed/debug.json` 添加调试配置来获得完全控制权。下方提供了配置示例。

### 配置 JavaScript 调试任务

JavaScript调试比其他语言更复杂，因为它涉及两种不同环境：Node.js和浏览器。`vscode-js-debug`提供了`type`字段，可用于指定运行环境，可选值为`node`或`chrome`。

- [vscode-js-debug配置文档](https://github.com/microsoft/vscode-js-debug/blob/main/OPTIONS.md)

### 附加调试器到浏览器中运行的服务器（`npx serve`）

对于外部运行的Web服务器（例如使用`npx serve`或`npx live-server`启动的服务器），可以附加调试器并通过浏览器打开。

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