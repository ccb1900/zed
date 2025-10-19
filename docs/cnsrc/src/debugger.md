# 调试器

Zed 采用[调试适配器协议（DAP）](https://microsoft.github.io/debug-adapter-protocol/)为多种编程语言提供调试功能。
DAP 是一套标准化协议，定义了调试器、编辑器与集成开发环境之间的通信规范。
该协议使 Zed 无需针对特定语言实现调试逻辑，即可支持各类调试器。
Zed 实现了协议的客户端功能，而各类_调试适配器_则负责实现服务端功能。

通过这一协议，用户可以在不同编程语言和运行时环境中，以统一方式进行设置断点、单步执行、查看变量等调试操作。

## 支持的语言

要调试特定语言编写的代码，Zed需要找到该语言的调试适配器。部分调试适配器由Zed直接提供无需额外设置，部分则通过[语言扩展](./extensions/debugger-extensions.md)提供。目前支持调试适配器的语言包括：

<!-- keep this sorted -->

- [C](./languages/c.md#debugging)（内置支持）
- [C++](./languages/cpp.md#debugging)（内置支持）
- [Go](./languages/go.md#debugging)（内置支持）
- [JavaScript](./languages/javascript.md#debugging)（内置支持）
- [PHP](./languages/php.md#debugging)（内置支持）
- [Python](./languages/python.md#debugging)（内置支持）
- [Ruby](./languages/ruby.md#debugging)（通过扩展提供）
- [Rust](./languages/rust.md#debugging)（内置支持）
- [Swift](./languages/swift.md#debugging)（通过扩展提供）
- [TypeScript](./languages/typescript.md#debugging)（内置支持）

> 如果你的语言未被列出，可以通过为其添加调试适配器来贡献力量。查看我们的[调试器扩展](./extensions/debugger-extensions.md)文档获取更多信息。

请点击对应链接查看针对特定语言和适配器的信息与示例，或继续阅读了解适用于所有适配器的 Zed 通用调试功能。

## 快速开始

对于大多数语言，最快捷的启动方式是执行 {#action debugger::Start} ({#kb debugger::Start})。这将打开_新建进程模态窗_，其中会显示当前项目预配置调试任务的上下文列表。调试任务可源自测试用例、入口点（例如 `main` 函数）等多种来源——请查阅对应语言的文档以获取完整的功能支持信息。

你也可以通过点击调试面板右上角的"加号"按钮打开相同的模态窗。

对于未提供预配置调试任务的语言（包括 C、C++ 及部分扩展支持的语言），您可以在项目根目录的 `.zed/debug.json` 文件中定义调试配置。该文件应为配置对象组成的数组：

```json [debug]
[
  {
    "adapter": "CodeLLDB",
    "label": "First configuration"
    // ...
  },
  {
    "adapter": "Debugpy",
    "label": "Second configuration"
    // ...
  }
]
```

请查阅对应语言的文档，获取涵盖典型使用场景的配置示例。当您将配置添加到 `.zed/debug.json` 后，这些配置将出现在新建进程模态窗口的列表中。

若在 `.zed/debug.json` 中未找到配置，Zed 也会从 `.vscode/launch.json` 加载调试配置，并在新建进程模态窗口中显示。

### 启动与附加

Zed调试器提供两种程序调试方式：既可通过_启动_新实例运行程序，也可通过_附加_到现有进程进行调试。具体选择取决于您的调试目标。

启动新实例时，由于Zed（及其底层调试适配器）能完整控制程序生命周期，通常比附加到现有进程更能有效捕获调试信息。运行单元测试或应用程序的调试版本时，采用启动方式是理想选择。

与启动方式相比，附加到现有进程看似不够完善，实则不然。当程序无法重启时（例如生产环境中难以复现的缺陷，或受特殊条件限制时），附加调试便成为不可或缺的手段。

## 配置说明

Zed 在所有调试任务中都需要 `adapter` 和 `label` 字段。此外，Zed 会使用 `build` 字段在调试器启动前执行必要的设置步骤（[详见下文](#build-tasks)），并可通过 `tcp_connection` 字段连接到现有进程。

其余字段均由调试适配器提供，且可包含[任务变量](./tasks.md#variables)。大多数适配器支持 `request`、`program` 和 `cwd`：

```json [debug]
[
  {
    // The label for the debug configuration and used to identify the debug session inside the debug panel & new process modal
    "label": "Example Start debugger config",
    // The debug adapter that Zed should use to debug the program
    "adapter": "Example adapter name",
    // Request:
    //  - launch: Zed will launch the program if specified, or show a debug terminal with the right configuration
    //  - attach: Zed will attach to a running program to debug it, or when the process_id is not specified, will show a process picker (only supported for node currently)
    "request": "launch",
    // The program to debug. This field supports path resolution with ~ or . symbols.
    "program": "path_to_program",
    // cwd: defaults to the current working directory of your project ($ZED_WORKTREE_ROOT)
    "cwd": "$ZED_WORKTREE_ROOT"
  }
]
```

请查阅调试适配器的文档，以获取有关其支持字段的更多信息。

### 构建任务

Zed允许在`build`字段中嵌入Zed任务，该任务会在调试器启动前执行。这在调试器启动前设置环境或运行必要准备步骤时非常实用。

```json [debug]
[
  {
    "label": "Build Binary",
    "adapter": "CodeLLDB",
    "program": "path_to_program",
    "request": "launch",
    "build": {
      "command": "make",
      "args": ["build", "-j8"]
    }
  }
]
```

构建任务还可以通过未替换的标签引用现有任务：

```json [debug]
[
  {
    "label": "Build Binary",
    "adapter": "CodeLLDB",
    "program": "path_to_program",
    "request": "launch",
    "build": "my build task" // Or "my build task for $ZED_FILE"
  }
]
```

### 自动场景创建

基于Zed任务，系统可自动创建场景。自动场景创建功能同时支撑着我们通过代码行侧边栏创建场景的交互方式。
目前支持自动场景创建的语言包括：Rust、Go、Python、JavaScript和TypeScript。

## 断点设置

在编辑器的行号旁点击即可设置断点。  
您可根据需求调整断点属性：右键点击行号旁的断点图标，选择所需功能。  
当前支持以下操作：

- **添加日志**：断点触发时自动输出日志信息  
- **设置条件**：仅当满足指定条件时暂停执行（条件语法取决于调试适配器）  
- **设置命中次数**：达到指定触发次数后才暂停执行  
- **禁用断点**：保留断点显示但暂时失效  

部分调试适配器（如 CodeLLDB 和 JavaScript）会*验证*断点有效性，无法触发的断点将在界面中突出显示。

针对特定项目启用的所有断点都会在调试会话界面的“断点”项中列出。通过界面中的“断点”项，您还可以管理异常断点。
当发生指定类型的异常时，调试适配器将自动暂停。具体支持哪些异常类型取决于调试适配器。

## 设置

调试器的相关设置统一归类在 `settings.json` 的 `debugger` 键下：

- `dock`：决定调试面板在界面中的显示位置
- `stepping_granularity`：设置单步调试的粒度级别
- `save_breakpoints`：控制是否在多次启动 Zed 时保留断点设置
- `button`：设置是否在状态栏显示调试按钮
- `timeout`：设置连接 TCP 调试适配器时的超时时长（毫秒）
- `log_dap_communications`：控制是否记录活跃调试适配器与 Zed 之间的通信消息
- `format_dap_log_messages`：设置添加到调试适配器日志时是否格式化 DAP 消息

### 停靠位置

- 描述：调试面板在用户界面中的定位方式。
- 默认值：`bottom`
- 设置项：debugger.dock

**可选配置**

1. `left` - 调试面板将固定于界面左侧。
2. `right` - 调试面板将固定于界面右侧。
3. `bottom` - 调试面板将固定于界面底部。

```json [settings]
"debugger": {
  "dock": "bottom"
},
```

### 单步执行粒度

- 描述：调试器使用的单步执行精细程度
- 默认值：`line`
- 设置项：`debugger.stepping_granularity`

**可选配置**

1. 语句级 - 单步执行将使程序运行至当前语句执行完毕。
   语句的具体定义由适配器决定，通常可视为等效于代码行。
   例如“for(int i = 0; i < 10; i++)”可能被视为包含三个语句：“int i = 0”、“i < 10”和“i++”。

```json [settings]
{
  "debugger": {
    "stepping_granularity": "statement"
  }
}
```

2. 逐行执行 - 该步骤应允许程序运行至当前源代码行执行完毕。

```json [settings]
{
  "debugger": {
    "stepping_granularity": "line"
  }
}
```

3. 单指令执行 - 该步骤应允许执行一条指令（例如一条x86指令）。

```json [settings]
{
  "debugger": {
    "stepping_granularity": "instruction"
  }
}
```

### 保存断点

- 描述：是否应在Zed会话之间保留断点设置。
- 默认值：`true`
- 设置项：`debugger.save_breakpoints`

**选项**

`boolean` 取值

```json [settings]
{
  "debugger": {
    "save_breakpoints": true
  }
}
```

### 按钮显示

- 描述：是否在调试器工具栏中显示该按钮。
- 默认值：`true`
- 设置项：`debugger.show_button`

**选项**

`boolean` 取值

```json [settings]
{
  "debugger": {
    "show_button": true
  }
}
```

### 超时设置

- 描述：连接到 TCP 调试适配器时的超时错误时间（毫秒）。
- 默认值：`2000`
- 设置项：`debugger.timeout`

**选项**

`integer` 取值

```json [settings]
{
  "debugger": {
    "timeout": 3000
  }
}
```

### 内联值显示

- 描述：是否在调试会话期间启用编辑器内联提示，显示代码中变量的值。
- 默认值：`true`
- 设置项：`inlay_hints.show_value_hints`

**选项**

```json [settings]
{
  "inlay_hints": {
    "show_value_hints": false
  }
}
```

内联值提示也可以通过编辑器工具栏中的“编辑器控件”菜单进行切换。

### 记录 DAP 通信

- 描述：是否记录活动调试适配器与 Zed 之间的消息。（用于 DAP 开发）
- 默认值：false
- 设置项：debugger.log_dap_communications

**选项**

`boolean` 取值

```json [settings]
{
  "debugger": {
    "log_dap_communications": true
  }
}
```

### 格式化 DAP 日志消息

- 说明：是否在将 DAP 消息添加到调试适配器日志时进行格式化。（用于 DAP 开发）
- 默认值：false
- 设置项：debugger.format_dap_log_messages

**选项**

`boolean` 取值

```json [settings]
{
  "debugger": {
    "format_dap_log_messages": true
  }
}
```

### 自定义调试适配器

- 说明：自定义程序路径和参数，用于覆盖 Zed 启动特定调试适配器的方式。
- 默认值：适配器特定
- 设置项：`dap.$ADAPTER.binary` 与 `dap.$ADAPTER.args`

可传递 `binary`、`args` 或同时传递二者。`binary` 应为 _调试适配器_ 路径（例如 `lldb-dap`），而非 _调试器_ 路径（例如 `lldb` 本身）。`args` 设置会覆盖 Zed 原本传递给适配器的所有参数。

```json [settings]
{
  "dap": {
    "CodeLLDB": {
      "binary": "/Users/name/bin/lldb-dap",
      "args": ["--wait-for-debugger"]
    }
  }
}
```

## 主题

调试器支持以下主题选项：

- `debugger.accent`：用于高亮断点及断点相关符号的颜色
- `editor.debugger_active_line.background`：活动调试行的背景色

## 故障排除

若遇到调试器相关问题，请[提交 GitHub Issue](https://github.com/zed-industries/zed/issues/new?template=04_bug_debugger.yml)，并尽可能提供详细背景信息。您还可以使用以下功能收集问题相关详情：

- 当调试面板中有运行中的会话时，可执行 {#action dev::CopyDebugAdapterArguments} 操作，将描述 Zed 如何初始化会话的 JSON 数据块复制到剪贴板。这在会话启动失败时特别有用，若提交 GitHub Issue 时附上该信息将极具参考价值。
- 也可通过 {#action dev::OpenDebugAdapterLogs} 操作查看最近调试会话期间 Zed 与调试适配器的完整通信追踪记录。