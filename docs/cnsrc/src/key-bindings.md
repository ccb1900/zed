# 快捷键绑定

Zed 拥有高度可定制的快捷键系统——你可以随心调整所有设置，让操作体验完全符合你的使用习惯！

## 预设快捷键方案

如果你习惯特定编辑器的默认设置，可以在[配置文件](./configuring-zed.md)中设置`base_keymap`。
目前我们支持：

- VS Code（默认方案）
- Atom
- Emacs（测试版）
- JetBrains
- Sublime Text
- TextMate
- Cursor
- 无（禁用所有快捷键）

该设置也可通过命令面板中的`zed: toggle base keymap selector`操作进行修改。

你还可以启用`vim_mode`或`helix_mode`来添加模态快捷键绑定。
更多信息请参阅 [Vim 模式](./vim.md) 和 [Helix 模式](./helix.md) 的文档。

## 自定义快捷键配置

Zed 查找快捷键配置的路径：

- macOS/Linux：`~/.config/zed/keymap.json`
- Windows：`~\AppData\Roaming\Zed/keymap.json`

您可以通过命令面板中的 {#action zed::OpenKeymapFile} 操作打开快捷键映射文件，或通过 {#action zed::OpenKeymap} 操作或 {#kb zed::OpenKeymap} 快捷键进入 Zed 快捷键编辑器进行编辑。

`keymap.json` 文件包含由 `"bindings"` 组成的 JSON 对象数组。若未设置 `"context"`，快捷键绑定将始终生效；若已设置，则仅当[上下文匹配](#contexts)时生效。

在每个绑定区块中，[按键序列](#keybinding-syntax)会映射到[特定操作](#actions)。若检测到冲突，将按照[下文所述规则](#precedence)进行解析。

若您使用非 QWERTY 布局的拉丁字符键盘，建议将 `use_key_equivalents` 设置为 `true`。更多信息请参阅[非 QWERTY 键盘](#non-qwerty-keyboards)章节。

例如：

你可以在以下默认键位设置中查看 Zed 的所有默认快捷键绑定：

- [macOS](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-macos.json)
- [Windows](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-windows.json)
- [Linux](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-linux.json)

如需调试自定义键位设置问题，可通过命令面板使用 `keymap: 输入键位` 功能。若遇到预期可用但实际失效的情况，请提交 [问题反馈](https://github.com/zed-industries/zed)。

### 快捷键语法说明

Zed 能够匹配的不仅是单个按键，还可以是按顺序输入的一系列按键。`"bindings"` 映射中的每个键都是一组由空格分隔的按键序列。

每个按键由修饰符和键名组成。修饰符包括：

- `ctrl-` 表示 Control 键
- `cmd-`、`win-` 或 `super-` 表示平台修饰键（macOS 上是 Command，Windows 上是 Windows 键，Linux 上是 Super 键）
- `alt-` 表示 Alt 键（macOS 上是 Option）
- `shift-` 表示 Shift 键
- `fn-` 表示功能键
- `secondary-` 在 macOS 上等同于 `cmd`，在 Windows 和 Linux 上等同于 `ctrl`

键名可以是键盘生成的任意单个 Unicode 码位（例如 `a`、`0`、`£` 或 `ç`），也可以是任意命名键（例如 `tab`、`f1`、`shift` 或 `cmd`）。如果使用非拉丁键盘布局（如西里尔字母），可以绑定到西里尔字符，也可以绑定到按下 `cmd` 时该键生成的拉丁字符。

以下是一些示例：

```json [settings]
 "bindings": {
   "cmd-k cmd-s": "zed::OpenKeymap", // matches ⌘-k then ⌘-s
   "space e": "editor::Complete", // type space then e
   "ç": "editor::Complete", // matches ⌥-c
   "shift shift": "file_finder::Toggle", // matches pressing and releasing shift twice
 }
```

`shift-` 修饰键仅能与字母键组合使用，表示对应的大写形式。例如，`shift-g` 对应键入 `G` 的组合操作。虽然在多数键盘上会通过 shift 键输入标点符号（如 `(`），但这类击键操作不被视为修饰键组合，因此 `shift-(` 无法实现匹配。

`alt-` 修饰键可在多种键盘布局中生成特殊字符。例如在 macOS 美式键盘上，组合键 `alt-c` 会输出 `ç`。您可以在键位配置文件中匹配这两种形式，不过按照惯例，Zed 会将此组合统一标注为 `alt-c`。

我们可以实现单独按下修饰键的匹配。例如，`shift shift` 可用于实现 JetBrains 的“全局搜索”快捷键。这种情况下，绑定会在按键释放时触发，而非按键按下时。

### 上下文匹配

若绑定组包含 `"context"` 键，系统会将其与 Zed 当前活跃的上下文进行匹配。

Zed 的上下文构成树状结构，根节点为 `Workspace`。工作区包含窗格与面板，窗格包含编辑器等。要查看当前活跃的上下文，最简便的方式是通过命令面板的 `dev: open key context view` 命令打开上下文视图。

示例：

```
# in an editor, it might look like this:
Workspace os=macos keyboard_layout=com.apple.keylayout.QWERTY
  Pane
    Editor mode=full extension=md vim_mode=insert

# in the project panel
Workspace os=macos
  Dock
    ProjectPanel not_editing
```

上下文表达式支持以下语法结构：

- `X && Y`、`X || Y` 用于连接两个条件（与/或关系）
- `!X` 用于检查条件为假
- `(X)` 用于条件分组
- `X > Y` 用于匹配当层级树中的祖先节点满足X条件且当前层级满足Y条件时

例如：

- `"context": "Editor"` - 匹配任何编辑器（包括内联输入框）
- `"context": "Editor && mode=full"` - 匹配用于编辑代码的主编辑器
- `"context": "!Editor && !Terminal"` - 匹配除编辑器或终端获得焦点之外的所有区域
- `"context": "os=macos > Editor"` - 在macOS系统上匹配任何编辑器

值得注意的是，属性仅在其定义的节点上可用。这意味着如果您需要（例如）仅在调试器于vim正常模式中暂停时启用某个键绑定，就需要使用 `debugger_stopped > vim_mode == normal` 这样的写法。

> 注意：在 Zed v0.197.x 版本之前，`!` 运算符每次仅检查一个节点，且 `>` 表示“父节点”而非“祖先节点”。这意味着 `!Editor` 会匹配上下文 `Workspace > Pane > Editor`，因为（令人困惑的是）面板匹配 `!Editor`，而 `os=macos > Editor` 由于存在中间 `Pane` 节点，无法匹配上下文 `Workspace > Pane > Editor`。

若您使用 Vim 模式，我们提供了关于 [Vim 模式如何影响上下文](./vim.md#contexts) 的说明。Helix 模式基于 Vim 模式构建，并使用相同的上下文机制。

### 操作指令

Zed 的几乎所有功能都以操作形式开放。  
虽然没有明确列出的文档，但你可以通过以下方式找到大部分操作：在命令面板中搜索、查看默认键位配置（[macOS](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-macos.json)、[Windows](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-windows.json) 或 [Linux](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-linux.json)），或在键位配置文件中使用 Zed 的自动补全功能。

大多数操作不需要任何参数，因此可以将它们绑定为字符串：`"ctrl-a": "language_selector::Toggle"`。有些操作需要单个参数，必须绑定为数组：`"cmd-1": ["workspace::ActivatePane", 0]`。还有一些操作需要多个参数，需绑定为包含字符串和对象的数组：`"ctrl-a": ["pane::DeploySearch", { "replace_enabled": true }]`。

### 优先级

当多个按键绑定拥有相同组合键且同时处于活动状态时，优先级通过两种方式确定：

- 在上下文树中匹配较低层级节点的绑定优先。这意味着若某个绑定的上下文为`Editor`，其优先级将高于上下文为`Workspace`的绑定。无上下文匹配的绑定位于树结构最底层。
- 若存在多个绑定在同一树层级匹配，则后定义的绑定优先。由于用户按键绑定在系统按键绑定之后加载，这使得用户绑定可覆盖内置绑定。

另一种冲突情形是当两个绑定存在前缀包含关系时。例如同时存在`"ctrl-w":"editor::DeleteToNextWordEnd"`与`"ctrl-w left":"editor::DeleteToEndOfLine"`的情况。

当这种情况发生，且当前上下文中两个绑定都处于激活状态时，Zed会在您输入`ctrl-w`后等待1秒，以判断您是否准备继续输入`left`。若期间未输入任何内容或输入其他按键，则触发`DeleteToNextWordEnd`；若输入指定按键，则触发`DeleteToEndOfLine`。

### 非QWERTY键盘支持

Zed对非QWERTY键盘的支持仍在持续完善中。

若您的键盘支持完整ASCII字符集（如DVORAK、COLEMAK等布局），快捷键将按预期正常工作。

若不符合上述条件，请继续阅读...

#### macOS系统

对于西里尔字母、希伯来语、亚美尼亚语等主要使用非ASCII字符的键盘布局，当按住`cmd`时，macOS会自动将按键映射至ASCII范围。Zed在此基础上进一步优化：无论修饰键状态或`use_key_equivalents`设置如何，系统始终能根据ASCII布局或实际布局匹配按键。例如在泰语键盘布局中，按下`ctrl-ๆ`既可匹配`ctrl-q`的绑定，也可匹配`ctrl-ๆ`的绑定。

在支持扩展拉丁字母的键盘（法语AZERTY、德语QWERTZ等）上，若不借助`option`通常无法输入完整的ASCII字符集。这会引发一个歧义问题：`option-2`会生成`@`。为确保所有内置键盘快捷键仍可在这些键盘上使用，我们会重新分配按键绑定。例如，在QWERTY布局中绑定到`@`的快捷键，在西班牙语布局中会被转移到`"`。该映射基于macOS系统默认设置，可通过命令面板运行`dev: open key context view`查看。

若需在个人键位映射中自定义快捷键，可通过在键位映射文件中将`use_key_equivalents`设为`true`来启用按键等效映射：

```json [keymap]
[
  {
    "use_key_equivalents": true,
    "bindings": {
      "ctrl->": "editor::Indent" // parsed as ctrl-: when a German QWERTZ keyboard is active
    }
  }
]
```

### Linux系统

自 v0.196.0 起，在 Linux 系统中，若您输入的按键无法生成 ASCII 字符，我们会采用 QWERTY 布局的等效按键作为键盘快捷键。这意味着许多快捷键可以在多种键盘布局下使用。

目前我们尚未对所有内置快捷键进行位置调整以确保它们能在每种布局下正常使用。因此，如果某些 ASCII 字符无法通过当前布局输入，而您的键盘布局在对应按键上设置了不同的 ASCII 字符，您可能需要添加自定义键位绑定来实现功能。我们计划在后续版本中解决这个问题，也非常欢迎您提供帮助！

## 使用技巧

### 禁用绑定配置

若希望特定情境下某个绑定不执行任何操作，可使用 `null` 作为操作指令。这在以下场景中非常实用：
- 意外触发某快捷键时需要禁用该功能
- 需要输入该按键序列原本对应的字符
- 需要禁用以该按键开头的多键组合绑定

```json [keymap]
[
  {
    "context": "Workspace",
    "bindings": {
      "cmd-r": null // cmd-r will do nothing when the Workspace context is active
    }
  }
]
```

一个`null`绑定遵循与常规操作相同的优先级规则，因此它会禁用树状结构中所有更高层级的匹配绑定。若希望树状结构中更高层级的绑定优先于较低层级的绑定，您需要在特定上下文中重新绑定所需操作。

这种机制能有效防止当指定操作具有条件性且可传播时，Zed回退到默认键位绑定。例如，`buffer_search::DeployReplace`仅在搜索栏不可见时触发。若搜索栏处于可见状态，该操作会向上传播并触发为该键位设置的默认操作（例如打开右侧停靠栏）。为避免这种情况：

### 重新映射按键

一个常见的需求是将单个按键映射为一系列按键操作。您可以通过 `workspace::SendKeystrokes` 动作实现这一功能。

然而，这种方法也存在一些局限性，主要包括：

- 所有异步操作都将在所有按键绑定分发完毕后才会执行。例如，这意味着虽然您可以通过绑定打开文件（如 `cmd-alt-r` 示例所示），但无法继续发送后续按键并期望它们在新视图中被解析。  
- 其他异步操作示例包括：打开命令面板、与语言服务器通信、更改缓冲区语言设置，以及任何涉及网络请求的操作。  
- 单次操作最多支持模拟 100 个按键。  

`SendKeystrokes` 的参数是以空格分隔的按键序列（使用与前述相同的语法）。由于按键解析机制的限制，任何未被识别为按键输入的片段都会被逐字发送至当前聚焦的输入框。  

若 `SendKeystrokes` 的参数包含触发该操作的绑定组合，系统将自动采用次高优先级的绑定定义。这一机制允许您在保留默认按键功能的基础上进行扩展。

### 向终端转发按键

若您使用 Linux 或 Windows 系统，可能会希望将特定快捷键转发至内置终端，而非由 Zed 编辑器处理。

例如，在 Linux 系统中，`ctrl-n` 用于在 Zed 创建新标签页。若希望当内置终端处于焦点状态时，将 `ctrl-n` 发送至终端，请在快捷键映射中添加以下配置：

```json [settings]
{
  "context": "Terminal",
  "bindings": {
    "ctrl-n": ["terminal::SendKeystroke", "ctrl-n"]
  }
}
```

### 任务快捷键绑定

您还可以为定义在 `tasks.json` 中的 Zed 任务设置快捷键绑定。
具体说明请参阅[任务文档](tasks.md#custom-keybindings-for-tasks)。