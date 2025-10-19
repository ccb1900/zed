# 任务

Zed 支持通过其内置终端启动（及重新运行）命令并输出结果。这些命令能够读取 Zed 状态的有限子集（例如当前编辑文件的路径或选中的文本内容）。

```json [tasks]
[
  {
    "label": "Example task",
    "command": "for i in {1..5}; do echo \"Hello $i/5\"; sleep 1; done",
    //"args": [],
    // Env overrides for the command, will be appended to the terminal's environment from the settings.
    "env": { "foo": "bar" },
    // Current working directory to spawn the command into, defaults to current project root.
    //"cwd": "/path/to/working/directory",
    // Whether to use a new terminal tab or reuse the existing one to spawn the process, defaults to `false`.
    "use_new_terminal": false,
    // Whether to allow multiple instances of the same task to be run, or rather wait for the existing ones to finish, defaults to `false`.
    "allow_concurrent_runs": false,
    // What to do with the terminal pane and tab, after the command was started:
    // * `always` — always show the task's pane, and focus the corresponding tab in it (default)
    // * `no_focus` — always show the task's pane, add the task's tab in it, but don't focus it
    // * `never` — do not alter focus, but still add/reuse the task's tab in its pane
    "reveal": "always",
    // What to do with the terminal pane and tab, after the command has finished:
    // * `never` — Do nothing when the command finishes (default)
    // * `always` — always hide the terminal tab, hide the pane also if it was the last tab in it
    // * `on_success` — hide the terminal tab on task success only, otherwise behaves similar to `always`
    "hide": "never",
    // Which shell to use when running a task inside the terminal.
    // May take 3 values:
    // 1. (default) Use the system's default terminal configuration in /etc/passwd
    //      "shell": "system"
    // 2. A program:
    //      "shell": {
    //        "program": "sh"
    //      }
    // 3. A program with arguments:
    //     "shell": {
    //         "with_arguments": {
    //           "program": "/bin/bash",
    //           "args": ["--login"]
    //         }
    //     }
    "shell": "system",
    // Whether to show the task line in the output of the spawned task, defaults to `true`.
    "show_summary": true,
    // Whether to show the command line in the output of the spawned task, defaults to `true`.
    "show_command": true
    // Represents the tags for inline runnable indicators, or spawning multiple tasks at once.
    // "tags": []
  }
]
```

有两个操作驱动着使用任务的工作流程：`task: spawn` 和 `task: rerun`。
`task: spawn` 会打开一个模态窗口，显示当前文件中所有可用的任务。
`task: rerun` 会重新运行最近启动的任务。您也可以从任务模态窗口中重新运行任务。

默认情况下，重新运行任务会复用同一个终端（因为 `"use_new_terminal": false` 的默认设置），但会等待前一个任务完成后再启动（因为 `"allow_concurrent_runs": false` 的默认设置）。

保持 `"use_new_terminal": false` 的设置，并将 `"allow_concurrent_runs": true` 设置为允许在重新运行时取消之前的任务。

## 任务模板

任务可以通过以下方式定义：

- 在全局的 `tasks.json` 文件中；这类任务在您使用的所有 Zed 项目中均可用。该文件通常位于 `~/.config/zed/tasks.json` 目录下。您可以通过 `zed: open tasks` 操作来编辑它们。
- 在工作区特定的（本地） `.zed/tasks.json` 文件中；这类任务仅在包含该工作区的项目中可用。您可以通过 `zed: open project tasks` 操作来编辑工作区特定任务。
- 通过[一次性任务](#oneshot-tasks)即时创建。这些任务具有项目特定性，不会在会话间保留。
- 通过语言扩展实现。

## 变量

Zed 任务的工作方式与 shell 完全相同；这意味着您可以通过类 shell 的 `$VAR_NAME` 语法引用环境变量。为了方便使用，系统还预设了若干额外的环境变量。
这些变量允许您从当前编辑器中提取信息并在任务中使用。以下是可用的变量：

- `ZED_COLUMN`：当前列号
- `ZED_ROW`：当前行号
- `ZED_FILE`：当前打开文件的绝对路径（例如`/Users/my-user/path/to/project/src/main.rs`）
- `ZED_FILENAME`：当前打开文件的文件名（例如`main.rs`）
- `ZED_DIRNAME`：去除文件名的当前打开文件绝对路径（例如`/Users/my-user/path/to/project/src`）
- `ZED_RELATIVE_FILE`：当前打开文件相对于`ZED_WORKTREE_ROOT`的路径（例如`src/main.rs`）
- `ZED_RELATIVE_DIR`：当前打开文件所在目录相对于`ZED_WORKTREE_ROOT`的路径（例如`src`）
- `ZED_STEM`：当前打开文件的主干名（不含扩展名的文件名，例如`main`）
- `ZED_SYMBOL`：当前选中的符号；应与符号导航栏显示的最后一个符号匹配（例如`mod tests > fn test_task_contexts`）
- `ZED_SELECTED_TEXT`：当前选中的文本
- `ZED_WORKTREE_ROOT`：当前工作树根目录的绝对路径（例如`/Users/my-user/path/to/project`）
- `ZED_CUSTOM_RUST_PACKAGE`：（Rust专用）$ZED_FILE源文件所属父包的名称

要在任务中使用变量，请在其前加上美元符号（`$`）：

```json [settings]
{
  "label": "echo current file's path",
  "command": "echo $ZED_FILE"
}
```

您也可以使用详细语法，以便在指定变量不可用时设置默认值：`${ZED_FILE:default_value}`

这些环境变量还可用于任务的`cwd`、`args`和`label`字段。

### 变量引用

当处理包含空格或其他特殊字符的路径时，请确保变量已正确转义。

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

在确定任务列表时，若任务定义包含当前不存在的变量，这些任务会被过滤掉。
例如，以下任务仅当存在文本选区时才会出现在生成模态框中：

```json [settings]
{
  "label": "selected text",
  "command": "echo \"$ZED_SELECTED_TEXT\""
}
```

为此类变量设置默认值可确保这些任务始终显示：

```json [settings]
{
  "label": "selected text with default",
  "command": "echo \"${ZED_SELECTED_TEXT:no text selected}\""
}
```

## 单次任务

通过`task: spawn`打开的同一任务模态框支持执行任意类bash命令：在模态框文本字段内输入命令，并使用`opt-enter`来生成执行。

此类临时命令会在整个会话期间持续保存在任务模态框中，若它们是最后执行的任务，`task: rerun`也会重新运行这些任务。

你也可以在模态窗口中调整当前选定的任务（默认快捷键为`tab`）。这样操作会将其命令放入可编辑的提示符中，随后可作为一次性任务启动。

### 临时任务

通过模态窗口启动任务时，可使用`cmd`修饰键；以此方式启动的任务不会增加使用计数（因此它们不会随`task: rerun`重新启动，在任务模态窗口中的排名也不会靠前）。
临时任务的设计初衷是通过持续使用`task: rerun`来保持工作流连贯性。

### 更灵活的任务重运行控制

默认情况下，任务会将其变量捕获到上下文中一次，之后始终运行这个"已解析任务"。

可通过任务的`"reevaluate_context"`参数控制此行为：设置为`true`将强制任务在每次运行前重新评估。

```json [keymap]
{
  "context": "Workspace",
  "bindings": {
    "alt-t": ["task::Rerun", { "reevaluate_context": true }]
  }
}
```

## 任务自定义快捷键绑定

您可以通过向`task::Spawn`添加额外参数来为任务定义专属快捷键。若要将前述`echo current file's path`任务绑定至`alt-g`组合键，只需在您的[`keymap.json`](./key-bindings.md)配置文件中加入以下代码片段：

```json [keymap]
{
  "context": "Workspace",
  "bindings": {
    "alt-g": ["task::Spawn", { "task_name": "echo current file's path" }]
  }
}
```

请注意，这些任务还可通过指定‘target’参数来控制衍生任务的显示位置。
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

Zed 支持通过工作区本地和全局 `tasks.json` 文件覆盖内联可运行指示器的默认操作，优先级顺序如下：

1. 工作区 `tasks.json`
2. 全局 `tasks.json`
3. 语言提供的标签绑定（默认）

要为任务添加标签，请将可运行标签名称添加到任务模板的 `tags` 字段：

```json [settings]
{
  "label": "echo current file's path",
  "command": "echo $ZED_FILE",
  "tags": ["rust-test"]
}
```

通过这种方式，您可以更改可运行指示器中显示的任务。

## 运行绑定至可运行项任务的快捷键

当你有一个与可运行项绑定的任务定义时，可以通过[代码操作](https://zed.dev/docs/configuring-languages?#code-actions)快速运行它。你可以通过`editor: Toggle Code Actions`命令或`cmd-.`/`ctrl-.`快捷键触发这些操作。你的任务将出现在下拉列表的首位。如果当前行没有其他代码操作，该任务将立即执行。