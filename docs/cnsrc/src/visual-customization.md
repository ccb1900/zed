# 视觉自定义

您可以通过 {#action zed::OpenSettings} ({#kb zed::OpenSettings}) 访问 Zed 的 settings.json 配置文件，调整编辑器视觉布局的多个方面。

更多配置信息及其他非视觉设置，请参阅 [Zed 配置指南](./configuring-zed.md)。

## 主题

您可以通过命令面板或菜单中的 {#action zed::Extensions} 安装包含 [主题](./themes.md) 与 [图标主题](./icon-themes.md) 的 Zed 扩展。

使用 {#action theme_selector::Toggle} ({#kb theme_selector::Toggle}) 和 {#action icon_theme_selector::Toggle} 可预览/切换已安装的主题与图标主题，这些操作将修改以下配置项：

```json [settings]
{
  "theme": "One Dark",
  "icon_theme": "Zed (Default)"
}
```

若需为浅色/深色模式分别设置不同主题，可通过以下配置实现：

## 字体设置

```json [settings]
  // 界面字体。使用 ".SystemUIFont" 来采用系统默认字体（macOS 上为 SF Pro），
  // 或使用 ".ZedSans" 来采用捆绑的默认字体（当前为 IBM Plex）
  "ui_font_family": ".SystemUIFont",
  "ui_font_weight": 400, // 字体粗细，使用标准 CSS 单位，范围从 100 到 900。
  "ui_font_size": 16,

// 编辑器缓冲区字体设置
"buffer_font_family": "Berkeley Mono", // 编辑器缓冲区字体名称
"buffer_font_size": 15,                 // 编辑器缓冲区字体大小
"buffer_font_weight": 400,              // 字体粗细（CSS单位，范围100-900）
// 行高设置："舒适"(1.618)、"标准"(1.3) 或自定义值 `{ "custom": 2 }`
"buffer_line_height": "舒适",

// 终端字体设置
"terminal": {
    "font_family": "",
    "font_size": 15,
    // 终端行高：舒适(1.618)、标准(1.3) 或 `{ "custom": 2 }`
    "line_height": "标准",
},

// 代理面板字体设置
"agent_font_size": 15
```

### Font ligatures

By default Zed enable font ligatures which will visually combines certain adjacent characters.

For example `=>` will be displayed as `→` and `!=` will be `≠`。这仅为视觉样式调整，单个字符本身保持不变。

要禁用此行为，请使用：

```json [settings]
{
  "buffer_font_features": {
    "calt": false // Disable ligatures
  }
}
```

### 状态栏

```json [settings]
{
  // Whether to show full labels in line indicator or short ones
  //   - `short`: "2 s, 15 l, 32 c"
  //   - `long`: "2 selections, 15 lines, 32 characters"
  "line_indicator_format": "long"

  // Individual status bar icons can be hidden:
  // "project_panel": {"button": false },
  // "outline_panel": {"button": false },
  // "collaboration_panel": {"button": false },
  // "git_panel": {"button": false },
  // "notification_panel": {"button": false },
  // "agent": {"button": false },
  // "debugger": {"button": false },
  // "diagnostics": {"button": false },
  // "search": {"button": false },
}
```

### 标题栏

## 工作区设置

```json [settings]
{
  // 强制使用 Zed 内置路径提示（文件和目录选择器）
  // 而非操作系统原生选择器（false）。
  "use_system_path_prompts": true,
  // 强制使用 Zed 内置确认提示（"是否要保存？"）
  // 而非操作系统原生提示（false）。在 Linux 系统上此设置会被忽略（始终为 false）。
  "use_system_prompts": true,
```

// 活动窗格样式设置。
"active_pane_modifiers": {
    // 活动窗格的内嵌边框大小，单位为像素。
    "border_size": 0.0,
    // 非活动窗格的不透明度。0表示完全透明，1表示完全不透明。
    "inactive_opacity": 1.0
},

// 底部停靠区的布局模式：contained（内嵌）、full（全宽）、left_aligned（左对齐）、right_aligned（右对齐）
"bottom_dock_layout": "contained",

// 调整停靠区大小时是否同时调整其中所有面板的大小。
// 可以是"left"、"right"和"bottom"的组合。
"resize_all_panels_in_dock": ["left"]
}
[[代码块_0]]json [设置]
    "centered_layout": {
    // 使用居中布局时，中央窗格左侧相对于工作区的相对宽度。
    "left_padding": 0.2,
    // 使用居中布局时，中央窗格右侧相对于工作区的相对宽度。
    "right_padding": 0.2
    },
```

## 编辑器

[[代码块_0]]显示空白字符功能已启用
    "空格": "•",
    "制表符": "→"
  },

  "无用代码淡化": 0.3, // 未使用代码的淡化程度

  // 在私有文件中隐藏变量值的可视化显示
  "屏蔽私有值": false,

// 软换行与标尺设置
"soft_wrap": "none",          // 可选值：不换行、编辑器宽度、首选行长度、边界换行
"preferred_line_length": 80,  // 软换行基准列数
"show_wrap_guides": true,     // 显示/隐藏换行参考线（垂直标尺）
"wrap_guides": [],            // 换行参考线位置设置（字符计数）

// 边栏设置
"gutter": {
    "line_numbers": true,         // 在边栏显示/隐藏行号
    "runnables": true,            // 在边栏显示/隐藏可执行按钮
    "breakpoints": true,          // 在边栏显示/隐藏断点标记
    "folds": true,                // 在边栏显示/隐藏折叠按钮
    "min_line_number_digits": 4   // 行号最小数字位数预留空间
},
"relative_line_numbers": false, // 在边栏显示相对行号

// 缩进参考线
"indent_guides": {
  "enabled": true,
  "line_width": 1,                  // 参考线宽度（像素）[1-10]
  "active_line_width": 1,           // 当前行参考线宽度（像素）[1-10]
  "coloring": "fixed",              // 禁用/固定/智能缩进配色
  "background_coloring": "disabled" // 禁用/智能缩进背景配色
}

[[代码块_0]]json [设置]
"git": {
  "inline_blame": {
    "enabled": true,             // 显示/隐藏行内追溯信息
    "delay_ms": 0,                  // 延迟显示时间（毫秒）
    "min_column": 0,             // 行内追溯信息显示的最小列数
    "padding": 7,                // 代码与行内追溯信息的间距（em单位）
    "show_commit_summary": false // 显示/隐藏提交摘要
  },
  "hunk_style": "staged_hollow"  // 暂存区空心/非暂存区空心样式
}
```

### 编辑器工具栏

### 编辑器滚动条与缩略图 {#editor-scrollbar}

```json [设置]
  // 滚动条相关设置
  "scrollbar": {
    // 编辑器滚动条显示时机（自动，系统，始终，从不）
    "show": "auto",
    "cursors": true,          // 在滚动条中显示光标位置
    "git_diff": true,         // 在滚动条中显示git差异指示器
    "search_results": true,   // 在滚动条中显示缓冲区搜索结果
    "selected_text": true,    // 在滚动条中显示选中文本出现位置
    "selected_symbol": true,  // 在滚动条中显示选中符号出现位置
    "diagnostics": "all",     // 显示诊断信息（无，错误，警告，信息，全部）
    "axes": {
      "horizontal": true,     // 显示/隐藏水平滚动条
      "vertical": true        // 显示/隐藏垂直滚动条
    }
  },
```

// 缩略图相关设置
"minimap": {
  "show": "never",                // 显示时机（自动/始终/从不）
  "display_in": "active_editor",  // 显示位置（当前编辑器/全部编辑器）
  "thumb": "always",              // 滑块显示时机（始终显示/悬停显示）
  "thumb_border": "left_open",    // 滑块边框样式（左侧开口/右侧开口/完整边框/无边框）
  "max_width_columns": 80,        // 缩略图最大宽度（列数）
  "current_line_highlight": null  // 当前行高亮设置（无/行高亮/边栏高亮）
},

// 控制编辑器滚动超出最后一行的行为：关闭、一页、垂直滚动边距
"scroll_beyond_last_line": "one_page",
// 使用键盘滚动时，在光标上下保留的行数
"vertical_scroll_margin": 3,
// 使用鼠标滚动时，在两侧保留的字符数
"horizontal_scroll_margin": 5,
// 滚动灵敏度倍数
"scroll_sensitivity": 1.0,
// 快速滚动的灵敏度倍数（按住alt键滚动时生效）
"fast_scroll_sensitivity": 4.0,
[[代码块_0]]json [设置]
// 每个窗格的最大标签页数量。未设置表示无限制。
"max_tabs": null,

// 自定义标签栏外观
"标签栏": {
  "显示": true,                     // 显示/隐藏标签栏
  "显示导航历史按钮": true,         // 在标签栏显示/隐藏历史按钮
  "显示标签栏按钮": true           // 显示/隐藏按钮（新建、分割、缩放）
},
"标签页": {
  "git状态": false,                // 显示git状态的颜色标识
  "关闭按钮位置": "右侧",          // 关闭按钮位置（左侧、右侧、隐藏）
  "显示关闭按钮": "悬停时",        // 关闭按钮显示方式（悬停时、始终显示、隐藏）
  "文件图标": false,               // 显示文件类型图标
  // 在文件图标中显示诊断信息（关闭、仅错误、全部显示）。需要启用文件图标功能
  "显示诊断信息": "关闭"
}

### 状态栏

### 多缓冲区

### 编辑器补全、代码片段、操作与诊断 {#editor-lsp}

[[代码块_0]]

### 编辑预测 {#editor-ai}

```json [settings]
  "edit_predictions": {
    "mode": "eager",                // Automatically show (eager) or hold-alt (subtle)
    "enabled_in_text_threads": true // Show/hide predictions in agent text threads
  },
  "show_edit_predictions": true     // Show/hide predictions in editor
```

### 编辑器内嵌提示

```json [settings]
{
  "inlay_hints": {
    "enabled": false,
    // Toggle certain types of hints on and off, all switched on by default.
    "show_type_hints": true,
    "show_parameter_hints": true,
    "show_other_hints": true,

    // Whether to show a background for inlay hints (theme `hint.background`)
    "show_background": false, //

    // 编辑后等待请求提示的时间（设为0可禁用防抖）
    "edit_debounce_ms": 700,
    // 滚动后等待请求提示的时间（设为0可禁用防抖）
    "scroll_debounce_ms": 50,

// 一组修饰键，按下时将切换行内提示的可见性。
"toggle_on_modifiers_press": {
  "control": false,
  "shift": false,
  "alt": false,
  "platform": false,
  "function": false
}

[[代码块_0]]json [设置项]
  // 文件查找器设置
  "file_finder": {
    "file_icons": true,         // 显示/隐藏文件图标
    "modal_max_width": "small", // 水平尺寸：小、中、大、超大、全屏
    "git_status": true,         // 显示每个条目的Git状态
    "include_ignored": null     // 结果中包含被Git忽略的文件：是、否、空值
  },

## 项目面板

可通过 {#操作 project_panel::ToggleFocus} ({#快捷键 project_panel::ToggleFocus}) 或 {#操作 pane::RevealInProjectPanel} ({#快捷键 pane::RevealInProjectPanel}) 显示/隐藏项目面板。

[[代码块_0]]

## 代理面板

[[代码块_0]]

更多非可视化AI设置请参阅 [Zed AI 文档](./ai/overview.md)。

## 终端面板

```json [设置]
  // 终端面板设置
  "terminal": {
    "dock": "bottom",                   // 停靠位置：left（左侧）, right（右侧）, bottom（底部）
    "button": true,                     // 显示/隐藏状态栏图标
    "default_width": 640,               // 默认宽度（左右停靠时）
    "default_height": 320,              // 默认高度（底部停靠时）

```json
// 设置终端光标闪烁行为（开启、关闭、终端控制）
"blinking": "terminal_controlled",
// 终端光标的默认形状（方块、竖线、下划线、空心）
"cursor_shape": "block",

// 添加到终端进程环境的环境变量
"env": {
  // "键": "值"
},

// 终端滚动条
"scrollbar": {
  "show": null                       // 显示/隐藏：（自动、系统、总是、从不）
},
// 终端字体设置
"font_family": "Fira Code",
"font_size": 15,
"font_weight": 400,
// 终端行高：舒适（1.618）、标准（1.3）或[[代码块0]]
"line_height": "comfortable",

"max_scroll_history_lines": 10000,   // 回滚历史记录（0=禁用，最大=100000）
```

有关其他非视觉自定义选项，请参阅[终端设置](./configuring-zed.md#terminal)。

### 其他面板

```json
[设置]
  // Git 面板
  "git_panel": {
    "button": true,               // 显示/隐藏状态栏图标
    "dock": "left",               // 停靠位置：左侧，右侧
    "default_width": 360,         // Git 面板默认宽度
    "status_style": "icon",       // 标签颜色，图标
    "sort_by_path": false,        // 按路径排序（否）或按状态排序（是）
    "scrollbar": {
      "show": null                // 显示/隐藏：（自动，系统，始终，从不）
    }
  },

  // 调试器面板
  "debugger": {
    "dock": "bottom",             // 停靠位置：左侧，右侧，底部
    "button": true                // 显示/隐藏状态栏图标
  },
```

// 大纲面板
"outline_panel": {
    "button": true,               // 显示/隐藏状态栏图标
    "default_width": 300,         // Git面板默认宽度
    "dock": "left",               // 停靠位置：左侧，右侧
    "file_icons": true,           // 显示/隐藏文件图标
    "folder_icons": true,         // 目录显示文件图标（true）或V形标记（false）
    "git_status": true,           // 显示Git状态
    "indent_size": 20,            // 嵌套项缩进尺寸（像素）
    "indent_guides": {
      "show": "always"            // 显示缩进参考线（始终，从不）
    },
    "auto_reveal_entries": true,  // 激活缓冲区时在面板中显示对应文件
    "auto_fold_dirs": true,       // 自动折叠仅含单个子目录的文件夹
    "scrollbar": {                // 项目面板滚动条设置
      "show": null                // 显示/隐藏：（自动，系统，始终，从不）
    }
}

## 协作面板

[[代码块_0]]