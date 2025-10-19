# 配置支持的语言

Zed 为支持的每种编程语言提供了强大的自定义选项。本指南将带您了解多种方式，让您能够根据个人偏好和项目需求来定制编码体验。

Zed 的语言支持基于两大核心技术构建：

1. Tree-sitter：负责语法高亮和基于结构的功能，如大纲面板。
2. 语言服务器协议（LSP）：提供语义功能，例如代码补全和诊断。

这些组件协同工作，共同支撑 Zed 的语言功能。

在本指南中，我们将涵盖：

- 特定语言设置
- 文件关联
- 使用语言服务器
- 格式化和代码检查配置
- 自定义语法高亮和主题
- 高级语言功能

通过本指南的学习，您将掌握如何在 Zed 中配置和自定义支持的语言。

如需查看 Zed 支持的语言完整列表及其具体配置，请访问我们的[支持的语言](./languages.md)页面。若想进一步探索，您可以尝试开发自己的扩展来增加对新语言的支持或增强现有功能。有关创建语言扩展的详细信息，请参阅我们的[语言扩展](./extensions/languages.md)指南。

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

- [[[代码块_0]]](./configuring-zed.md#tab-size)：每个缩进级别的空格数
- [[[代码块_1]]](./configuring-zed.md#formatter)：用于代码格式化的工具
- [[[代码块_2]]](./configuring-zed.md#format-on-save)：保存时是否自动格式化代码
- [[[代码块_3]]](./configuring-zed.md#enable-language-server)：切换语言服务器支持
- [[[代码块_4]]](./configuring-zed.md#hard-tabs)：使用制表符而非空格进行缩进
- [[[代码块_5]]](./configuring-zed.md#preferred-line-length)：建议的最大行长度
- [[[代码块_6]]](./configuring-zed.md#soft-wrap)：长代码行的换行方式
- [[[代码块_7]]](./configuring-zed.md#show-completions-on-input)：输入时是否显示补全建议
- [[[代码块_8]]](./configuring-zed.md#show-completion-documentation)：是否在补全菜单中显示内联及侧边文档说明

这些设置可帮助您在不同语言和项目中保持特定的编码风格。

## 文件关联

Zed 会根据文件扩展名自动检测文件类型，但您可以根据工作流程自定义这些关联。

要设置自定义文件关联，请在 `settings.json` 中使用 [`file_types`](./configuring-zed.md#file-types) 设置：

```json [settings]
"file_types": {
  "C++": ["c"],
  "TOML": ["MyLockFile"],
  "Dockerfile": ["Dockerfile*"]
}
```

此配置将指示 Zed：

- 将 `.c` 文件识别为 C++ 而非 C 语言
- 将名为 "MyLockFile" 的文件识别为 TOML 格式
- 对任何以 "Dockerfile" 开头的文件应用 Dockerfile 语法

您可以使用通配符模式实现更灵活的匹配，从而处理项目中复杂的命名规范。

## 语言服务器配置

语言服务器是 Zed 智能编码功能的核心组成部分，可提供自动补全、跳转到定义、实时错误检查等能力。

### 什么是语言服务器？

语言服务器实现了语言服务器协议（LSP），该协议标准化了编辑器与语言特定工具之间的通信。这使得 Zed 能够支持多种编程语言的高级功能，而无需单独实现每个功能。

语言服务器提供的一些关键功能包括：

- 代码补全
- 错误检查与诊断
- 代码导航（跳转到定义、查找引用）
- 代码操作（重命名、提取方法）
- 悬停信息
- 工作区符号搜索

### 管理语言服务器

Zed 为用户简化了语言服务器的管理：

1. 自动下载：当您打开具有匹配文件类型的文件时，Zed 会自动下载相应的语言服务器。对于已知文件类型，Zed 可能会提示您安装扩展。

2. 存储位置：

- macOS：[[代码块1]]
- Linux：[[代码块2]]、[[代码块3]] 或 [[代码块4]]

3. 自动更新：Zed 会保持您的语言服务器处于最新状态，确保您始终拥有最新的功能和改进。

### 选择语言服务器

Zed 中某些语言提供多种语言服务器选项。您可能安装了多个包含针对同一语言的服务器扩展，这可能导致功能重叠。为确保获得您偏好的功能，Zed 允许您设定语言服务器的使用优先级及顺序。

您可以通过 [[代码块5]] 设置来指定偏好：

[[代码块0]]

在此示例中：

- `intelephense` 被设为主要语言服务器
- `phpactor` 已被禁用（请注意 `!` 前缀）
- `...` 扩展为其余已注册的 PHP 语言服务器

此配置允许您根据具体需求定制语言服务器设置，确保为开发工作流程提供最合适的功能。

### 工具链

某些语言服务器需要配置当前的“工具链”，即特定版本编程语言编译器或/和解释器的安装环境，其中可能包含项目的完整依赖项集合。例如，Zed 将 Python 中的虚拟环境视为一种工具链。并非 Zed 中的所有语言都支持工具链的发现和选择，但对于支持的语言，您可以通过工具链选择器（通过 {#action toolchain::Select}）指定工具链。要了解更多关于 Zed 中工具链的信息，请参阅 [`toolchains`](./toolchains.md)。

### 配置语言服务器

许多语言服务器接受自定义配置选项。您可以在 `settings.json` 的 `lsp` 部分设置这些选项：

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

此示例配置 Rust Analyzer 在保存文件时使用 Clippy 进行额外的代码检查。

#### 嵌套对象

在 Zed 中配置语言服务器选项时，务必使用嵌套对象而非点分隔字符串。这在处理复杂配置时尤为重要。让我们通过 TypeScript 语言服务器的实际示例来说明：

假设您需要为 TypeScript 配置以下设置：
- 启用严格空值检查
- 设置目标 ECMAScript 版本为 ES2020

以下是在 Zed 的 `settings.json` 中配置这些设置的结构：

#### 可能的配置选项

根据特定语言服务器的实现方式，它们可能依赖不同的配置选项，这些选项均在 LSP 规范中定义。

- [initializationOptions](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#version_3_17_0)

该选项仅在语言服务器启动时发送一次，需要重启服务器才能使更改生效。

例如，rust-analyzer 和 clangd 仅依赖此种配置方式。

[[代码块_0]]

- [配置请求](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_configuration)

服务器可能会多次查询此配置。
大多数服务器仅依赖这种方式进行配置。

[[代码块_1]]

除了与LSP相关的服务器配置选项外，Zed中的某些服务器还允许配置Zed启动二进制文件的方式。

如果语言服务器在您的路径中被找到，它们将自动下载或启动。如果您希望指定一个明确的替代二进制文件，可以在设置中进行配置：

### 启用或禁用语言服务器

你可以全局或按语言切换语言服务器支持：

```json [settings]
  "languages": {
    "Markdown": {
      "enable_language_server": false
    }
  }
```

这将禁用 Markdown 文件的语言服务器，对于大型文档项目的性能优化很有帮助。你可以在全局的 `~/.config/zed/settings.json` 中配置此设置，或在项目目录内的 `.zed/settings.json` 中进行配置。

## 格式化与代码检查

Zed 提供代码格式化和代码检查功能，以保持一致的代码风格并及早发现潜在问题。

### 配置格式化工具

Zed 同时支持内置与外部格式化工具。更多信息请查阅 [`formatter`](./configuring-zed.md#formatter) 文档。您可以在 `settings.json` 中全局或按语言配置格式化工具：

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

此示例配置了在保存时使用 Prettier 格式化 JavaScript，并使用语言服务器自带的格式化工具处理 Rust 代码。

若需禁用特定语言的格式化功能：

```json [settings]
"languages": {
  "Markdown": {
    "format_on_save": "off"
  }
}
```

### 配置代码检查工具

Zed 的代码检查功能通常由语言服务器实现。多数语言服务器支持自定义检查规则：

此配置设定 ESLint 在保存时自动整理 JavaScript 文件的导入顺序。

要实现保存时自动运行 linter 修复：

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

Zed 支持在保存时同时执行代码格式化与检查。以下示例展示如何通过 Prettier 格式化 JavaScript 文件，并使用 ESLint 进行代码检查：

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

1. 检查 Zed 的日志文件以查找错误信息（使用命令面板：`zed: open log`）
2. 确保外部工具（格式化程序、代码检查工具）已正确安装并位于您的 PATH 环境变量中
3. 验证 Zed 设置和特定语言配置文件中的配置（例如，`.eslintrc`、`.prettierrc`）

## 语法高亮与主题

Zed 提供了语法高亮和主题的自定义选项，让您能够定制代码的视觉外观。

### 自定义语法高亮

Zed 使用 Tree-sitter 语法解析器进行语法高亮。您可以使用 `experimental.theme_overrides` 设置来覆盖默认的高亮效果。

以下示例将注释设置为斜体并更改字符串的颜色：

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

更改您的主题：

1. 使用主题选择器 ({#kb theme_selector::Toggle})
2. 或在 `settings.json` 中设置：

```json [settings]
"theme": {
  "mode": "dark",
  "dark": "One Dark",
  "light": "GitHub Light"
}
```

通过在 `~/.config/zed/themes/` 目录中创建 JSON 文件来自定义主题。Zed 会自动检测此目录中的所有主题并使其可用。

### 使用主题扩展

Zed 支持主题扩展。您可以在扩展面板 ({#kb zed::Extensions}) 中浏览并安装主题扩展。

要创建自己的主题扩展，请参阅[开发主题扩展](./extensions/themes.md)指南。

## 使用语言服务器功能

### 内联提示

内联提示会在代码行内提供额外信息，例如参数名称或推断类型。在 `settings.json` 中配置内联提示：

```json [settings]
"inlay_hints": {
  "enabled": true,
  "show_type_hints": true,
  "show_parameter_hints": true,
  "show_other_hints": true
}
```

如需了解特定语言的嵌入提示设置，请参阅各语言的专属文档。

### 代码操作功能

代码操作提供快速修复与重构选项。当出现可用操作时，您可通过 `editor: Toggle Code Actions` 命令进行访问，或直接点击光标旁出现的灯泡图标。

### 转到定义与引用追踪

使用以下命令在代码库中导航：

- `editor: Go to Definition`（<kbd>f12|f12</kbd>）
- `editor: Go to Type Definition`（<kbd>cmd-f12|ctrl-f12</kbd>）
- `editor: Find All References`（<kbd>shift-f12|shift-f12</kbd>）

### 符号重命名

若要在整个项目中重命名符号：

1. 将光标置于符号位置
2. 使用 `editor: Rename Symbol` 命令（<kbd>f2|f2</kbd>）
3. 输入新名称并按下回车键

上述功能的可用性取决于各语言服务器对相应功能的支持程度。

当重命名一个跨多个文件的符号时，Zed 会在多缓冲区中打开预览。这使您可以在应用更改前查看项目中所有相关修改。要确认重命名，只需保存多缓冲区即可。若决定不执行重命名，您可以通过撤销更改或直接关闭多缓冲区（不保存）来取消操作。

### 悬停信息

使用 `editor: Hover` 命令可显示光标所在符号的相关信息，通常包含类型说明、文档注释及相关资源链接。

### 工作区符号搜索

通过 `workspace: Open Symbol` 命令可在整个项目中搜索符号（包括函数、类、变量等），该功能特别适合在大型代码库中快速定位目标。

### 代码补全

Zed 会在您输入时提供智能代码补全建议。您也可以使用 `editor: Show Completions` 命令手动触发补全功能，并通过 <kbd>tab|tab</kbd> 或 <kbd>enter|enter</kbd> 快捷键确认采纳建议。

### 诊断功能

语言服务器会在您编写代码时提供实时诊断信息（错误、警告、提示）。使用 `diagnostics: Toggle` 命令即可查看项目中所有诊断信息。