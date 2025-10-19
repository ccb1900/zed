# 任务

Zed 支持通过其内置终端启动（及重新运行）命令并输出结果。这些命令可读取有限的 Zed 状态信息（例如当前编辑文件的路径或选中的文本内容）。

[[代码块_0]]

使用任务的工作流程主要由两个操作驱动：`task: spawn` 和 `task: rerun`。
`task: spawn` 会打开一个模态窗口，显示当前文件中所有可用的任务。
`task: rerun` 会重新运行最近启动的任务。你也可以通过任务模态窗口重新运行任务。

默认情况下，重新运行任务会复用同一个终端（因为 `"use_new_terminal": false` 的默认设置），但会等待上一个任务完成后再启动（因为 `"allow_concurrent_runs": false` 的默认设置）。

保留 `"use_new_terminal": false` 并将 `"allow_concurrent_runs": true` 设置为允许在重新运行时取消之前的任务。

## 任务模板

任务可以通过以下方式定义：

- 在全局的 `tasks.json` 文件中；这类任务适用于您参与的所有 Zed 项目。该文件通常位于 `~/.config/zed/tasks.json` 目录下。您可以使用 `zed: open tasks` 操作来编辑这些任务。
- 在工作区特定的（本地）`.zed/tasks.json` 文件中；这类任务仅在包含该工作区的项目中可用。您可以使用 `zed: open project tasks` 操作来编辑工作区特定任务。
- 通过[一次性任务](#oneshot-tasks)临时创建。这些任务为项目特定设置，不会在会话间保留。
- 通过语言扩展配置。

## 变量

Zed 任务的操作方式与 shell 完全相同；这意味着您可以通过类 shell 的 `$VAR_NAME` 语法引用环境变量。为了方便使用，系统还预设了一些额外的环境变量。
这些变量允许您从当前编辑器中提取信息并在任务中使用。以下是可用的变量：

- `ZED_COLUMN`：当前光标所在列
- `ZED_ROW`：当前光标所在行
- `ZED_FILE`：当前打开文件的绝对路径（例如 `/Users/my-user/path/to/project/src/main.rs`）
- `ZED_FILENAME`：当前打开文件的文件名（例如 `main.rs`）
- `ZED_DIRNAME`：去除文件名的当前打开文件绝对路径（例如 `/Users/my-user/path/to/project/src`）
- `ZED_RELATIVE_FILE`：当前打开文件相对于 `ZED_WORKTREE_ROOT` 的路径（例如 `src/main.rs`）
- `ZED_RELATIVE_DIR`：当前打开文件所在目录相对于 `ZED_WORKTREE_ROOT` 的路径（例如 `src`）
- `ZED_STEM`：当前打开文件的主文件名（不含扩展名，例如 `main`）
- `ZED_SYMBOL`：当前选中的符号；应与符号导航栏中显示的最后一个符号匹配（例如 `mod tests > fn test_task_contexts`）
- `ZED_SELECTED_TEXT`：当前选中的文本
- `ZED_WORKTREE_ROOT`：当前工作树根目录的绝对路径（例如 `/Users/my-user/path/to/project`）
- `ZED_CUSTOM_RUST_PACKAGE`：（Rust 专用）$ZED_FILE 源文件所属父包的名称

要在任务中使用变量，请在其前添加美元符号（`$`）：

```json [settings]
{
  "label": "echo current file's path",
  "command": "echo $ZED_FILE"
}
```

您也可以使用详细语法，以便在指定变量不可用时设置默认值：`${ZED_FILE:default_value}`

这些环境变量还可以用于任务的`cwd`、`args`和`label`字段。

### 变量引用

当处理包含空格或其他特殊字符的路径时，请确保正确转义变量。

例如，不要这样写（如果路径中有空格，将会失败）：

```json [settings]
{
  "label": "stat current file",
  "command": "stat $ZED_FILE"
}
```

请提供以下形式：

```json [settings]
{
  "label": "stat current file",
  "command": "stat",
  "args": ["$ZED_FILE"]
}
```

或者显式包含转义引号，如下所示：

```json [settings]
{
  "label": "stat current file",
  "command": "stat \"$ZED_FILE\""
}
```

### 基于变量的任务筛选

在确定任务列表时，若任务定义包含当前不存在的变量，则该任务会被过滤掉。
例如，以下任务仅当存在文本选择时才会出现在生成模态框中：

```json [settings]
{
  "label": "selected text",
  "command": "echo \"$ZED_SELECTED_TEXT\""
}
```

为此类变量设置默认值可确保此类任务始终显示：

```json [settings]
{
  "label": "selected text with default",
  "command": "echo \"${ZED_SELECTED_TEXT:no text selected}\""
}
```

## 一次性任务

通过`task: spawn`打开的同一任务模态框支持执行任意类bash命令：在模态框文本字段中输入命令，并使用`opt-enter`生成执行。

此类临时命令会在会话期间持续保存，若其为最后执行的任务，`task: rerun`也会重新运行这些任务。

您还可以在模态窗口中调整当前选定的任务（默认按键绑定为`tab`）。此操作会将其命令置入提示框，编辑后可立即作为一次性任务启动。

### 临时任务

通过模态窗口启动任务时，可使用`cmd`修饰键——以此方式启动的任务不会增加使用计数（既不会通过`task: rerun`自动重启，也不会在任务模态窗口中获得高排名）。
临时任务的设计初衷是让用户通过持续使用`task: rerun`保持工作流连贯性。

### 更灵活的任务重运行控制

默认情况下，任务会一次性捕获变量并生成固定上下文，后续始终重运行这个"已解析任务"。

可通过任务的`"reevaluate_context"`参数进行控制：设置为`true`将强制任务在每次运行前重新解析变量。

```json [keymap]
{
  "context": "Workspace",
  "bindings": {
    "alt-t": ["task::Rerun", { "reevaluate_context": true }]
  }
}
```

## 自定义任务快捷键

您可以通过向 `task::Spawn` 添加额外参数来为任务定义专属快捷键。若要将前述 `echo current file's path` 任务绑定至 `alt-g` 组合键，只需在 [`keymap.json`](./key-bindings.md) 配置文件中加入以下代码片段：

```json [keymap]
{
  "context": "Workspace",
  "bindings": {
    "alt-g": ["task::Spawn", { "task_name": "echo current file's path" }]
  }
}
```

值得注意的是，这些任务还可通过指定‘target’参数来控制衍生任务的显示位置。
这对于启动需要居中显示的终端应用程序尤为实用：

```json [tasks]
// In tasks.json
{
  "label": "start lazygit",
  "command": "lazygit -p $ZED_WORKTREE_ROOT"
}
```

```json [keymap]
// In keymap.json
{
  "context": "Workspace",
  "bindings": {
    "alt-g": [
      "task::Spawn",
      { "task_name": "start lazygit", "reveal_target": "center" }
    ]
  }
}
```

## 将可运行标签绑定至任务模板

Zed 支持通过工作区本地和全局 `tasks.json` 文件覆盖内联可运行指示符的默认操作，优先级层次如下：

1. 工作区 `tasks.json`
2. 全局 `tasks.json`
3. 语言提供的标签绑定（默认）。

要为任务添加标签，请将可运行标签名称添加至任务模板的 `tags` 字段：

```json [settings]
{
  "label": "echo current file's path",
  "command": "echo $ZED_FILE",
  "tags": ["rust-test"]
}
```

通过此操作，您可以更改可运行指示符中显示的任务。

## 运行绑定至可运行项任务的快捷键

当你有一个与可运行项绑定的任务定义时，可以通过[代码操作](https://zed.dev/docs/configuring-languages?#code-actions)快速运行它。你可以通过`editor: Toggle Code Actions`命令或`cmd-.`/`ctrl-.`快捷键触发。你的任务将位于下拉菜单的首位。如果该行没有其他代码操作，任务将立即执行。