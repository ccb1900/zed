# JSON

Zed 原生支持 JSON 格式。

- 语法分析器：[tree-sitter/tree-sitter-json](https://github.com/tree-sitter/tree-sitter-json)
- 语言服务器：[zed-industries/json-language-server](https://github.com/zed-industries/json-language-server)

## JSONC

Zed 还支持 JSON 的超集 JSONC，该格式允许在 JSON 文件中使用单行注释（`//`）。
编辑这类文件时，您可以使用 `cmd-/`（macOS 系统）或 `ctrl-/`（Linux 系统）来切换当前行或选中内容的注释状态。

## JSONC 的 Prettier 格式化

当您使用 `Format Document` 或启用 `format_on_save` 功能时，若处理的文件扩展名为 `*.jsonc`，Zed 会调用 Prettier 作为格式化工具。Prettier 存在一个[待解决的问题](https://github.com/prettier/prettier/issues/15956)，会在扩展名为 `jsonc` 的文件中添加尾随逗号。而扩展名为 `.json` 的 JSONC 文件不受此影响。

要规避此问题，您可以在 `.prettierrc` 配置文件中添加以下配置：

## JSON 语言服务器

Zed 默认开箱即用地支持对 `package.json` 和 `tsconfig.json` 文件进行 JSON 模式验证，但 `json-language-server` 可以使用项目文件中的 JSON 模式定义、来自 [JSON 模式存储库](https://www.schemastore.org) 或其他公开可用 URL 的 JSON 验证。

### 内联模式规范

要在 JSON 文件中内联指定模式，请添加一个 `$schema` 顶级键，指向您的 JSON 模式文件。

例如，为配合 [lua-language-server](https://github.com/LuaLS/lua-language-server/) 使用 `.luarc.json`：

```json [settings]
{
  "$schema": "https://raw.githubusercontent.com/sumneko/vscode-lua/master/setting/schema.json",
  "runtime.version": "Lua 5.4"
}
```

### 通过设置指定模式

您也可以通过 Zed LSP 设置将 JSON 模式与文件路径关联。

具体操作如下：

```json [settings]
"lsp": {
  "json-language-server": {
    "settings": {
      "json": {
        "schemas": [
          {
            "fileMatch": ["*/*.luarc.json"],
            "url": "https://raw.githubusercontent.com/sumneko/vscode-lua/master/setting/schema.json"
          }
        ]
      }
    }
  }
}
```

您还可以通过 Zed 的 settings.json 配置文件，向 json-language-server 传递任何[支持的设置](https://github.com/Microsoft/vscode/blob/main/extensions/json-language-features/server/README.md#settings)：

<!--
TBD: Add formatter (prettier) settings (autoformat, tab_size, etc)
-->