# Lua

Lua 支持可通过 [Lua 扩展](https://github.com/zed-extensions/lua) 获得。

- Tree-sitter: [tree-sitter-grammars/tree-sitter-lua](https://github.com/tree-sitter-grammars/tree-sitter-lua)
- 语言服务器: [LuaLS/lua-language-server](https://github.com/LuaLS/lua-language-server)

## luarc.json

要配置 LuaLS，您可以在工作区根目录下创建一个 `.luarc.json` 文件。

```json [settings]
{
  "$schema": "https://raw.githubusercontent.com/LuaLS/vscode-lua/master/setting/schema.json",
  "runtime.version": "Lua 5.4",
  "format.enable": true,
  "workspace.library": ["../somedir/library"]
}
```

请参阅 [LuaLS 设置文档](https://luals.github.io/wiki/settings/) 获取所有可用的配置选项。在 Zed 编辑器中编辑此文件时，可用设置选项将自动补全（例如 `runtime.version` 会显示 `"Lua 5.1"`、`"Lua 5.2"`、`"Lua 5.3"`、`"Lua 5.4"` 和 `"LuaJIT"` 作为允许值）。请注意，从 VS Code 导入设置选项时，需移除 `Lua.` 前缀（例如应使用 `runtime.version` 而非 `Lua.runtime.version`）。

### LuaCATS 定义

LuaLS 能够借助 LuaCATS（Lua 注释与类型系统）定义提供增强的 LSP 自动补全建议和类型验证。这些定义适用于众多常用 Lua 库，且可通过 `luarc.json` 中的 `workspace.library` 指定包含这些定义的本地路径。若将定义库检出至项目同级目录（如 `../playdate-luacats`、`../love2d` 等），可使用相对路径配置。此外，您也可以为每个 LuaCATS 定义库在项目内创建子模块。

### LÖVE (Love2D) {#love2d}

若要在 Zed 中使用 [LÖVE (Love2D)](https://love2d.org/)，请将 [LuaCATS/love2d](https://github.com/LuaCATS/love2d) 检出至项目父级目录下名为 `love2d-luacats` 的文件夹中：

```sh
cd .. && git clone https://github.com/LuaCATS/love2d love2d-luacats
```

随后在您的 `.luarc.json` 中配置：

```
{
  "$schema": "https://raw.githubusercontent.com/LuaLS/vscode-lua/master/setting/schema.json",
  "runtime.version": "Lua 5.4",
  "workspace.library": ["../love2d-luacats"],
  "runtime.special": {
    "love.filesystem.load": "loadfile"
  }
}
```

### Playdate SDK

要在 Zed 中使用 [Playdate Lua SDK](https://play.date/dev/)，请将 [playdate-luacats](https://github.com/notpeter/playdate-luacats) 检出到您项目的父文件夹中：

```sh
cd .. && git clone https://github.com/notpeter/playdate-luacats
```

然后在您的 `.luarc.json` 中：

```json [settings]
{
  "$schema": "https://raw.githubusercontent.com/LuaLS/vscode-lua/master/setting/schema.json",
  "runtime.version": "Lua 5.4",
  "runtime.nonstandardSymbol": [
    "+=",
    "-=",
    "*=",
    "/=",
    "//=",
    "%=",
    "<<=",
    ">>=",
    "&=",
    "|=",
    "^="
  ],
  "diagnostics.severity": { "duplicate-set-field": "Hint" },
  "diagnostics.globals": ["import"],
  "workspace.library": ["../playdate-luacats"],
  "format.defaultConfig": {
    "indent_style": "space",
    "indent_size": "4"
  },
  "format.enable": true,
  "runtime.builtin": { "io": "disable", "os": "disable", "package": "disable" }
}
```

### 内联提示

要在 Zed 中启用 LuaLS 的[内联提示](../configuring-languages.md#inlay-hints)功能：

1. 请将以下内容添加到您的 Zed settings.json 配置文件中：

```json [settings]
  "languages": {
    "Lua": {
      "inlay_hints": {
        "enabled": true,
        "show_type_hints": true,
        "show_parameter_hints": true,
        "show_other_hints": true
      }
    }
  }
```

2. 将`"hint.enable": true`添加到您的`.luarc.json`中。

## 格式化

### LuaLS 格式化

要启用 LuaLS（由 [CppCXY/EmmyLuaCodeStyle](https://github.com/CppCXY/EmmyLuaCodeStyle) 提供）的自动格式化功能，请确保您的 .luarc.json 文件中包含`"format.enable": true,`：

```json [settings]
{
  "$schema": "https://raw.githubusercontent.com/sumneko/vscode-lua/master/setting/schema.json",
  "format.enable": true
}
```

然后将以下内容添加到您的 Zed `settings.json` 中：

```json [settings]
{
  "languages": {
    "Lua": {
      "format_on_save": "on",
      "formatter": "language_server"
    }
  }
}
```

您可以通过 `.editorconfig` 自定义多种 EmmyLuaCodeStyle 样式选项，完整可用选项请参阅 [lua.template.editorconfig](https://github.com/CppCXY/EmmyLuaCodeStyle/blob/master/lua.template.editorconfig)。

### StyLua 格式化

若需使用 [StyLua](https://github.com/JohnnyMorganz/StyLua) 进行自动格式化：

1. 安装 [StyLua](https://github.com/JohnnyMorganz/StyLua)：`brew install stylua` 或 `cargo install stylua --features lua52,lua53,lua54,luau,luajit`（可移除不需要的 Lua 版本）。
2. 将以下内容添加到您的 `settings.json`：

```json [settings]
{
  "languages": {
    "Lua": {
      "format_on_save": "on",
      "formatter": {
        "external": {
          "command": "stylua",
          "arguments": [
            "--syntax=Lua54",
            "--respect-ignores",
            "--stdin-filepath",
            "{buffer_path}",
            "-"
          ]
        }
      }
    }
  }
}
```

你可以通过命令行（例如`--syntax=Lua54`）或在工作区的`stylua.toml`中为StyLua指定各种选项：

```toml
syntax = "Lua54"
column_width = 100
line_endings = "Unix"
indent_type = "Spaces"
indent_width = 4
quote_style = "AutoPreferDouble"
call_parentheses = "Always"
collapse_simple_statement = "All"

[sort_requires]
enabled = true
```

如需查看完整的可用选项列表，请参阅：[StyLua选项](https://github.com/JohnnyMorganz/StyLua?tab=readme-ov-file#options)。