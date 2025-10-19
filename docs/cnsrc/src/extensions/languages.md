# 语言扩展

Zed 的语言支持包含以下几个组成部分：

- 语言元数据与配置
- 语法解析
- 查询系统
- 语言服务器

## 语言元数据

Zed 支持的每种语言都必须在扩展的 `languages` 目录下的子目录中进行定义。

该子目录必须包含一个名为 `config.toml` 的文件，其结构如下：

```toml
name = "My Language"
grammar = "my-language"
path_suffixes = ["myl"]
line_comments = ["# "]
```

- `name`（必需）是显示在语言选择下拉菜单中的人类可读名称。
- `grammar`（必需）是语法规则的名称。语法规则需单独注册，具体说明见下文。
- `path_suffixes` 是与该语言关联的文件后缀数组。与设置中的 `file_types` 不同，此处不支持通配符模式。
- `line_comments` 是用于识别语言中行注释的字符串数组。该设置用于 `editor::ToggleComments` 快捷键：{#kb editor::ToggleComments} 来切换代码行的注释状态。
- `tab_size` 定义该语言使用的缩进/制表符大小（默认为 `4`）。
- `hard_tabs` 指定使用制表符（`true`）还是空格（`false`，默认值）进行缩进。
- `first_line_pattern` 是一个正则表达式，除了设置中的 `path_suffixes`（上文提及）或 `file_types` 外，还可用于匹配应使用此语言的文件。例如，Zed 通过匹配脚本首行的 [Shebang 行](https://github.com/zed-industries/zed/blob/main/crates/languages/src/bash/config.toml) 来识别 Shell 脚本。
- `debuggers` 是用于识别语言调试器的字符串数组。当启动调试器的 `New Process Modal` 时，Zed 将按此数组中的条目顺序对可用调试器进行排序。

## 语法

Zed采用[Tree-sitter](https://tree-sitter.github.io)解析库来提供内置的编程语言专属功能。目前已有多种语言的语法解析器可供使用，您也可以[自行开发语法解析器](https://tree-sitter.github.io/tree-sitter/creating-parsers#writing-the-grammar)。通过Tree-sitter查询对语法树进行模式匹配，Zed正在不断扩展基于此的功能特性。如前所述，每个在扩展中定义的语言都必须指定用于解析的Tree-sitter语法解析器名称。这些语法解析器会在扩展的`extension.toml`文件中单独注册，具体示例如下：

```toml
[grammars.gleam]
repository = "https://github.com/gleam-lang/tree-sitter-gleam"
rev = "58b7cac8fc14c92b0677c542610d8738c373fa81"
```

`repository` 字段必须指定一个应从中加载 Tree-sitter 语法的代码库，而 `rev` 字段必须包含要使用的 Git 修订版本，例如 Git 提交的 SHA。如果您在本地开发扩展并希望从本地文件系统加载语法，则可以为 `repository` 使用 `file://` URL。一个扩展可以通过引用多个 Tree-sitter 代码库来提供多种语法。

## Tree-sitter 查询

Zed 使用 [Tree-sitter](https://tree-sitter.github.io) 查询语言生成的语法树来实现以下功能：

- 语法高亮
- 括号匹配
- 代码大纲/结构
- 自动缩进
- 代码注入
- 语法覆盖
- 文本隐藏
- 可运行代码检测
- 选择类、函数等

以下章节详细阐述 [Tree-sitter 查询](https://tree-sitter.github.io/tree-sitter/using-parsers#query-syntax)如何通过 [JSON 语法](https://www.json.org/json-en.html)示例在 Zed 中实现这些功能。

### 语法高亮

在 Tree-sitter 中，`highlights.scm` 文件用于定义特定语法的语法高亮规则。

以下是一个 JSON `highlights.scm` 的示例：

```scheme
(string) @string

(pair
  key: (string) @property.json_key)

(number) @number
```

该查询会标记需要高亮的字符串、对象键名和数值。以下是主题支持的完整捕获类型列表：

| 捕获项                  | 描述                             |
| ------------------------ | -------------------------------- |
| @attribute               | 捕获属性                         |
| @boolean                 | 捕获布尔值                       |
| @comment                 | 捕获注释                         |
| @comment.doc             | 捕获文档注释                     |
| @constant                | 捕获常量                         |
| @constructor             | 捕获构造函数                     |
| @embedded                | 捕获嵌入内容                     |
| @emphasis                | 捕获强调文本                     |
| @emphasis.strong         | 捕获强烈强调文本                 |
| @enum                    | 捕获枚举                         |
| @function                | 捕获函数                         |
| @hint                    | 捕获提示                         |
| @keyword                 | 捕获关键字                       |
| @label                   | 捕获标签                         |
| @link_text               | 捕获链接文本                     |
| @link_uri                | 捕获链接地址                     |
| @number                  | 捕获数值                         |
| @operator                | 捕获操作符                       |
| @predictive              | 捕获预测文本                     |
| @preproc                 | 捕获预处理器指令                 |
| @primary                 | 捕获主要元素                     |
| @property                | 捕获属性                         |
| @punctuation             | 捕获标点符号                     |
| @punctuation.bracket     | 捕获括号                         |
| @punctuation.delimiter   | 捕获分隔符                       |
| @punctuation.list_marker | 捕获列表标记                     |
| @punctuation.special     | 捕获特殊标点                     |
| @string                  | 捕获字符串字面量                 |
| @string.escape           | 捕获字符串中的转义字符           |
| @string.regex            | 捕获正则表达式                   |
| @string.special          | 捕获特殊字符串                   |
| @string.special.symbol   | 捕获特殊符号                     |
| @tag                     | 捕获标签                         |
| @tag.doctype             | 捕获文档类型声明（如HTML中的）   |
| @text.literal            | 捕获字面文本                     |
| @title                   | 捕获标题                         |
| @type                    | 捕获类型                         |
| @variable                | 捕获变量                         |
| @variable.special        | 捕获特殊变量                     |
| @variant                 | 捕获变体                         |

### 括号匹配

`brackets.scm` 文件定义了匹配括号的规则。

以下是一个来自 `brackets.scm` 文件的 JSON 示例：

```scheme
("[" @open "]" @close)
("{" @open "}" @close)
("\"" @open "\"" @close)
```

该查询用于识别开括号、闭括号、大括号以及引号。

| 捕获组   | 描述                                       |
| -------- | ------------------------------------------ |
| @open    | 捕获开括号、大括号和引号                   |
| @close   | 捕获闭括号、大括号和引号                   |

### 代码大纲/结构

`outline.scm` 文件定义了代码大纲的结构。

以下是一个来自 `outline.scm` 文件的 JSON 示例：

```scheme
(pair
  key: (string (string_content) @name)) @item
```

该查询捕获用于大纲结构的对象键。

| 捕获项          | 描述                                                                 |
| ---------------- | -------------------------------------------------------------------- |
| @name            | 捕获对象键的内容                                                     |
| @item            | 捕获完整的键值对                                                     |
| @context         | 捕获为大纲条目提供上下文的元素                                       |
| @context.extra   | 捕获大纲条目的额外上下文信息                                         |
| @annotation      | 捕获标注大纲条目的节点（文档注释、特性、装饰器）[^1]                 |

[^1]: 助手在生成代码修改步骤时会使用这些标注。

### 自动缩进

`indents.scm` 文件定义了缩进规则。

以下是一个来自 `indents.scm` 文件的 JSON 配置示例：

[[代码块_0]]

此查询标志着数组和对象的结束，用于缩进目的。

| 捕获项   | 描述                                        |
| -------- | ------------------------------------------- |
| @end     | 捕获闭合括号和大括号                        |
| @indent  | 捕获整个数组和对象以进行缩进                |

### 代码注入

[[代码块_2]] 文件定义了将一种语言嵌入到另一种语言中的规则，例如 Markdown 中的代码块或 Python 字符串中的 SQL 查询。

以下是一个来自 [[代码块_3]] 文件的 Markdown 示例：

[[代码块_1]]

此查询用于识别围栏代码块，捕获信息字符串中指定的语言及块内内容。同时也会捕获行内内容并将其语言设为"markdown-inline"。

| 捕获项               | 描述                                           |
| -------------------- | ---------------------------------------------- |
| @injection.language  | 捕获代码块的语言标识符                         |
| @injection.content   | 捕获需视为其他语言处理的内容                   |

请注意，此处无法使用JSON作为示例，因为该格式不支持语言注入功能。

### 语法覆盖

`overrides.scm`文件定义了语法作用域，可用于在特定语言结构内覆盖某些编辑器设置。

例如，有一种语言特定设置名为`word_characters`，用于控制哪些非字母字符被视为单词的一部分，比如双击选择变量时。在JavaScript中，"$"和"#"会被识别为单词字符。

另有一种语言特定设置名为`completion_query_characters`，用于控制哪些字符触发自动补全提示。在JavaScript中，当光标位于字符串内时，"-"应被识别为补全查询字符。为实现这一功能，JavaScript的`overrides.scm`文件包含以下模式：

```scheme
[
  (string)
  (template_string)
] @string
```

同时JavaScript的`config.toml`文件包含如下设置：

```toml
word_characters = ["#", "$"]

[overrides.string]
completion_query_characters = ["-"]
```

你也可以在特定作用域中禁用某些自动补全括号。例如，若要在字符串内阻止自动补全 `'`，可以在 JavaScript 的 `config.toml` 中添加以下内容：

```toml
brackets = [
  { start = "'", end = "'", close = true, newline = false, not_in = ["string"] },
  # other pairs...
]
```

#### 范围包含性

默认情况下，`overrides.scm` 中定义的范围是**不包含边界**的。因此，在上述情况下，如果光标位于字符串引号**之外**，`string` 作用域将不会生效。有时，你可能希望将范围设为**包含边界**。可以通过在查询中的捕获名称后添加 `.inclusive` 后缀来实现。

例如，在 JavaScript 中，我们还会禁用注释内单引号的自动补全。而注释作用域必须一直延伸到行注释后的换行符。为实现这一点，JavaScript 的 `overrides.scm` 包含以下模式：

```scheme
(comment) @comment.inclusive
```

### 文本对象

`textobjects.scm` 文件定义了基于文本对象的导航规则。该功能于 Zed v0.165 版本加入，目前仅用于 Vim 模式。

Vim 为文件导航提供了两种粒度级别：通过 `[]` 等实现按章节导航，通过 `]m` 等实现按方法导航。即使是不支持函数和类的语言，也可以通过定义类似概念实现良好适配。例如 CSS 将规则集定义为方法，将媒体查询定义为类。

对于支持闭包的语言，这些闭包通常不应被 Zed 视为函数。但这是尽力而为的优化，因为像 JavaScript 这类语言在语法层面并未区分闭包和顶层函数声明。

对于类似 C 的声明式语言，需提供匹配 `@class.around` 或 `@function.around` 的查询语句。若不存在内部作用域，`if` 和 `ic` 文本对象将默认采用这些规则。

如果你不确定在 textobjects.scm 中该写什么，可以参考 [nvim-treesitter-textobjects](https://github.com/nvim-treesitter/nvim-treesitter-textobjects) 和 [Helix 编辑器](https://github.com/helix-editor/helix)，它们为多种语言提供了查询配置。你也可以查阅 Zed 的[内置语言配置](https://github.com/zed-industries/zed/tree/main/crates/languages/src)，了解如何适配这些规则。

| 捕获范围        | 描述                                                                 | Vim 模式                                                                         |
|-----------------|----------------------------------------------------------------------|---------------------------------------------------------------------------------|
| @函数.外围       | 完整函数定义或文件中对应的独立小节                                       | [[代码块0]]、[[代码块1]]、[[代码块2]]、[[代码块3]] 移动指令。[[代码块4]] 文本对象 |
| @函数.内部       | 函数体内容（大括号内的部分）                                             | [[代码块5]] 文本对象                                                             |
| @类.外围         | 完整类定义或文件中对应的较大区块                                         | [[代码块6]]、[[代码块7]]、[[代码块8]]、[[代码块9]] 移动指令。[[代码块10]] 文本对象 |
| @类.内部         | 类定义内部包含的内容                                                     | [[代码块11]] 文本对象                                                            |
| @注释.外围       | 完整注释块（例如相邻的行注释组或整段块注释）                               | [[代码块12]] 文本对象                                                            |
| @注释.内部       | 注释块内部包含的内容                                                     | [[代码块13]] 文本对象（较少被支持）                                               |

例如：

```scheme
; include only the content of the method in the function
(method_definition
    body: (_
        "{"
        (_)* @function.inside
        "}")) @function.around

; match function.around for declarations with no body
(function_signature_item) @function.around

; join all adjacent comments into one
(comment)+ @comment.around
```

### 文本脱敏规则

`redactions.scm` 文件定义了文本脱敏规则。在协作和屏幕共享时，该规则确保特定语法节点以脱敏模式呈现，避免信息泄露。

以下是一个针对 JSON 的 `redactions.scm` 文件示例：

```scheme
(pair value: (number) @redact)
(pair value: (string) @redact)
(array (number) @redact)
(array (string) @redact)
```

该查询会标记键值对和数组中需要脱敏的数值与字符串内容。

| 捕获节点   | 说明                     |
| ---------- | ------------------------ |
| @redact    | 捕获需要脱敏的数值内容   |

### 可运行代码检测

`runnables.scm` 文件定义了检测可运行代码的规则。

以下是一个来自 `runnables.scm` 文件的 JSON 示例：

```scheme
(
    (document
        (object
            (pair
                key: (string
                    (string_content) @_name
                    (#eq? @_name "scripts")
                )
                value: (object
                    (pair
                        key: (string (string_content) @run @script)
                    )
                )
            )
        )
    )
    (#set! tag package-script)
    (#set! tag composer-script)
)
```

此查询用于检测 package.json 和 composer.json 文件中的可运行脚本。

`@run` 捕获指定了运行按钮在编辑器中的显示位置。其他捕获（除了以下划线为前缀的捕获）在运行代码时会作为环境变量暴露，并带有 `ZED_CUSTOM_$(capture_name)` 前缀。

| 捕获项   | 描述                                           |
| -------- | ---------------------------------------------- |
| @\_name  | 捕获 "scripts" 键                              |
| @run     | 捕获脚本名称                                   |
| @script  | 同样捕获脚本名称（用于不同目的）               |

<!--
TBD: [[CODE_BLOCK_1]]
-->

## 语言服务器

Zed 采用[语言服务器协议](https://microsoft.github.io/language-server-protocol/)提供高级语言支持。

扩展可提供任意数量的语言服务器。若要通过扩展提供语言服务器，请在 `extension.toml` 中添加对应条目，注明语言服务器名称及其适用的语言。`languages` 列表中的条目必须与该语言 `config.toml` 文件中的 `name` 字段相匹配：

```toml
[language_servers.my-language-server]
name = "My Language LSP"
languages = ["My Language"]
```

接着，在你的扩展 Rust 代码中，实现 `language_server_command` 方法：

```rust
impl zed::Extension for MyExtension {
    fn language_server_command(
        &mut self,
        language_server_id: &LanguageServerId,
        worktree: &zed::Worktree,
    ) -> Result<zed::Command> {
        Ok(zed::Command {
            command: get_path_to_language_server_executable()?,
            args: get_args_for_language_server()?,
            env: get_env_for_language_server()?,
        })
    }
}
```

你可以通过 `Extension` 特性中的多个可选方法来自定义语言服务器的处理方式。例如，你可以使用 `label_for_completion` 方法来控制补全项的显示样式。完整的方法列表请参阅 [Zed 扩展 API 文档](https://docs.rs/zed_extension_api)。

### 多语言支持

如果你的语言服务器支持更多语言，你可以使用 `language_ids` 将 Zed `languages` 映射到所需的 [LSP 特定 `languageId`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocumentItem) 标识符：

```toml

[language-servers.my-language-server]
name = "Whatever LSP"
languages = ["JavaScript", "HTML", "CSS"]

[language-servers.my-language-server.language_ids]
"JavaScript" = "javascript"
"TSX" = "typescriptreact"
"HTML" = "html"
"CSS" = "css"
```