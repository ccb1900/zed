# Vim 模式

Zed 内置了一个名为“vim 模式”的 Vim 模拟层。本页面将指导您如何启用或禁用 Zed 的 vim 模式，了解 Zed 提供了哪些工具和命令来帮助您浏览和编辑代码，以及如何充分发挥 vim 模式的功能。

您将学习如何：

- 理解 Zed 的 vim 模式与传统 Vim 的核心差异
- 启用或禁用 vim 模式
- 在 vim 模式下充分利用 Zed 的专属功能
- 自定义 vim 模式快捷键绑定
- 配置 vim 模式设置

无论您是 vim 模式的新用户，还是希望优化 Zed 使用体验的资深 Vim 用户，本指南都将帮助您充分发挥 Zed 中模态编辑的强大功能。

## Zed 的 vim 模式设计理念

Vim 模式致力于为 Vim 用户提供熟悉的操作体验：在适用场景下精准复现移动指令和命令行为，同时结合 Zed 的专属功能，打造开箱即用、无需额外配置的编辑体验。

这包括支持语义导航、多光标，或是通常由插件提供的环绕文本等功能。

因此，Zed的vim模式并非与Vim一一对应，而是将Vim的模态设计与Zed的现代功能相融合，以提供更流畅的体验。它也是可配置的，你可以添加自定义按键绑定或覆盖默认设置。

### 核心差异

vim模式中有四类功能采用了Zed的核心能力，因此行为上存在一些差异：

1. **移动操作**：Vim 模式利用 Zed 的语义解析功能，针对不同语言优化移动操作的行为。例如在 Rust 中，使用 `%` 跳转到匹配括号时，该操作可兼容管道符 `|`；而在 JavaScript 中，`w` 会将 `$` 识别为单词字符。

2. **可视化块选择**：Vim 模式通过 Zed 的多光标功能模拟可视化块选择，使块选择操作更加灵活。例如在块选择后插入内容时，所有选定行都会实时更新，且支持随时增删光标。

3. **宏功能**：Vim 模式依托 Zed 的录制系统实现 Vim 宏。这意味着您可以录制并回放更复杂的操作，例如自动补全。

4. **搜索替换**：Vim 模式采用 Zed 的搜索系统，因此其正则表达式语法与 Vim 略有差异。具体说明请参阅[正则表达式差异章节](#regex-differences)。

> **注意：** Zed 的 Vim 模式基础功能已覆盖多数使用场景，我们也在持续优化改进。如果您发现工作流中依赖的功能缺失，请[在 GitHub 上提交问题](https://github.com/zed-industries/zed/issues)。

## 启用与禁用 Vim 模式

首次打开 Zed 时，您会在欢迎界面看到启用 Vim 模式的复选框。

若初始未设置，您随时可通过命令面板调用工作区命令 `toggle vim mode` 来切换 Vim 模式开关。

> **注意**：该命令会切换您用户设置中的以下属性：
>
> ```json [settings]
> {
>   "vim_mode": true
> }
> ```

## Zed 特色功能

Zed 基于现代化架构构建，不仅集成 Tree-sitter 和语言服务器来解析编辑文件的内容，更原生支持多光标操作。

Vim 模式包含多个“核心 Zed”快捷键绑定，可帮助您充分利用 Zed 的特定功能集。

### 语言服务器

以下命令借助语言服务器功能，助您更高效地浏览和重构代码。

| 命令                                  | 默认快捷键 |
| ---------------------------------------- | ---------------- |
| 转到定义                         | `g d`            |
| 转到声明                        | `g D`            |
| 转到类型定义                    | `g y`            |
| 转到实现                     | `g I`            |
| 重命名（更改定义）               | `c d`            |
| 转到当前词的所有引用 | `g A`            |
| 在当前文件中查找符号              | `g s`            |
| 在整个项目中查找符号            | `g S`            |
| 转到下一个诊断                    | `g ]` 或 `] d`   |
| 转到上一个诊断                | `g [` 或 `[ d`   |
| 显示内联错误（悬停）                | `g h`            |
| 打开代码操作菜单               | `g .`            |

### Git

| 命令                             | 默认快捷键       |
| ------------------------------- | ---------------- |
| 跳转到下一个 git 变更           | `] c`            |
| 跳转到上一个 git 变更           | `[ c`            |
| 展开差异区块                    | `d o`            |
| 切换暂存状态                    | `d O`            |
| 暂存并跳转下一项（差异视图）    | `d u`            |
| 取消暂存并跳转下一项（差异视图）| `d U`            |
| 恢复变更                        | `d p`            |

### Tree-sitter 语法解析器

Tree-sitter 是 Zed 用来理解代码结构的强大工具。Zed 提供了用于改变当前光标位置的光标移动命令，以及可作为操作目标的文本对象选择功能。

| 命令                             | 默认快捷键                  |
| ------------------------------- | --------------------------- |
| 跳转到下一个/上一个方法         | `] m` / `[ m`               |
| 跳转到下一个/上一个方法结尾     | `] M` / `[ M`               |
| 跳转到下一个/上一个章节         | `] ]` / `[ [`               |
| 跳转到下一个/上一个章节结尾     | `] [` / `[ ]`               |
| 跳转到下一个/上一个注释         | `] /`, `] *` / `[ /`, `[ *` |
| 选择更大的语法节点              | `[ x`                       |
| 选择更小的语法节点              | `] x`                       |

| 文本对象                                               | 默认快捷键 |
| ------------------------------------------------------ | ---------- |
| 类、定义等的外部范围                                   | `a c`            |
| 类、定义等的内部范围                                   | `i c`            |
| 函数、方法等的外部范围                                 | `a f`            |
| 函数、方法等的内部范围                                 | `i f`            |
| 注释块                                                 | `g c`            |
| 参数或列表项等                                         | `i a`            |
| 参数或列表项等（包含尾部逗号）                         | `a a`            |
| 类HTML标签的外部范围                                   | `a t`            |
| 类HTML标签的内部范围                                   | `i t`            |
| 当前缩进级别及前后各一行                               | `a I`            |
| 当前缩进级别及前一行                                   | `a i`            |
| 当前缩进级别                                           | `i i`            |

需要注意的是，`[m` 系列移动操作的目标定义与 `af` 定义的边界相同。`[[` 的目标与 `ac` 定义的目标一致，不过当不存在类时，函数也会被纳入目标范围。类似地，`gc` 用于查找 `[ /`。`g c`

函数、类和注释的定义与具体编程语言相关，扩展功能可以通过添加 [`textobjects.scm`] 来支持。参数和标签的定义在 Tree-sitter 语法解析层级实现，其通过识别语法树中的特定模式进行工作，目前暂不支持按语言进行单独配置。

### 多光标模式

这些命令可帮助您在 Zed 编辑器中管理多个光标。

| 命令                                                         | 默认快捷键       |
| ------------------------------------------------------------ | ---------------- |
| 在当前单词的下一个匹配项处添加光标                           | `g l`            |
| 在当前单词的上一个匹配项处添加光标                           | `g L`            |
| 跳过最近选中的单词，并选中下一个匹配项                       | `g >`            |
| 跳过最近选中的单词，并选中上一个匹配项                       | `g <`            |
| 为当前单词的所有匹配项添加可视化选区                         | `g a`            |

### 窗格管理

以下命令用于打开新窗格或跳转到指定窗格。

| 命令                                       | 默认快捷键         |
| ------------------------------------------ | ------------------ |
| 打开项目范围搜索                           | `g /`              |
| 打开当前搜索片段                           | `g <space>`        |
| 在分屏中打开当前搜索片段                   | `<ctrl-w> <space>` |
| 在分屏中跳转到定义                         | `<ctrl-w> g d`     |
| 在分屏中跳转到类型定义                     | `<ctrl-w> g D`     |

### 在插入模式下

以下命令可帮助您调出Zed的补全菜单、向GitHub Copilot请求建议，或在不离开插入模式的情况下打开内联AI助手。

| 命令                                                                         | 默认快捷键       |
| ---------------------------------------------------------------------------- | ---------------- |
| 打开补全菜单                                                                 | `ctrl-x ctrl-o`  |
| 请求 GitHub Copilot 建议（需配置 GitHub Copilot）                            | `ctrl-x ctrl-c`  |
| 打开内联 AI 助手（需配置助手）                                              | `ctrl-x ctrl-a`  |
| 打开代码操作菜单                                                             | `ctrl-x ctrl-l`  |
| 隐藏所有建议                                                                 | `ctrl-x ctrl-z`  |

### 支持的插件

Zed 的 vim 模式包含了一些在 Vim 生态系统中通常由非常流行的插件提供的功能：

- 你可以使用 `ys`（环绕复制）来环绕文本对象，使用 `cs` 更改环绕内容，使用 `ds` 删除环绕内容。  
- 在可视模式下使用 `gc` 可以注释或取消注释选中内容，在普通模式下使用 `gcc` 实现相同功能。  
- 项目面板支持许多仿照 Vim 插件 `netrw` 的快捷键：使用 `hjkl` 进行导航，使用 `o` 打开文件，使用 `t` 在新标签页中打开文件等。  
- 你可以将按键绑定添加到键位映射中，以便在 "camelCase" 命名中导航。前往[可选按键绑定](#optional-key-bindings)部分了解具体方法。  
- 你可以使用 `gR` 实现 [ReplaceWithRegister](https://github.com/vim-scripts/ReplaceWithRegister) 功能。  
- 你可以使用 `cx` 实现 [vim-exchange](https://github.com/tommcdo/vim-exchange) 功能。注意，该功能在可视模式下没有默认绑定，但你可以通过键位映射自行添加（参考[可选按键绑定](#optional-key-bindings)部分）。  
- 你可以通过 [indent wise](https://github.com/jeetsukumaran/vim-indentwise) 插件 `[-`、`]-`、`[+`、`]+`、`[=`、`]=` 相对于光标位置导航至不同缩进层级。  
- 你可以使用 AnyQuotes 选择引号内的文本，使用 AnyBrackets 选择括号内的文本。Zed 还提供了 MiniQuotes 和 MiniBrackets，它们基于 [mini.ai](https://github.com/echasnovski/mini.nvim/blob/main/readmes/mini-ai.md) Neovim 插件提供不同的选择行为。详见下方的[引号和括号文本对象](#quote-and-bracket-text-objects)部分。  
- 你可以配置 AnyQuotes、AnyBrackets、MiniQuotes 和 MiniBrackets 文本对象，使用不同的选择策略来选择引号和括号内的文本。详见下方的[任意括号功能](#any-bracket-functionality)部分。

### 任意括号功能

Zed 提供两种不同的策略来选择被任意引号或任意括号包围的文本。这些文本对象**默认未启用**，必须在按键映射中进行配置才能使用。

#### 包含字符

每种文本对象类型对应特定字符：

| 文本对象              | 字符                                                                             |
| --------------------- | -------------------------------------------------------------------------------- |
| 任意引号/迷你引号     | 单引号 (`'`)、双引号 (`"`)、反引号 (`` ` ``)                             |
| AnyBrackets/MiniBrackets | Parentheses (`()`), Square brackets (`[]`), Curly braces (`{}`), Angle brackets (`<>`) |

"任意"和"迷你"变体都支持相同的字符集，但选择策略有所不同。

#### 任意引号与任意括号（传统 Vim 行为）

这些文本对象实现了传统 Vim 的行为：

- **选择优先级**：首先查找最内层（最近）的引号或括号
- **回退机制**：若未找到，则回退至当前行
- **基于字符的匹配**：仅关注开合字符，不考虑语法
- **原生Vim相似性**：AnyBrackets的行为与原生Vim中的`ci<`、`ci(`等命令保持一致，包括潜在边缘情况（例如将`=>`中的`>`视为闭合分隔符）

#### MiniQuotes与MiniBrackets（mini.ai行为）

这些文本对象实现了[mini.ai](https://github.com/echasnovski/mini.nvim/blob/main/readmes/mini-ai.md) Neovim插件的行为：

- **选择优先级**：先搜索当前行，再向外扩展
- **Tree-sitter集成**：使用Tree-sitter查询实现更智能的上下文感知选择
- **语法感知匹配**：能区分实际括号与其他语境中的相似字符（例如`=>`中的`>`）

#### 方案选择指南

- 选择 **AnyQuotes/AnyBrackets** 的情况：

  - 偏好传统 Vim 操作方式
  - 需要基于字符的选区，优先选择最内层分隔符
  - 需要与原生 Vim 文本对象高度匹配的行为

- 选择 **MiniQuotes/MiniBrackets** 的情况：
  - 偏好 mini.ai 插件的操作逻辑
  - 需要基于 Tree-sitter 的上下文感知选区功能
  - 在搜索时优先考虑当前行内容

#### 配置示例

要使用这些文本对象，您需要在键位映射中添加绑定。以下示例配置展示了如何在文本对象操作符（`i` 和 `a`）或变更环绕符（`cs`）时启用这些功能：

```json [settings]
{
  "context": "vim_operator == a || vim_operator == i || vim_operator == cs",
  "bindings": {
    // Traditional Vim behavior
    "q": "vim::AnyQuotes",
    "b": "vim::AnyBrackets",

    // mini.ai plugin behavior
    "Q": "vim::MiniQuotes",
    "B": "vim::MiniBrackets"
  }
}
```

完成配置后，您就可以使用如下命令：

- `cib` - 使用AnyBrackets模式修改括号内内容
- `ciB` - 使用MiniBrackets模式修改括号内内容  
- `ciq` - 使用AnyQuotes模式修改引号内内容
- `ciQ` - 使用MiniQuotes模式修改引号内内容

## 命令面板

Vim模式允许您通过`:`打开Zed的命令面板。随后您可以通过输入来访问所有常规Zed命令。此外，vim模式还为常用Vim命令设置了别名，确保您的肌肉记忆能无缝迁移到Zed。例如，您可以通过输入`:w`或`:write`来保存文件。

下方表格列出了可在命令面板中使用的指令。我们将可选字符置于方括号内，表示这些字符可以省略。

> **注意**：我们尚未完全复现Vim命令行的全部功能。特别需要注意的是，当前命令暂不支持参数配置。如果您发现命令面板中缺少某些功能，请[在GitHub上提交问题](https://github.com/zed-industries/zed)。

### 文件与窗口管理

该表格展示了用于管理窗口、标签页和窗格的命令。由于当前命令不支持参数，因此在保存或创建新文件时无法指定文件名。

| 命令             | 描述                                             |
| ---------------- | ------------------------------------------------ |
| `:w[rite][!]` | 保存当前文件                                     |
| `:wq[!]` | 保存文件并关闭缓冲区                             |
| `:q[uit][!]` | 关闭缓冲区                                       |
| `:wa[ll][!]` | 保存所有已打开的文件                             |
| `:wqa[ll][!]` | 保存所有已打开文件并关闭所有缓冲区               |
| `:qa[ll][!]` | 关闭所有缓冲区                                   |
| `:[e]x[it][!]` | 关闭缓冲区                                       |
| `:up[date]` | 保存当前文件                                     |
| `:cq` | 完全退出（关闭所有正在运行的Zed实例）           |
| `:vs[plit]` | 垂直分割窗格                                     |
| `:sp[lit]` | 水平分割窗格                                    |
| `:new` | 在水平分割中创建新文件                          |
| `:vne[w]` | 在垂直分割中创建新文件                          |
| `:tabedit` | 在新标签页中创建新文件                          |
| `:tabnew` | 在新标签页中创建新文件                          |
| `:tabn[ext]` | 切换到下一个标签页                              |
| `:tabp[rev]` | 切换到上一个标签页                              |
| `:tabc[lose]` | 关闭当前标签页                                  |
| `:ls` | 显示所有缓冲区                                  |

> **注意：** `!` 字符用于强制命令在不保存更改或覆盖文件前不提示的情况下执行。

### 扩展命令

以下扩展命令用于打开 Zed 的各类面板和窗口。

| 命令                         | 默认快捷键       |
| ---------------------------- | ---------------- |
| 打开项目面板                 | `:E[xplore]`     |
| 打开协作面板                 | `:C[ollab]`      |
| 打开聊天面板                 | `:Ch[at]`        |
| 打开 AI 面板                 | `:A[I]`          |
| 打开 Git 面板                | `:G[it]`         |
| 打开调试面板                 | `:D[ebug]`       |
| 打开通知面板                 | `:No[tif]`       |
| 打开反馈窗口                 | `:fe[edback]`    |
| 打开诊断窗口                 | `:cl[ist]`       |
| 打开终端                     | `:te[rm]`        |
| 打开扩展窗口                 | `:Ext[ensions]`  |

### 诊断导航

以下命令用于在诊断信息间导航。

| 命令                      | 描述                         |
| ------------------------ | ------------------------------ |
| `:cn[ext]` 或 `:ln[ext]` | 跳转到下一个诊断问题           |
| `:cp[rev]` 或 `:lp[rev]` | 跳转到上一个诊断问题           |
| `:cc` 或 `:ll`           | 打开错误页面                   |

### Git 版本控制

这些命令与版本控制系统 git 进行交互。

| 命令         | 描述                                             |
| --------------- | ------------------------------------------------------- |
| `:dif[fupdate]` | 查看光标所在位置的差异（普通模式下使用 `d o`）   |
| `:rev[ert]`     | 还原光标所在位置的差异（普通模式下使用 `d p`） |

### 跳转

这些命令可跳转到文件中的特定位置。

| 命令                | 描述                     |
| ------------------- | ------------------------ |
| `:<number>`         | 跳转到指定行号           |
| `:$`                | 跳转到文件末尾           |
| `:/foo` 与 `:?foo` | 跳转到下一个/上一个匹配 foo 的行 |

### 替换操作

此命令用于替换文本，它模拟了 vim 中的替换命令。替换命令使用正则表达式，而 Zed 采用的语法与 vim 略有不同。您可以在下方的[正则表达式差异章节](#regex-differences)中深入了解 Zed 的语法规则。Zed 默认仅替换当前行中首个匹配的搜索模式，若需替换全部匹配项，请在命令后添加 `g` 标志。

| 命令                  | 描述                     |
| --------------------- | ------------------------ |
| `:[range]s/foo/bar/[g]` | 将所有的 foo 替换为 bar |

### 编辑操作

以下命令可辅助进行文本编辑。

| 命令           | 描述                                             |
| ----------------- | ------------------------------------------------------- |
| `:j[oin]`         | 合并当前行                                   |
| `:d[elete][l][p]` | 删除当前行                                 |
| `:s[ort] [i]`     | 对当前选中内容进行排序（使用i参数，不区分大小写） |
| `:y[ank]`         | 复制当前选中内容或整行               |

### 设置

这些命令会为当前缓冲区局部修改编辑器选项。

| 命令                         | 描述                                                                                   |
| ------------------------------- | --------------------------------------------------------------------------------------------- |
| `:se[t] [no]wrap`               | 超过窗口宽度的行将自动换行并在下一行继续显示 |
| `:se[t] [no]nu[mber]`           | 在每行前方显示行号                                                   |
| `:se[t] [no]r[elative]nu[mber]` | 将显示的行号改为相对于光标的相对行号                                     |
| `:se[t] [no]i[gnore]c[ase]`     | 控制缓冲区和项目搜索是否启用区分大小写的匹配                    |

### 命令助记符

由于所有Zed命令均可调用，您可能会发现记住运行正确命令的助记符很有帮助。例如：

- `:diffs` 对应"切换所有代码块差异"
- `:cpp` 对应"复制文件路径"
- `:crp` 对应"复制相对路径"
- `:reveal` 对应"在访达中显示"
- `:zlog` 对应"打开 Zed 日志"
- `:clank` 对应"取消语言服务器工作"

## 自定义按键绑定

在本节中，我们将学习如何自定义 Zed 编辑器的 Vim 模式按键绑定。您将了解：

- 如何为新按键绑定选择正确的上下文
- Vim 模式按键绑定的实用上下文
- 提升效率的常用自定义按键绑定

### 选择正确的上下文

Zed 的按键绑定仅在 `"context"` 属性与您在编辑器中的位置匹配时才会生效。例如，如果将按键绑定添加到 `"Editor"` 上下文，这些绑定仅在编辑文件时生效。如果将按键绑定添加到 `"Workspace"` 上下文，这些绑定将在 Zed 的所有位置生效。以下是一个在编辑文件时保存的按键绑定示例：

```json [settings]
{
  "context": "Editor",
  "bindings": {
    "ctrl-s": "file::Save"
  }
}
```

上下文是嵌套的，因此当您编辑文件时，当前处于`"Editor"`上下文，该上下文包含在`"Pane"`上下文内，而后者又包含在`"Workspace"`上下文内。这就是为什么您添加到`"Workspace"`上下文的任何键位绑定在编辑文件时都会生效。以下是一个示例：

```json [keymap]
// This key binding will work when you're editing a file. It comes built into Zed by default as the workspace: save command.
{
  "context": "Workspace",
  "bindings": {
    "ctrl-s": "workspace::Save"
  }
}
```

上下文本质上是表达式。它们支持布尔运算符，例如`&&`（逻辑与）和`||`（逻辑或）。举例来说，您可以使用`"Editor && vim_mode == normal"`上下文来创建仅在编辑文件且处于Vim正常模式时生效的键位绑定。

Vim模式会向`"Editor"`上下文添加若干子上下文：

| 操作符               | 描述                                                                                                                                                                 |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| VimControl           | 表示应启用 Vim 键位绑定。当前为 `vim_mode == normal \|\| vim_mode == visual \|\| vim_mode == operator` 的别名，但定义可能随时间变化                                                                      |
| vim_mode == normal   | 普通模式                                                                                                                                                             |
| vim_mode == visual   | 可视模式                                                                                                                                                             |
| vim_mode == insert   | 插入模式                                                                                                                                                             |
| vim_mode == replace  | 替换模式                                                                                                                                                            |
| vim_mode == waiting  | 等待任意按键（例如输入 `f` 或 `t` 后）                                                                                             |
| vim_mode == operator | 等待触发其他绑定（例如输入 `c` 或 `d` 后）                                                                                         |
| vim_operator         | 默认设为 `none`，除非 `vim_mode == operator` 成立，此时将设为当前操作符的默认键位绑定（例如输入 `d` 后，`vim_operator == d`） |

> **注意**：上下文每次仅匹配一个层级。因此可以使用表达式 `"Editor && vim_mode == normal"`，但 `"Workspace && vim_mode == normal"` 永远不会匹配，因为我们将 vim 上下文设置在 `"Editor"` 层级。

### Vim 模式按键绑定的实用上下文

以下模板包含实用的 vim 模式上下文，可帮助您自定义 vim 模式按键绑定。您可以复制该模板并将其集成到您的用户键位映射中。

```json [keymap]
[
  {
    "context": "VimControl && !menu",
    "bindings": {
      // Put key bindings here if you want them to work in normal & visual mode.
    }
  },
  {
    "context": "vim_mode == normal && !menu",
    "bindings": {
      // "shift-y": ["workspace::SendKeystrokes", "y $"] // Use neovim's yank behavior: yank to end of line.
    }
  },
  {
    "context": "vim_mode == insert",
    "bindings": {
      // "j k": "vim::NormalBefore" // In insert mode, make jk escape to normal mode.
    }
  },
  {
    "context": "EmptyPane || SharedScreen",
    "bindings": {
      // Put key bindings here (in addition to the context above) if you want them to
      // work when no editor exists.
      // "space f": "file_finder::Toggle"
    }
  }
]
```

> **注意**：如果您希望模拟 Vim 的 `map` 命令（`nmap` 等），可以在正确的上下文中使用 `workspace::SendKeystrokes` 操作。

### 可选按键绑定

默认情况下，你可以使用快捷键在编辑器中打开的不同文件之间导航，例如按下`ctrl+w`后，再按`hjkl`可分别向左、下、上或右移动。

但相同的快捷键无法在所有编辑器面板（终端、项目面板、助手面板等）之间切换。若想使用相同快捷键导航至这些面板，可将以下按键绑定添加到用户键位配置中。

```json [settings]
{
  "context": "Dock",
  "bindings": {
    "ctrl-w h": "workspace::ActivatePaneLeft",
    "ctrl-w l": "workspace::ActivatePaneRight",
    "ctrl-w k": "workspace::ActivatePaneUp",
    "ctrl-w j": "workspace::ActivatePaneDown"
    // ... or other keybindings
  }
}
```

子词移动功能（允许你在驼峰命名或蛇形命名单词中进行逐个单词导航和选择）默认未启用。要启用此功能，请将这些绑定添加到你的键位配置中。

```json [settings]
{
  "context": "VimControl && !menu && vim_mode != operator",
  "bindings": {
    "w": "vim::NextSubwordStart",
    "b": "vim::PreviousSubwordStart",
    "e": "vim::NextSubwordEnd",
    "g e": "vim::PreviousSubwordEnd"
  }
}
```

Vim模式在普通模式下提供了包围选区的快捷键（`ys`），但在可视模式下没有添加包围结构的快捷方式。默认情况下，`shift-s`会替换选区内容（删除文本并进入插入模式）。若要在可视模式下使用`shift-s`添加包围结构，可将以下对象添加到键位映射中：

```json [settings]
{
  "context": "vim_mode == visual",
  "bindings": {
    "shift-s": "vim::PushAddSurrounds"
  }
}
```

在非模态文本编辑器中，光标导航通常在越过行尾时会自动换行。但Zed默认采用与Vim完全相同的处理方式：光标会在行边界处停止。如果您希望光标在行间自动换行，请重写以下键位绑定：

```json [settings]
// In VimScript, this would look like this:
// set whichwrap+=<,>,[,],h,l
{
  "context": "VimControl && !menu",
  "bindings": {
    "left": "vim::WrappingLeft",
    "right": "vim::WrappingRight",
    "h": "vim::WrappingLeft",
    "l": "vim::WrappingRight"
  }
}
```

[Sneak 快速跳转功能](https://github.com/justinmk/vim-sneak) 允许您在文本中快速导航至任意两个连续字符的位置。您可以通过在按键映射中添加以下键位绑定来启用此功能。默认情况下，`s` 键被映射为 `vim::Substitute`。添加这些绑定将覆盖默认行为，请确保此项调整符合您的工作流程习惯。

```json [settings]
{
  "context": "vim_mode == normal || vim_mode == visual",
  "bindings": {
    "s": "vim::PushSneak",
    "shift-s": "vim::PushSneakBackward"
  }
}
```

[vim-exchange](https://github.com/tommcdo/vim-exchange) 功能在可视模式下没有默认按键绑定，因为 `shift-x` 绑定与可视模式下的默认 `shift-x` 绑定（`vim::VisualDeleteLine`）存在冲突。要设置默认的 vim-exchange 绑定，请将以下按键绑定添加到你的键位配置中：

```json [settings]
{
  "context": "vim_mode == visual",
  "bindings": {
    "shift-x": "vim::Exchange"
  }
}
```

### 恢复常用文本编辑和 Zed 按键绑定

如果你在 Linux 或 Windows 上使用 vim 模式，可能会发现它覆盖了一些你离不开的按键绑定：`ctrl+v` 用于粘贴、`ctrl+f` 用于搜索等。你可以通过将以下数据复制到键位配置中来恢复这些绑定：

```json [keymap]
{
  "context": "Editor && !menu",
  "bindings": {
    "ctrl-c": "editor::Copy",               // vim default: return to normal mode
    "ctrl-x": "editor::Cut",                // vim default: decrement
    "ctrl-v": "editor::Paste",              // vim default: visual block mode
    "ctrl-y": "editor::Undo",               // vim default: line up
    "ctrl-f": "buffer_search::Deploy",      // vim default: page down
    "ctrl-o": "workspace::Open",            // vim default: go back
    "ctrl-s": "workspace::Save",            // vim default: show signature
    "ctrl-a": "editor::SelectAll",          // vim default: increment
    "ctrl-b": "workspace::ToggleLeftDock"   // vim default: down
  }
},
```

## 修改 Vim 模式设置

您可以通过调整以下设置来改变 Vim 模式的行为：

| 属性                         | 描述                                                                                                                                                                                           | 默认值        |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| default_mode                 | 启动时的默认模式。可选值："normal"（常规）、"insert"（插入）、"replace"（替换）、"visual"（可视）、"visual_line"（行可视）、"visual_block"（块可视）、"helix_normal"（螺旋常规）                                                                 | "normal"      |
| use_system_clipboard         | 控制系统剪贴板的使用方式：<br><ul><li>"always"：所有操作均使用</li><li>"never"：仅当明确指定时使用</li><li>"on_yank"：仅复制操作使用</li></ul> | "always"      |
| use_multiline_find           | 已弃用                                                                                                                                                                                    |
| use_smartcase_find           | 当目标字母为小写时，`true`、`f`和`t`操作是否启用大小写不敏感匹配                                                                                                      | false         |
| toggle_relative_line_numbers | 若设为`true`，常规模式下显示相对行号，插入模式下显示绝对行号，实现两种模式的优势结合                                                                         | false         |
| custom_digraphs              | 用于添加自定义双字符组合的对象。下方提供使用示例                                                                                                                  | {}            |
| highlight_on_yank_duration   | 复制高亮动画的持续时间（单位：毫秒）。设为`0`可禁用该功能                                                                                                                         | 200           |

这是一个添加僵尸表情符号合字的示例。通过设置，你可以输入`ctrl-k f z`来插入僵尸表情符号。你可以根据需要添加任意数量的合字。

```json [settings]
{
  "vim": {
    "custom_digraphs": {
      "fz": "🧟‍♀️"
    }
  }
}
```

以下是修改后的设置示例：

```json [settings]
{
  "vim": {
    "default_mode": "insert",
    "use_system_clipboard": "never",
    "use_smartcase_find": true,
    "toggle_relative_line_numbers": true,
    "highlight_on_yank_duration": 50,
    "custom_digraphs": {
      "fz": "🧟‍♀️"
    }
  }
}
```

## Vim模式下实用的Zed核心设置

以下是一些通用的Zed设置，可以帮助你优化Vim使用体验：

| 属性                   | 描述                                                                                                                                                             | 默认值               |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| cursor_blink           | 若设为`true`，光标将闪烁显示                                                                                                                                   | `true`               |
| relative_line_numbers  | 若设为`true`，左侧边栏的行号将相对于光标位置显示                                                                                                             | `false`              |
| scrollbar              | 控制滚动条显示的对象。设为`{ "show": "never" }`可隐藏滚动条                                                                                                             | `{ "show": "auto" }` |
| scroll_beyond_last_line| 设为`"one_page"`时允许滚动至末行后一页。设为`"off"`可禁止此行为                                                                                             | `"one_page"`         |
| vertical_scroll_margin | 滚动时在光标上下方保留的行数。设为`0`时光标可垂直移动至屏幕边缘                                                                                             | `3`                  |
| gutter.line_numbers    | 控制边栏行号显示。将`"line_numbers"`属性设为`false`可隐藏行号                                                                                                | `true`               |
| command_aliases        | 定义命令面板中命令别名的对象。可用于为常用命令定义快捷名称。具体示例请参阅下文说明                                                                                         | `{}`                 |

以下是一个修改这些设置的示例：

```json [settings]
{
  // Disable cursor blink
  "cursor_blink": false,
  // Use relative line numbers
  "relative_line_numbers": true,
  // Hide the scroll bar
  "scrollbar": { "show": "never" },
  // Prevent the buffer from scrolling beyond the last line
  "scroll_beyond_last_line": "off",
  // Allow the cursor to reach the edges of the screen
  "vertical_scroll_margin": 0,
  "gutter": {
    // Disable line numbers completely:
    "line_numbers": false
  },
  "command_aliases": {
    "W": "w",
    "Wq": "wq",
    "Q": "q"
  }
}
```

`command_aliases` 属性是一个独立对象，用于将按键或按键序列映射到 vim 模式命令。上例定义了多个别名：`W` 对应 `w`，`Wq` 对应 `wq`，`Q` 对应 `q`。

## 正则表达式差异

Zed 使用的正则表达式引擎与 Vim 不同。这意味着在某些情况下需要使用不同的语法。以下是最常见的差异：

- **捕获组**：Vim使用`\(`和`\)`表示捕获组，而在Zed中则对应`(`和`)`。相反地，在Vim中`(`和`)`表示原义括号，但在Zed中必须转义为`\(`和`\)`。
- **匹配项**：替换时，Vim使用反斜杠加数字表示匹配的捕获组，例如`\1`。Zed则改用美元符号。因此，Vim中用`\0`表示完整匹配，在Zed中需改用`$0`语法。编号捕获组同理：Vim的`\1`在Zed中对应`$1`。
- **全局选项**：Vim默认每行仅匹配首个结果，需在查询末尾添加`/g`来匹配所有结果。Zed的正则搜索默认即为全局匹配。
- **大小写敏感**：Vim使用`/i`表示不区分大小写的搜索。在Zed中，您可以在模式开头写入`(?i)`，或使用快捷键{#kb search::ToggleCaseSensitive}切换大小写敏感设置。

> **注意**：为便于过渡，当您编写 Vim 风格的替换命令时，命令面板会自动修正圆括号并替换分组，`:%s//`。因此，Zed 会将 `%s:/\(a\)(b)/\1/` 转换为搜索 "(a)\(b\)" 并替换为 "$1"。

有关 Zed 正则表达式引擎支持的完整语法，请参阅 [正则表达式库文档](https://docs.rs/regex/latest/regex/#syntax)。