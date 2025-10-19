# 视觉自定义

您可以通过 Zed 的 settings.json 配置文件调整编辑器视觉布局的多个方面，该文件可通过 {#action zed::OpenSettings} ({#kb zed::OpenSettings}) 访问。

更多配置信息及其他非视觉设置，请参阅 [Zed 配置指南](./configuring-zed.md)。

## 主题

您可以通过命令面板或菜单中的 {#action zed::Extensions} 安装包含 [主题](./themes.md) 和 [图标主题](./icon-themes.md) 的 Zed 扩展插件。

通过 {#action theme_selector::Toggle} ({#kb theme_selector::Toggle}) 和 {#action icon_theme_selector::Toggle} 可预览/选择已安装的主题与图标主题，这些操作将修改以下设置：

```json [settings]
{
  "theme": "One Dark",
  "icon_theme": "Zed (Default)"
}
```

若需为浅色模式/深色模式分别设置不同主题，可通过以下配置实现：

```json [settings]
{
  "theme": {
    "dark": "One Dark",
    "light": "One Light",
    // Mode to use (dark, light) or "system" to follow the OS's light/dark mode (default)
    "mode": "system"
  },
  "icon_theme": {
    "dark": "Zed (Default)",
    "light": "Zed (Default)",
    // Mode to use (dark, light) or "system" to follow the OS's light/dark mode (default)
    "mode": "system"
  }
}
```

## 字体

```json [settings]
  // 界面字体。使用".SystemUIFont"来采用默认系统字体（macOS系统为SF Pro），
  // 或使用".ZedSans"来采用捆绑的默认字体（当前为IBM Plex）
  "ui_font_family": ".SystemUIFont",
  "ui_font_weight": 400, // 字体粗细，采用标准CSS单位，范围从100到900
  "ui_font_size": 16,

// 缓冲区字体 - 编辑器缓冲区使用
// 使用 ".ZedMono" 作为捆绑的默认等宽字体（当前为 Lilex）
"buffer_font_family": "Berkeley Mono", // 编辑器缓冲区的字体名称
"buffer_font_size": 15,                 // 编辑器缓冲区的字体大小
"buffer_font_weight": 400,              // CSS单位制的字体粗细 [100-900]
// 行高 "舒适" (1.618)、"标准" (1.3) 或自定义：`{ "custom": 2 }`
"buffer_line_height": "舒适",

// 终端字体设置
"terminal": {
  "font_family": "",
  "font_size": 15,
  // 终端行高：舒适 (1.618)、标准 (1.3) 或 `{ "custom": 2 }`
  "line_height": "标准",
},

// 代理面板字体设置
"agent_font_size": 15
```

### Font ligatures

By default Zed enable font ligatures which will visually combines certain adjacent characters.

For example `=>` will be displayed as `→` and `!=` will be `≠`。这纯粹是视觉上的调整，单个字符保持不变。

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

```json [settings]
  // Control which items are shown/hidden in the title bar
  "title_bar": {
    "show_branch_icon": false,      // Show/hide branch icon beside branch switcher
    "show_branch_name": true,       // Show/hide branch name
    "show_project_items": true,     // Show/hide project host and name
    "show_onboarding_banner": true, // Show/hide onboarding banners
    "show_user_picture": true,      // Show/hide user avatar
    "show_sign_in": true,           // Show/hide sign-in button
    "show_menus": false             // Show/hide menus
  },
```

## 工作区

```json [settings]
{
  // 强制使用 Zed 内置路径提示框（文件和目录选择器）
  // 而非操作系统原生选择器（false）。
  "use_system_path_prompts": true,
  // 强制使用 Zed 内置确认提示框（"是否要保存？"）
  // 而非操作系统原生提示框（false）。在 Linux 系统上此设置会被忽略（始终为 false）。
  "use_system_prompts": true,

// 活动窗格样式设置
"active_pane_modifiers": {
    // 活动窗格的内嵌边框尺寸，单位为像素
    "border_size": 0.0,
    // 非活动窗格的不透明度。0表示完全透明，1表示完全不透明
    "inactive_opacity": 1.0
},

// 底部停靠栏的布局模式：contained（内嵌）、full（全宽）、left_aligned（左对齐）、right_aligned（右对齐）
"bottom_dock_layout": "contained",

// 调整停靠栏大小时是否同时调整其中所有面板的大小
// 可设置为"left"、"right"和"bottom"的组合
"resize_all_panels_in_dock": ["left"]
}
```

<!--TBD: Centered layout related settings
```json [设置]
    "centered_layout": {
    // 使用居中布局时，中央窗格左侧相对于工作区的相对宽度
    "left_padding": 0.2,
    // 使用居中布局时，中央窗格右侧相对于工作区的相对宽度
    "right_padding": 0.2
    },
```-->

## 编辑器

`json [settings]
  // Whether the cursor blinks in the editor.
  "cursor_blink": true,

  // Cursor shape for the default editor: bar, block, underline, hollow
  "cursor_shape": null,

  // Highlight the current line in the editor: none, gutter, line, all
  "current_line_highlight": "all",

  // When does the mouse cursor hide: never, on_typing, on_typing_and_movement
  "hide_mouse": "on_typing_and_movement",

  // Whether to highlight all occurrences of the selected text in an editor.
  "selection_highlight": true,

  // Visually show tabs and spaces  (none, all, selection, boundary, trailing)
  "show_whitespaces": "selection",
  "whitespace_map": { // Which characters to show when `显示空白字符` 已启用
    "空格": "•",
    "制表符": "→"
  },

  "无用代码淡化": 0.3, // 未使用代码的淡化程度

  // 在私有文件中隐藏变量值的视觉显示
  "屏蔽私有值": false,

// 软换行与标尺设置
"soft_wrap": "none",          // 换行模式：不换行、编辑器宽度、首选行长度、边界换行
"preferred_line_length": 80,  // 软换行基准列数
"show_wrap_guides": true,     // 显示/隐藏换行参考线（垂直标尺）
"wrap_guides": [],            // 换行参考线位置设置（字符计数）

// 边栏设置
"gutter": {
    "line_numbers": true,         // 在边栏显示/隐藏行号
    "runnables": true,            // 在边栏显示/隐藏可运行按钮
    "breakpoints": true,          // 在边栏显示/隐藏断点标记
    "folds": true,                // 在边栏显示/隐藏折叠按钮
    "min_line_number_digits": 4   // 为行号保留的最小数字位数
},
"relative_line_numbers": false, // 在边栏显示相对行号

// 缩进参考线
"indent_guides": {
  "enabled": true,                    // 是否启用
  "line_width": 1,                    // 参考线宽度（像素）[1-10]
  "active_line_width": 1,             // 当前活动行参考线宽度（像素）[1-10]
  "coloring": "fixed",                // 着色方式：禁用、固定、根据缩进智能着色
  "background_coloring": "disabled"   // 背景着色：禁用、根据缩进智能着色
}
```

### Git Blame {#editor-blame}

```

"git": {
  "inline_blame": {
    "enabled": true,                  // 显示/隐藏行内代码追溯
    "delay_ms": 0,                    // 延迟显示时间（毫秒）
    "min_column": 0,                  // 行内显示追溯信息的最小列数
    "padding": 7,                     // 代码与行内追溯信息之间的间距（em单位）
    "show_commit_summary": false      // 显示/隐藏提交摘要
  },
  "hunk_style": "staged_hollow"       // 代码块样式：暂存区空心、未暂存区空心
}

### 编辑器工具栏

```json [settings]
  // Editor toolbar related settings
  "toolbar": {
    "breadcrumbs": true, // Whether to show breadcrumbs.
    "quick_actions": true, // Whether to show quick action buttons.
    "selections_menu": true, // Whether to show the Selections menu
    "agent_review": true, // Whether to show agent review buttons
    "code_actions": false // Whether to show code action buttons
  }
```

### 编辑器滚动条与缩略图 {#editor-scrollbar}

```json [设置]
  // 滚动条相关设置
  "scrollbar": {
    // 在编辑器中何时显示滚动条（自动，系统，总是，从不）
    "show": "auto",
    "cursors": true,          // 在滚动条中显示光标位置。
    "git_diff": true,         // 在滚动条中显示 git 差异指示器。
    "search_results": true,   // 在滚动条中显示缓冲区搜索结果。
    "selected_text": true,    // 在滚动条中显示选中文本的出现位置。
    "selected_symbol": true,  // 在滚动条中显示选中符号的出现位置。
    "diagnostics": "all",     // 显示诊断信息（无，错误，警告，信息，全部）
    "axes": {
      "horizontal": true,     // 显示/隐藏水平滚动条
      "vertical": true        // 显示/隐藏垂直滚动条
    }
  },

// 缩略图相关设置
  "minimap": {
    "show": "never",                // 显示时机（自动，始终，从不）
    "display_in": "active_editor",  // 显示位置（当前编辑器，所有编辑器）
    "thumb": "always",              // 缩略图显示时机（始终，悬停时）
    "thumb_border": "left_open",    // 缩略图边框（左侧开放，右侧开放，完整边框，无边框）
    "max_width_columns": 80,        // 缩略图最大宽度
    "current_line_highlight": null  // 当前行高亮（无，行高亮，装订线高亮）
  },

// 控制编辑器滚动超出最后一行的行为：关闭、一页、垂直滚动边距
"scroll_beyond_last_line": "one_page",
// 使用键盘滚动时，在光标上下保留的行数
"vertical_scroll_margin": 3,
// 使用鼠标滚动时，在两侧保留的字符数
"horizontal_scroll_margin": 5,
// 滚动灵敏度倍数
"scroll_sensitivity": 1.0,
// 快速滚动的灵敏度倍数（滚动时按住alt键）
"fast_scroll_sensitivity": 4.0,
```

### Editor Tabs

```
// 每个窗格的最大标签页数量。未设置表示无限制。
"max_tabs": null,

// 自定义标签栏外观
"标签栏": {
  "显示": true,                     // 显示/隐藏标签栏
  "显示导航历史按钮": true,         // 在标签栏显示/隐藏历史按钮
  "显示标签栏按钮": true            // 显示/隐藏按钮（新建、分割、缩放）
},
"标签页": {
  "git状态": false,                 // 显示git状态的颜色
  "关闭按钮位置": "右侧",           // 关闭按钮位置（左侧、右侧、隐藏）
  "显示关闭按钮": "悬停时",         // 关闭按钮显示方式（悬停时、始终显示、隐藏）
  "文件图标": false,                // 显示文件类型图标
  // 在文件图标中显示诊断信息（关闭、仅错误、全部）。需要启用file_icons=true
  "显示诊断信息": "关闭"
}

### 状态栏

```json [settings]
  "status_bar": {
    // Show/hide a button that displays the active buffer's language.
    // Clicking the button brings up the language selector.
    // Defaults to true.
    "active_language_button": true,
    // Show/hide a button that displays the cursor's position.
    // Clicking the button brings up an input for jumping to a line and column.
    // Defaults to true.
    "cursor_position_button": true,
  },
  "global_lsp_settings": {
    // Show/hide the LSP button in the status bar.
    // Activity from the LSP is still shown.
    // Button is not shown if "enable_language_server" if false.
    "button": true
  },
```

### 多缓冲区

```json [settings]
{
  // The default number of lines to expand excerpts in the multibuffer by.
  "expand_excerpt_lines": 5,
  // The default number of lines of context provided for excerpts in the multibuffer by.
  "excerpt_context_lines": 2
}
```

### 编辑器补全、代码片段、操作与诊断 {#editor-lsp}

```json [settings]
  "snippet_sort_order": "inline",        // Snippets completions: top, inline, bottom, none
  "show_completions_on_input": true,     // Show completions while typing
  "show_completion_documentation": true, // Show documentation in completions
  "auto_signature_help": false,          // Show method signatures inside parentheses

  // Whether to show the signature help after completion or a bracket pair inserted.
  // If `auto_signature_help` is enabled, this setting will be treated as enabled also.
  "show_signature_help_after_edits": false,

  // Whether to show code action button at start of buffer line.
  "inline_code_actions": true,

  // Which level to use to filter out diagnostics displayed in the editor:
  "diagnostics_max_severity": null,      // off, error, warning, info, hint, null (all)

  // How to render LSP `textDocument/documentColor` colors in the editor.
  "lsp_document_colors": "inlay",        // none, inlay, border, background
```

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

    // 编辑后等待请求提示的时间（设为0可取消防抖延迟）
    "edit_debounce_ms": 700,
    // 滚动后等待请求提示的时间（设为0可取消防抖延迟）
    "scroll_debounce_ms": 50,

// 一组修饰键，按下时将切换行内提示的可见性。
"toggle_on_modifiers_press": {
  "control": false,
  "shift": false,
  "alt": false,
  "platform": false,
  "function": false
}
}
}
```

## File Finder

```

// 文件查找器设置
"file_finder": {
  "file_icons": true,         // 显示/隐藏文件图标
  "modal_max_width": "small", // 水平尺寸：small, medium, large, xlarge, full
  "git_status": true,         // 显示每个条目的git状态
  "include_ignored": null     // 结果中包含git忽略的文件：true, false, null
},

## 项目面板

可通过 {#action project_panel::ToggleFocus} ({#kb project_panel::ToggleFocus}) 或 {#action pane::RevealInProjectPanel} ({#kb pane::RevealInProjectPanel}) 显示/隐藏项目面板。

```json [settings]
  // Project Panel Settings
  "project_panel": {
    "button": true,                 // Show/hide button in the status bar
    "default_width": 240,           // Default panel width
    "dock": "left",                 // Position of the dock (left, right)
    "entry_spacing": "comfortable", // Vertical spacing (comfortable, standard)
    "file_icons": true,             // Show/hide file icons
    "folder_icons": true,           // Show/hide folder icons
    "git_status": true,             // Indicate new/updated files
    "indent_size": 20,              // Pixels for each successive indent
    "auto_reveal_entries": true,    // Show file in panel when activating its buffer
    "auto_fold_dirs": true,         // Fold dirs with single subdir
    "sticky_scroll": true,          // Stick parent directories at top of the project panel.
    "drag_and_drop": true,          // Whether drag and drop is enabled
    "scrollbar": {                  // Project panel scrollbar settings
      "show": null                  // Show/hide: (auto, system, always, never)
    },
    "show_diagnostics": "all",      //
    // Settings related to indent guides in the project panel.
    "indent_guides": {
      // When to show indent guides in the project panel. (always, never)
      "show": "always"
    },
    // Whether to hide the root entry when only one folder is open in the window.
    "hide_root": false,
    // Whether to hide the hidden entries in the project panel.
    "hide_hidden": false
  }
```

## 代理面板

```json [settings]
  "agent": {
    "version": "2",
    "enabled": true,        // Enable/disable the agent
    "button": true,         // Show/hide the icon in the status bar
    "dock": "right",        // Where to dock: left, right, bottom
    "default_width": 640,   // Default width (left/right docked)
    "default_height": 320,  // Default height (bottom docked)
  },
  "agent_font_size": 16
```

更多非可视化AI设置请参阅 [Zed AI 文档](./ai/overview.md)。

## 终端面板

```json [settings]
  // 终端面板设置
  "terminal": {
    "dock": "bottom",                   // 停靠位置：left, right, bottom
    "button": true,                     // 显示/隐藏状态栏图标
    "default_width": 640,               // 默认宽度（左右停靠时）
    "default_height": 320,              // 默认高度（底部停靠时）

// 设置终端光标闪烁行为（开启、关闭、终端控制）
"blinking": "terminal_controlled",
// 终端光标的默认形状（方块、竖线、下划线、空心）
"cursor_shape": "方块",

// 要添加到终端进程环境的环境变量
"env": {
  // "键": "值"
},

// 终端滚动条设置
"scrollbar": {
  "show": null                       // 显示/隐藏：（自动、系统、始终、从不）
},
// 终端字体设置
"font_family": "Fira Code",
"font_size": 15,
"font_weight": 400,
// 终端行高：舒适（1.618）、标准（1.3）或`{ "custom": 2 }`
"line_height": "舒适",

"max_scroll_history_lines": 10000,   // 回滚历史记录（0=禁用，最大值=100000）
```

更多非视觉自定义选项请参阅[终端设置](./configuring-zed.md#terminal)。

### 其他面板

```json [设置]
  // Git 面板
  "git_panel": {
    "button": true,               // 显示/隐藏状态栏图标
    "dock": "left",               // 停靠位置：左侧、右侧
    "default_width": 360,         // Git 面板默认宽度
    "status_style": "icon",       // 标签颜色、图标
    "sort_by_path": false,        // 按路径排序（false）或按状态排序（true）
    "scrollbar": {
      "show": null                // 显示/隐藏：（自动、系统、始终、从不）
    }
  },

  // 调试器面板
  "debugger": {
    "dock": "bottom",             // 停靠位置：左侧、右侧、底部
    "button": true                // 显示/隐藏状态栏图标
  },

// 大纲面板
"outline_panel": {
  "button": true,               // 显示/隐藏状态栏图标
  "default_width": 300,         // Git面板的默认宽度
  "dock": "left",               // 停靠位置：左侧、右侧
  "file_icons": true,           // 显示/隐藏文件图标
  "folder_icons": true,         // 目录显示文件图标（true）或展开箭头（false）
  "git_status": true,           // 显示Git状态
  "indent_size": 20,            // 嵌套项目的缩进大小（像素）
  "indent_guides": {
    "show": "always"            // 显示缩进参考线（始终显示、从不显示）
  },
  "auto_reveal_entries": true,  // 激活缓冲区时在面板中显示对应文件
  "auto_fold_dirs": true,       // 折叠仅包含单个子目录的目录
  "scrollbar": {                // 项目面板滚动条设置
    "show": null                // 显示/隐藏：（自动、系统、始终、从不）
  }
}

## 协作面板

```json [settings]
{
  // Collaboration Panel
  "collaboration_panel": {
    "button": true, // Show/hide status bar icon
    "dock": "left", // Where to dock: left, right
    "default_width": 240 // Default width of the collaboration panel.
  },
  "show_call_status_icon": true, // Shown call status in the OS status bar.

  // Notification Panel
  "notification_panel": {
    // Whether to show the notification panel button in the status bar.
    "button": true,
    // Where to dock the notification panel. Can be 'left' or 'right'.
    "dock": "right",
    // Default width of the notification panel.
    "default_width": 380
  }
}
```
