# 配置支持的语言

Zed 为支持的每种编程语言提供了强大的自定义选项。本指南将带您了解各种定制编码体验的方法，以满足您的偏好和项目需求。

Zed 的语言支持建立在两项核心技术之上：

1. Tree-sitter：负责语法高亮和基于结构的功能，如大纲面板
2. 语言服务器协议（LSP）：提供语义功能，如代码补全和诊断

这些组件协同工作，共同构成了 Zed 的语言能力。

在本指南中，我们将涵盖：

- 语言特定设置
- 文件关联
- 使用语言服务器
- 格式化和代码检查配置
- 自定义语法高亮和主题
- 高级语言功能

完成本指南后，您将掌握如何在 Zed 中配置和自定义支持的语言。

要查看 Zed 支持的所有语言及其具体配置，请参阅我们的[支持的语言](./languages.md)页面。若需进一步扩展，您可以探索开发自己的扩展插件，以添加对新语言的支持或增强现有功能。有关创建语言扩展的更多信息，请参阅我们的[语言扩展指南](./extensions/languages.md)。

## 语言专属设置

Zed 允许您为特定语言覆盖全局设置。这些自定义配置需在 `settings.json` 文件中通过 `languages` 字段进行定义。

以下为语言专属设置示例：

```json [settings]
"languages": {
  "Python": {
    "tab_size": 4,
    "formatter": "language_server",
    "format_on_save": "on"
  },
  "JavaScript": {
    "tab_size": 2,
    "formatter": {
      "external": {
        "command": "prettier",
        "arguments": ["--stdin-filepath", "{buffer_path}"]
      }
    }
  }
}
```

您可以为每种语言自定义多种设置，包括：

- [`tab_size`](./configuring-zed.md#tab-size)：每个缩进级别的空格数
- [`formatter`](./configuring-zed.md#formatter)：用于代码格式化的工具
- [`format_on_save`](./configuring-zed.md#format-on-save)：保存时是否自动格式化代码
- [`enable_language_server`](./configuring-zed.md#enable-language-server)：切换语言服务器支持
- [`hard_tabs`](./configuring-zed.md#hard-tabs)：使用制表符而非空格进行缩进
- [`preferred_line_length`](./configuring-zed.md#preferred-line-length)：建议的最大行长度
- [`soft_wrap`](./configuring-zed.md#soft-wrap)：长代码行的换行方式
- [`show_completions_on_input`](./configuring-zed.md#show-completions-on-input)：输入时是否显示补全建议
- [`show_completion_documentation`](./configuring-zed.md#show-completion-documentation)：是否在补全菜单中显示内联及侧边文档说明

这些设置让您能够在不同语言和项目中保持特定的编码风格。

## 文件关联

Zed 会根据文件扩展名自动识别文件类型，但您可以根据工作流程自定义这些关联关系。

要设置自定义文件关联，请在您的`settings.json`中使用[`file_types`](./configuring-zed.md#file-types)设置：

```json [settings]
"file_types": {
  "C++": ["c"],
  "TOML": ["MyLockFile"],
  "Dockerfile": ["Dockerfile*"]
}
```

此配置将指示 Zed：

- 将`.c`文件识别为 C++ 而非 C 语言
- 将名为 "MyLockFile" 的文件识别为 TOML 格式
- 对任何以 "Dockerfile" 开头的文件应用 Dockerfile 语法

您可以使用通配符模式实现更灵活的匹配，从而处理项目中复杂的命名规范。

## 语言服务器配置

语言服务器是 Zed 智能编码功能的核心组成部分，提供自动补全、跳转到定义、实时错误检查等强大功能。

### 什么是语言服务器？

语言服务器实现了语言服务器协议（LSP），该协议标准化了编辑器与语言特定工具之间的通信。这使得Zed能够支持多种编程语言的高级功能，而无需单独实现每个功能。

语言服务器提供的一些关键功能包括：

- 代码补全
- 错误检查和诊断
- 代码导航（跳转到定义、查找引用）
- 代码操作（重命名、提取方法）
- 悬停信息
- 工作区符号搜索

### 管理语言服务器

Zed为用户简化了语言服务器的管理：

1. 自动下载：当您打开具有匹配文件类型的文件时，Zed会自动下载相应的语言服务器。对于已知文件类型，Zed可能会提示您安装扩展。

2. 存储位置：

- macOS：`~/Library/Application Support/Zed/languages`
- Linux：`$XDG_DATA_HOME/zed/languages`、`$FLATPAK_XDG_DATA_HOME/zed/languages` 或 `$HOME/.local/share/zed/languages`

3. 自动更新：Zed 会持续维护语言服务器的最新版本，确保您始终能使用最新的功能与优化。

### 语言服务器选择

Zed 中部分语言提供多个语言服务器选项。当您安装的多个扩展包都包含针对同一语言的服务器时，可能会出现功能重叠。为确保使用您偏好的功能，Zed 允许您设定语言服务器的优先级及调用顺序。

您可通过 `language_servers` 配置项指定偏好设置：

```json [settings]
  "languages": {
    "PHP": {
      "language_servers": ["intelephense", "!phpactor", "..."]
    }
  }
```

此示例中：

- `intelephense` 被设为主语言服务器
- `phpactor` 已禁用（注意 `!` 前缀）
- `...` 扩展为其他已注册的 PHP 语言服务器

此配置允许您根据具体需求定制语言服务器设置，确保为开发工作流程提供最合适的功能。

### 工具链

某些语言服务器需要配置当前"工具链"，即特定版本编程语言编译器或/和解释器的安装环境，其中可能包含项目的完整依赖项集合。例如，Zed 将 Python 的虚拟环境视为一种工具链。并非 Zed 中的所有语言都支持工具链发现和选择，但对于支持的语言，您可以通过工具链选择器（使用 {#action toolchain::Select} 操作）指定工具链。要了解更多关于 Zed 中工具链的信息，请参阅 [`toolchains`](./toolchains.md)。

### 配置语言服务器

许多语言服务器接受自定义配置选项。您可以在 `settings.json` 的 `lsp` 部分进行设置：

```json [settings]
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        "check": {
          "command": "clippy"
        }
      }
    }
  }
```

此示例配置了 Rust Analyzer 在保存文件时使用 Clippy 进行额外的代码检查。

#### 嵌套对象

在 Zed 中配置语言服务器选项时，务必使用嵌套对象而非点分隔字符串。这在处理复杂配置时尤为重要。让我们通过 TypeScript 语言服务器的实际示例来说明：

假设您需要为 TypeScript 配置以下设置：
- 启用严格空值检查
- 将目标 ECMAScript 版本设置为 ES2020

以下是在 Zed 的 `settings.json` 中配置这些设置的正确结构：

```json [settings]
"lsp": {
  "typescript-language-server": {
    "initialization_options": {
      // These are not supported (VSCode dotted style):
      // "preferences.strictNullChecks": true,
      // "preferences.target": "ES2020"
      //
      // These is correct (nested notation):
      "preferences": {
        "strictNullChecks": true,
        "target": "ES2020"
      },
    }
  }
}
```

#### 可能的配置选项

根据具体语言服务器的实现方式，它们可能依赖不同的配置选项，这些选项均在LSP规范中定义。

- [初始化选项](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#version_3_17_0)

在语言服务器启动期间仅发送一次，需要重启服务器才能使更改生效。

例如，rust-analyzer和clangd仅依赖此种配置方式。

```json [settings]
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        "checkOnSave": false
      }
    }
  }
```

- [配置请求](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_configuration)

服务器可能会多次查询此配置。
大多数服务器主要依赖这种方式进行配置。

```json [settings]
"lsp": {
  "tailwindcss-language-server": {
    "settings": {
      "tailwindCSS": {
        "emmetCompletions": true,
      },
    }
  }
}
```

除了与LSP相关的服务器配置选项外，Zed中的某些服务器还支持配置Zed启动二进制文件的方式。

如果语言服务器能在您的路径中找到，Zed会自动下载或启动它们。若希望指定其他特定的二进制文件，您可以在设置中进行配置：

```json [settings]
  "lsp": {
    "rust-analyzer": {
      "binary": {
        // Whether to fetch the binary from the internet, or attempt to find locally.
        "ignore_system_version": false,
        "path": "/path/to/langserver/bin",
        "arguments": ["--option", "value"],
        "env": {
          "FOO": "BAR"
        }
      }
    }
  }
```

### 启用或禁用语言服务器

您可以全局或按语言切换语言服务器支持：

```json [settings]
  "languages": {
    "Markdown": {
      "enable_language_server": false
    }
  }
```

这将禁用Markdown文件的语言服务器功能，在处理大型文档项目时有助于提升性能。您可以在全局配置文件`~/.config/zed/settings.json`中设置此选项，也可以在项目目录的`.zed/settings.json`文件内进行配置。

## 代码格式化与静态检查

Zed 提供代码格式化和静态检查功能，帮助保持统一的代码风格并及早发现潜在问题。

### 配置格式化工具

Zed 支持内置和外部格式化工具。更多信息请参阅 [`formatter`](./configuring-zed.md#formatter) 文档。您可以在 `settings.json` 中全局或按语言配置格式化工具：

```json [settings]
"languages": {
  "JavaScript": {
    "formatter": {
      "external": {
        "command": "prettier",
        "arguments": ["--stdin-filepath", "{buffer_path}"]
      }
    },
    "format_on_save": "on"
  },
  "Rust": {
    "formatter": "language_server",
    "format_on_save": "on"
  }
}
```

此示例使用 Prettier 处理 JavaScript，并使用语言服务器的格式化工具处理 Rust，两者均设置为保存时自动格式化。

若要禁用特定语言的格式化功能：

```json [settings]
"languages": {
  "Markdown": {
    "format_on_save": "off"
  }
}
```

### 设置代码检查工具

Zed 中的代码检查通常由语言服务器处理。许多语言服务器允许您配置代码检查规则：

```json [settings]
"lsp": {
  "eslint": {
    "settings": {
      "codeActionOnSave": {
        "rules": ["import/order"]
      }
    }
  }
}
```

此配置用于在保存 JavaScript 文件时自动整理导入语句。

要实现保存时自动运行代码检查修复：

```json [settings]
"languages": {
  "JavaScript": {
    "formatter": {
      "code_action": "source.fixAll.eslint"
    }
  }
}
```

### 格式化与代码检查的集成

Zed 支持在保存时同时执行格式化和代码检查。以下示例使用 Prettier 进行格式化，ESLint 进行 JavaScript 文件检查：

```json [settings]
"languages": {
  "JavaScript": {
    "formatter": [
      {
        "code_action": "source.fixAll.eslint"
      },
      {
        "external": {
          "command": "prettier",
          "arguments": ["--stdin-filepath", "{buffer_path}"]
        }
      }
    ],
    "format_on_save": "on"
  }
}
```

### 故障排除

若遇到格式化或代码检查相关问题：

1. 检查 Zed 日志文件中的错误信息（使用命令面板：`zed: open log`）
2. 确保外部工具（格式化程序、代码检查工具）已正确安装并位于 PATH 环境变量中
3. 验证 Zed 设置和语言特定配置文件中的配置（例如 `.eslintrc`、`.prettierrc`）

## 语法高亮与主题

Zed 提供语法高亮和主题的自定义选项，让您能够定制代码的视觉呈现效果。

### 自定义语法高亮

Zed 使用 Tree-sitter 语法解析器实现语法高亮。可通过 `experimental.theme_overrides` 设置覆盖默认高亮样式。

以下示例将注释设置为斜体并修改字符串颜色：

```json [settings]
"experimental.theme_overrides": {
  "syntax": {
    "comment": {
      "font_style": "italic"
    },
    "string": {
      "color": "#00AA00"
    }
  }
}
```

### 选择与自定义主题

切换主题：

1. 使用主题选择器（{#kb theme_selector::Toggle}）
2. 或在 `settings.json` 中设置：

```json [settings]
"theme": {
  "mode": "dark",
  "dark": "One Dark",
  "light": "GitHub Light"
}
```

通过在 `~/.config/zed/themes/` 目录中创建 JSON 文件来自定义主题。Zed 会自动检测该目录中的所有主题并使其可用。

### 使用主题扩展

Zed 支持主题扩展功能。可通过扩展面板（{#kb zed::Extensions}）浏览并安装主题扩展。

要创建自己的主题扩展，请参阅[开发主题扩展](./extensions/themes.md)指南。

## 使用语言服务器功能

### 内联提示

内联提示会在代码行内显示额外信息，例如参数名称或推断类型。在 `settings.json` 中配置内联提示：

```json [settings]
"inlay_hints": {
  "enabled": true,
  "show_type_hints": true,
  "show_parameter_hints": true,
  "show_other_hints": true
}
```

关于语言特定的内联提示设置，请参阅各语言的文档说明。

### 代码操作

代码操作提供快速修复和重构选项。当出现可用操作时，可通过`editor: Toggle Code Actions`命令或点击光标旁显示的灯泡图标来访问代码操作功能。

### 转到定义与引用查找

使用以下命令在代码库中导航：

- `editor: Go to Definition`（<kbd>f12|f12</kbd>）
- `editor: Go to Type Definition`（<kbd>cmd-f12|ctrl-f12</kbd>）
- `editor: Find All References`（<kbd>shift-f12|shift-f12</kbd>）

### 符号重命名

要在整个项目中重命名符号：

1. 将光标置于符号上
2. 使用`editor: Rename Symbol`命令（<kbd>f2|f2</kbd>）
3. 输入新名称并按回车键

这些功能的可用性取决于各语言语言服务器的支持能力。

当重命名一个跨多个文件的符号时，Zed 会在多缓冲区中打开预览界面。这使您能在应用修改前，完整审阅项目中所有相关变更。只需保存多缓冲区即可确认重命名操作。若决定放弃重命名，您可以选择撤销更改或直接关闭多缓冲区而不保存。

### 悬停信息

使用 `editor: Hover` 命令可显示光标所在符号的详细信息，通常包含类型说明、文档说明及相关资源链接。

### 工作区符号搜索

通过 `workspace: Open Symbol` 命令可在整个项目中搜索符号（函数、类、变量），这对快速浏览大型代码库特别实用。

### 代码补全

Zed 会在您输入时提供智能代码补全建议。您也可通过 `editor: Show Completions` 命令手动触发补全功能，使用 <kbd>tab|tab</kbd> 或 <kbd>enter|enter</kbd> 键即可采纳建议。

### 诊断

语言服务器会在您编写代码时提供实时诊断信息（错误、警告、提示）。使用`diagnostics: Toggle`命令可查看项目的所有诊断结果。