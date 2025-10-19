# 文本对话串

## 概述 {#overview}

[智能体面板](./agent-panel.md)中的文本对话串与其他编辑器功能相似。您可以使用自定义快捷键绑定并同时操作多个光标，实现在编写代码与语言模型对话之间无缝切换。

其独特之处在于引入了消息区块功能。这些区块作为文本容器，对应对话中的不同角色身份，主要包括：

- `You`
- `Assistant`
- `System`

请先在 `You` 区块中输入消息开始对话。

![提问示意图](https://zed.dev/img/assistant/ask-a-question.png)

输入时，界面会实时更新所选模型的剩余令牌计数。

若要从编辑器插入文本，只需选中文本后执行 `agent: quote selection` ({#kb agent::QuoteSelection}) 命令；若内容为代码，Zed 会自动将其包裹在代码块中。

![引用选中内容示意图](https://zed.dev/img/assistant/quoting-a-selection.png)

要发送消息，请使用 {#kb assistant::Assist}(`assistant: assist`)。与普通对话线程中按 <kbd>回车键</kbd>直接发送消息不同，在文本线程中，我们的目标是让体验尽可能接近常规编辑器。因此，按 {#kb editor::Newline} 仅会插入新行。

提交消息后，回复将以 `Assistant` 消息块的形式在下方流式呈现。

![接收回答](https://zed.dev/img/assistant/receiving-an-answer.png)

您随时可以按 <kbd>ESC键</kbd> 取消信息流。若发现回复不符合预期，这个功能会非常实用。

如需随时开启新对话，可以按 <kbd>Cmd-N|Ctrl-N</kbd> 组合键，或通过面板左上角汉堡菜单中的 `New Chat` 选项进行操作。

简单的来回对话很适合文本线程。不过，有时你可能想要修改对话中的先前文本，并将其引导至不同的方向。

## 编辑文本线程 {#edit-text-thread}

文本线程让你能够灵活地控制上下文。
你可以自由编辑任何先前的文本，包括来自大语言模型的回复。
如果想要完全删除某个消息块，只需将光标置于该消息块的开头，然后按下 `delete` 键。
一个典型的工作流程可能涉及在整个上下文中进行编辑和调整，以优化你的查询或提供额外信息。以下是一个示例：

1. 在 `You` 块中编写文本。
2. 使用 {#kb assistant::Assist} 提交消息。
3. 收到不符合预期的 `Assistant` 响应。
4. 使用 <kbd>escape</kbd> 取消响应。
5. 清除 `Assistant` 消息块的内容并完全移除该块。
6. 向原始消息中添加额外上下文。
7. 使用 {#kb assistant::Assist} 提交消息。

能够编辑先前的消息使您可以控制令牌的使用方式。您无需开启新对话来纠正错误或补充信息，也不必因提交后续修正而浪费令牌。

> **注意**：在语言模型的语境中，编辑过往消息的行为常被称为“重写历史”。

需要记住的其他要点：

- 通过点击消息块的角色标识，可以循环切换其类型。当您收到一个`Assistant`格式的回复并希望编辑后以`You`格式重新发送时，这一功能尤为实用。

## 命令概览 {#commands}

斜杠命令可增强助手功能。在行首输入`/`即可查看可用命令列表：

- `/default`：插入默认规则
- `/diagnostics`：注入项目语言服务器报告的错误信息
- `/fetch`：获取网页内容并插入
- `/file`：插入单个文件或整个目录文件
- `/now`：插入当前日期与时间
- `/prompt`：向上下文添加自定义配置提示（[查看规则库](./rules.md#rules-library)）
- `/symbols`：插入当前标签页的活跃符号
- `/tab`：插入当前标签页或所有打开标签页的内容
- `/terminal`：插入终端输出的指定行数内容
- `/selection`：插入已选中的文本内容

> **注意：** 请记住，命令仅在创建文本线程时或插入命令时被评估，因此像 `/now` 这样的命令不会持续更新，`/file` 命令也不会保持其内容的最新状态。

### `/default`

在[规则：编辑默认规则](./rules.md#default-rules)部分中了解更多关于 `/default` 的信息。

用法：`/default`

### `/diagnostics`

`/diagnostics` 命令将项目语言服务器报告的错误注入到上下文中。这对于获取项目中当前问题的概览非常有用。

用法：`/diagnostics [--include-warnings] [path]`

- `--include-warnings`：可选标志，用于在错误之外包含警告。
- `path`：可选路径，用于将诊断限制到特定文件或目录。

### `/file`

`/file` 命令将单个文件或一个目录中的文件内容插入到上下文中。这使您可以在与助手的对话中引用项目的特定部分。

用法：`/file <path>`

您可以使用通配符模式匹配多个文件或目录。

示例：

- `/file src/index.js` - 将`src/index.js`的内容插入上下文。
- `/file src/*.js` - 将`src`目录中所有`.js`文件的内容插入上下文。
- `/file src` - 将`src`目录中所有文件的内容插入上下文。

### `/now`

`/now`命令将当前日期和时间插入上下文。这有助于让语言模型了解当前时间（进而可推知其知识库的新旧程度）。

用法：`/now`

### `/prompt`

`/prompt`命令从提示库中提取提示并插入上下文。也可用于在提示中嵌套其他提示。

用法：`/prompt <prompt_name>`

相关命令：`/default`

### `/symbols`

`/symbols`命令将当前标签页中的活跃符号（函数、类等）插入上下文。适用于快速了解当前文件的结构概览。

用法：`/symbols`

### 插入标签页内容

`/tab` 指令可将当前活动标签页或所有已打开标签页的内容插入上下文，便于您参考正在处理的内容。

使用方法：`/tab [tab_name|all]`

- `tab_name`：可选参数，指定要插入内容的特定标签页名称
- `all`：插入所有已打开标签页的内容

使用示例：

- `/tab` - 插入当前活动标签页的内容
- `/tab "index.js"` - 插入名为 "index.js" 标签页的内容
- `/tab all` - 插入所有已打开标签页的内容

### 插入终端输出

`/terminal` 指令可将终端输出的指定行数内容插入上下文，便于参考最近的命令输出或日志信息。

使用方法：`/terminal [<number>]`

- `<number>`：可选参数，指定要插入的行数（默认为50行）

### 插入选中文本

`/selection` 指令可将编辑器中选中的文本内容插入上下文，便于引用代码的特定部分。

这相当于 `agent: quote selection` 命令（{#kb agent::QuoteSelection}）。

用法：`/selection`

## 规则库中的命令 {#slash-commands-in-rules}

[命令](#commands) 可在规则库（先前称为提示库）的规则中使用，以插入动态内容或执行操作。
例如，若需创建需要模型知晓当前日期的规则，可使用 `/now` 命令插入当前日期。

> **警告：** 规则中的斜杠命令**仅**在文本线程中生效。非文本线程不支持使用此功能。

> **注意：** 规则中的斜杠命令**必须**独立成行。

有关命令详情及可用斜杠命令列表，请参阅上方的[命令列表](#commands)。

### 示例

```plaintext
You are an expert Rust engineer. The user has asked you to review their project and answer some questions.

Here is some information about their project:

/file Cargo.toml
```

在上述示例中，使用 `@file` 命令将 `Cargo.toml` 文件（或项目中所有 `Cargo.toml` 文件）的内容插入到规则中。

## 嵌套规则

与向默认规则添加规则类似，您可以使用 `/prompt` 命令在其他规则中嵌套规则（目前仅支持在文本线程中使用）。

您可能希望嵌套规则以实现以下目的：

- 动态创建模板
- 将文档或引用等集合拆分为更小、可混合搭配的部件
- 创建相似规则的变体（例如 `Async Rust - Tokio` 与 `Async Rust - Async-std`）

### 示例：

```plaintext
Title: Zed-Flavored Rust

## About Zed

/prompt Zed: Zed (a rule about what Zed is)

## Rust - Zed Style

/prompt Rust: Async - Async-std (zed doesn't use tokio)
/prompt Rust: Zed-style Crates (we have some unique conventions)
/prompt Rust - Workspace deps (bias towards reusing deps from the workspace)
```

_上方括号中的文本是注释，不属于规则的一部分。_

> **注意：** 虽然技术上确实_可以_将规则嵌套在自身内部，但我们强烈不建议这样做。使用时请自行承担风险！

通过使用嵌套规则，您可以创建模块化和可复用的规则组件，这些组件可以通过不同方式组合以适应各种场景。

> **注意：** 当使用斜杠命令引入额外上下文时，注入的内容可以直接在文本线程中进行内联编辑——此处的编辑不会传播到已保存的规则中。

## 可扩展性

扩展可以提供额外的斜杠命令。

请参阅[扩展：斜杠命令](../extensions/slash-commands.md)了解如何创建自定义命令。

## 高级概念

### 规则模板 {#rule-templates}

Zed 使用规则模板来驱动内部助手功能，例如终端助手或内联助手中使用的内容规则。

Zed 内置以下规则模板：

- `content_prompt.hbs`：用于在编辑器中生成内容。
- `terminal_assistant_prompt.hbs`：用于终端助手功能。

目前尚不确定是否会进一步扩展模板以支持用户自定义创建。

### 覆盖模板

> **注意：** 除非你清楚自己在做什么，否则不建议覆盖模板。错误地编辑模板会导致助手功能失效。

Zed 允许你通过在 `~/.config/zed/prompt_overrides` 目录中放置自定义的 Handlebars (.hbs) 模板，来覆盖各种助手功能使用的默认规则。

以下模板可以被覆盖：

1. [`content_prompt.hbs`](https://github.com/zed-industries/zed/tree/main/assets/prompts/content_prompt.hbs)：用于在编辑器中生成内容。

2. [`terminal_assistant_prompt.hbs`](https://github.com/zed-industries/zed/tree/main/assets/prompts/terminal_assistant_prompt.hbs)：用于终端助手功能。

> **注意：** 请确认您确实需要覆盖这些模板，否则您将错过我们内置功能的迭代更新。此功能主要适用于开发Zed时使用。

您可以根据需要自定义这些模板，同时保留Zed使用的核心结构和变量。当磁盘上的提示模板发生变化时，Zed会自动重新加载您覆盖的提示内容。

请参考Zed的[assets/prompts](https://github.com/zed-industries/zed/tree/main/assets/prompts)目录查看当前可用的模板版本。

### 历史记录 {#history}

当您在文本对话中提交第一条消息后，语言模型会为您的上下文生成一个名称，并自动将上下文保存至以下路径：

- `~/.config/zed/conversations`（macOS系统）
- `~/.local/share/zed/conversations`（Linux系统）
- `%LocalAppData%\Zed\conversations`（Windows系统）

您可以通过点击智能助手面板左上角的历史记录按钮来访问和加载之前的上下文内容。

![查看助手历史记录](https://zed.dev/img/assistant/assistant-history.png)