# Deno

Deno 支持可通过 [Deno 扩展](https://github.com/zed-extensions/deno)实现。

- 语言服务器：[Deno 语言服务器](https://docs.deno.com/runtime/manual/advanced/language_server/overview/)

## Deno 配置

若要在 TypeScript 和 TSX 文件中使用 Deno 语言服务器，建议禁用默认语言服务器并启用 deno，只需将以下内容添加到您的 `settings.json` 中：

```json [settings]
{
  "lsp": {
    "deno": {
      "settings": {
        "deno": {
          "enable": true
        }
      }
    }
  },
  "languages": {
    "JavaScript": {
      "language_servers": [
        "deno",
        "!typescript-language-server",
        "!vtsls",
        "!eslint"
      ],
      "formatter": "language_server"
    },
    "TypeScript": {
      "language_servers": [
        "deno",
        "!typescript-language-server",
        "!vtsls",
        "!eslint"
      ],
      "formatter": "language_server"
    },
    "TSX": {
      "language_servers": [
        "deno",
        "!typescript-language-server",
        "!vtsls",
        "!eslint"
      ],
      "formatter": "language_server"
    }
  }
}
```

更多信息请参阅 Zed 文档中的[配置支持语言](../configuring-languages.md)。

<!--
TBD: Deno TypeScript REPL instructions [docs/repl#typescript-deno](../repl.md#typescript-deno)
-->

## 配置补全

要获取 `deno.json` 或 `package.json` 的补全功能，您可以在 `settings.json` 中添加以下内容：（更多信息请参阅 https://zed.dev/docs/languages/json）

```json [settings]
"lsp": {
    "json-language-server": {
      "settings": {
        "json": {
          "schemas": [
            {
              "fileMatch": [
                "deno.json",
                "deno.jsonc"
              ],
              "url": "https://raw.githubusercontent.com/denoland/deno/refs/heads/main/cli/schemas/config-file.v1.json"
            },
            {
              "fileMatch": [
                "package.json"
              ],
              "url": "http://json.schemastore.org/package"
            }
          ]
        }
      }
    }
  }
```

## DAP 支持

若要调试 deno 程序，请将以下内容添加到 `.zed/debug.json`：

## 可运行支持

若要从用户界面运行 Deno 任务（如测试），请将以下内容添加到 `.zed/tasks.json`：

```json [tasks]
[
  {
    "label": "deno test",
    "command": "deno test -A --filter '/^$ZED_CUSTOM_DENO_TEST_NAME$/' '$ZED_FILE'",
    "tags": ["js-test"]
  }
]
```

## 另请参阅：

- [TypeScript](./typescript.md)
- [JavaScript](./javascript.md)