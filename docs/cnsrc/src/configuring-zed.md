# 配置 Zed

Zed 专为可配置性而设计：我们希望能完美契合您的工作流程和个人偏好。我们提供的默认设置旨在为大多数用户打造舒适的入门体验，但更期待您通过调整配置使其臻于完美。

除了本文介绍的设置外，您可能还需要更改[主题](./themes.md)、配置[快捷键](./key-bindings.md)、设置[任务](./tasks.md)或安装[扩展插件](https://github.com/zed-industries/extensions)。

## 配置文件

<!--
TBD: Settings files. Rewrite with "remote settings" in mind (e.g. [[CODE_BLOCK_0]] on the remote host).
Consider renaming [[CODE_BLOCK_1]] to [[CODE_BLOCK_2]].

TBD: Add settings documentation about how settings are merged as overlays. E.g. project>注意：本地配置会覆盖默认配置。请注意映射类设置会进行合并，而数组类设置会被完全替换且必须包含默认值。
-->

您的设置文件可通过 {#kb zed::OpenSettings} 打开。默认情况下它位于 `~/.config/zed/settings.json`，但如果您在 Linux 系统中设置了 XDG_CONFIG_HOME 环境变量，则文件将存放在 `$XDG_CONFIG_HOME/zed/settings.json`。

该配置会与项目内的所有本地配置进行合并。您可以通过命令面板执行 {#action zed::OpenProjectSettings} 来打开项目设置。这将创建一个包含 `.zed/settings.json` 的 `.zed` 目录。

虽然大多数项目仅需在根目录存放一个设置文件，但您可以根据需要为子目录添加更多本地设置文件。并非所有设置都能在本地文件中配置，仅限影响编辑器和语言工具行为的设置。例如您可以设置 `tab_size`、`formatter` 等，但不能配置 `theme`、`vim_mode` 及类似设置。

配置文件的语法采用支持 `//` 注释的 JSON 超集格式。

## 默认设置

您可以通过命令面板运行 {#action zed::OpenDefaultSettings} 来查看当前 Zed 的默认设置。

提供语言服务器功能的扩展也可能为这些语言服务器提供默认设置。

# 设置

## 活动窗格修饰

- 描述：应用于活动窗格的样式设置。
- 设置项：`active_pane_modifiers`
- 默认值：

```json [settings]
{
  "active_pane_modifiers": {
    "border_size": 0.0,
    "inactive_opacity": 1.0
  }
}
```

### 边框尺寸

- 描述：活动窗格周围边框的尺寸。设置为 0 时，活动窗格不显示任何边框。边框采用内嵌方式绘制。
- 设置项：`border_size`
- 默认值：`0.0`

**选项**

非负 `float` 数值

### 非活动状态透明度

- 描述：非活动面板的不透明度。设为1.0时，非活动窗格与活动窗格具有相同不透明度。若设为0，非活动窗格内容将完全不可见。数值会被限制在[0.0, 1.0]范围内。
- 设置项：`inactive_opacity`
- 默认值：`1.0`

**可选值**

`float` 数值

## 底部停靠布局

- 描述：控制底部停靠区相对于左右停靠区的布局方式。
- 设置项：`bottom_dock_layout`
- 默认值：`"contained"`

**布局选项**

1. 将底部停靠区包含在左右停靠区内，使左右停靠区获得窗口的完整高度。

```json [settings]
{
  "bottom_dock_layout": "contained"
}
```

2. 使底部停靠区占据窗口完整宽度，截断左右停靠区。

```json [settings]
{
  "bottom_dock_layout": "full"
}
```

3. 左对齐底部停靠区，截断左侧停靠区并使右侧停靠区获得窗口完整高度。

```json [settings]
{
  "bottom_dock_layout": "left_aligned"
}
```

4. 将底部停靠区右对齐，使左侧停靠区占据窗口的完整高度，并截断右侧停靠区。

```json [settings]
{
  "bottom_dock_layout": "right_aligned"
}
```

## 代理字体大小

- 描述：代理面板中文本的字体大小。如果未设置，则继承用户界面的字体大小。
- 设置：`agent_font_size`
- 默认值：`null`

**选项**

`integer` 取值范围从 `6` 到 `100` 像素（包含边界值）

## 允许重新换行

- 描述：控制在当前语言范围内允许 {#action editor::Rewrap} 操作的位置
- 设置：`allow_rewrap`
- 默认值：`"in_comments"`

**选项**

1. 仅在注释中允许重新换行：

```json [settings]
{
  "allow_rewrap": "in_comments"
}
```

2. 仅在选定内容中允许重新换行：

```json [settings]
{
  "allow_rewrap": "in_selections"
}
```

3. 允许在任何位置重新换行：

```json [settings]
{
  "allow_rewrap": "anywhere"
}
```

注意：此设置在 Vim 模式下无效，因为已经允许在任何位置进行重新换行。

## 自动缩进

- 描述：输入时是否应根据上下文调整缩进。此设置可按编程语言分别指定。
- 设置项：`auto_indent`
- 默认值：`true`

**可选值**

`boolean` 取值

## 粘贴时自动缩进

- 描述：是否应根据上下文调整粘贴内容的缩进
- 设置项：`auto_indent_on_paste`
- 默认值：`true`

**可选值**

`boolean` 取值

## 自动安装扩展

- 描述：定义需要自动安装或禁止安装的扩展程序
- 设置项：`auto_install_extension`
- 默认值：`{ "html": true }`

**配置说明**

您可以通过列出[扩展安装目录](./extensions/installing-extensions.md#installation-location)下的子文件夹名称，查看当前已安装扩展的标识名：

macOS系统：

```sh
ls ~/Library/Application\ Support/Zed/extensions/installed/
```

Linux系统：

```sh
ls ~/.local/share/zed/extensions/installed
```

定义应安装（`true`）或永不安装（`false`）的扩展。

```json [settings]
{
  "auto_install_extensions": {
    "html": true,
    "dockerfile": true,
    "docker-compose": false
  }
}
```

## 自动保存

- 描述：何时自动保存编辑过的缓冲区。
- 设置：`autosave`
- 默认值：`off`

**选项**

1. 要禁用自动保存，请将其设置为`off`：

```json [settings]
{
  "autosave": "off"
}
```

2. 要在焦点改变时自动保存，请使用`on_focus_change`：

```json [settings]
{
  "autosave": "on_focus_change"
}
```

3. 要在活动窗口改变时自动保存，请使用`on_window_change`：

```json [settings]
{
  "autosave": "on_window_change"
}
```

4. 要在无操作一段时间后自动保存，请使用`after_delay`：

```json [settings]
{
  "autosave": {
    "after_delay": {
      "milliseconds": 1000
    }
  }
}
```

请注意，即使早于配置的无操作时间，当关闭未保存的标签页时也会触发保存操作。

## 点击时自动滚动

- 描述：点击可见文本区域边缘附近时是否滚动。
- 设置：`autoscroll_on_clicks`
- 默认值：`false`

**选项**

`boolean` 取值

## 自动签名帮助

- 描述：在括号内时，在编辑器中显示方法签名。
- 设置：`auto_signature_help`
- 默认值：`false`

**选项**

`boolean` 取值

### 编辑后显示签名帮助

- 描述：在完成输入或插入括号对后是否显示签名帮助。如果启用了 `auto_signature_help`，此设置也将被视为启用。
- 设置：`show_signature_help_after_edits`
- 默认值：`false`

**选项**

`boolean` 取值

## 自动更新

- 描述：是否自动检查更新。
- 设置：`auto_update`
- 默认值：`true`

**选项**

`boolean` 取值

## 基础键位映射

- 描述：基础按键绑定方案。基础键位映射可以通过用户键位映射覆盖。
- 设置：`base_keymap`
- 默认值：`VSCode`

**选项**

1. VS Code

```json [settings]
{
  "base_keymap": "VSCode"
}
```

2. Atom

```json [settings]
{
  "base_keymap": "Atom"
}
```

3. JetBrains

```json [settings]
{
  "base_keymap": "JetBrains"
}
```

4. 无

```json [settings]
{
  "base_keymap": "None"
}
```

5. Sublime Text

```json [settings]
{
  "base_keymap": "SublimeText"
}
```

6. TextMate

```json [settings]
{
  "base_keymap": "TextMate"
}
```

## 缓冲区字体族

- 描述：用于在编辑器中渲染文本的字体名称。
- 设置：`buffer_font_family`
- 默认值：`.ZedMono`。当前别名指向[Lilex](https://lilex.myrt.co)。

**选项**

用户系统上安装的任何字体系列名称，或`".ZedMono"`。

## 缓冲区字体特性

- 描述：为编辑器中的文本启用的OpenType特性。
- 设置：`buffer_font_features`
- 默认值：`null`
- 平台：macOS和Windows。

**选项**

Zed 支持所有可针对特定缓冲区或终端字体启用或禁用的 OpenType 功能，同时支持设置字体特性的数值。

例如，要禁用字体连字，请在设置中添加以下内容：

```json [settings]
{
  "buffer_font_features": {
    "calt": false
  }
}
```

您还可以设置其他 OpenType 功能，例如将 `cv01` 设为 `7`：

```json [settings]
{
  "buffer_font_features": {
    "cv01": 7
  }
}
```

## 缓冲区字体回退

- 说明：设置缓冲区文本的字体回退列表，该列表将与平台的默认回退字体合并。
- 设置项：`buffer_font_fallbacks`
- 默认值：`null`
- 适用平台：macOS 和 Windows。

**选项示例**

例如，要使用 `Nerd Font` 作为回退字体，请在设置中添加以下内容：

```json [settings]
{
  "buffer_font_fallbacks": ["Nerd Font"]
}
```

## 缓冲区字体大小

- 说明：编辑器文本的默认字体尺寸。
- 设置项：`buffer_font_size`
- 默认值：`15`

**选项**

字体大小从`6`到`100`像素（含边界值）

## 缓冲区字体粗细

- 描述：编辑器中文本的默认字体粗细
- 设置项：`buffer_font_weight`
- 默认值：`400`

**可选值**

`integer`取值在`100`到`900`之间

## 缓冲区行高

- 描述：编辑器中文本的默认行高
- 设置项：`buffer_line_height`
- 默认值：`"comfortable"`

**可选值**

`"standard"`、`"comfortable"`或`{ "custom": float }`（`1`为紧凑模式，`2`为宽松模式）

## 居中布局

- 描述：居中布局模式的配置
- 设置项：`centered_layout`
- 默认值：

```json [settings]
"centered_layout": {
  "left_padding": 0.2,
  "right_padding": 0.2,
}
```

**可选值**

`left_padding`和`right_padding`选项用于定义启用居中布局模式时，中央窗格相对于工作区的左右边距宽度。有效取值范围为`0`到`0.4`

## 文件删除时关闭

- 描述：当磁盘上对应的文件被删除时，是否自动关闭编辑器标签页。
- 设置项：`close_on_file_delete`
- 默认值：`false`

**选项**

`boolean` 取值

启用此设置后，系统将自动关闭文件系统中已被删除文件对应的标签页。这对于涉及频繁创建和删除临时文件或草稿文件的工作流尤其有用。禁用时（默认），已删除文件的标签页将保持打开状态，并在其标题上显示删除线。

注意：即使启用此设置，含未保存更改的脏文件也不会被自动关闭，以确保您不会丢失未保存的工作。

## 退出确认

- 描述：关闭应用程序前是否提示用户进行确认。
- 设置项：`confirm_quit`
- 默认值：`false`

**选项**

`boolean` 取值

## 诊断最高严重级别

- 描述：使用哪个级别来过滤编辑器中显示的诊断信息
- 设置：`diagnostics_max_severity`
- 默认值：`null`

**选项**

1. 允许所有诊断信息（默认）：

```json [settings]
{
  "diagnostics_max_severity": "all"
}
```

2. 仅显示错误：

```json [settings]
{
  "diagnostics_max_severity": "error"
}
```

3. 显示错误和警告：

```json [settings]
{
  "diagnostics_max_severity": "warning"
}
```

4. 显示错误、警告和信息：

```json [settings]
{
  "diagnostics_max_severity": "info"
}
```

5. 显示全部（包括提示）：

```json [settings]
{
  "diagnostics_max_severity": "hint"
}
```

## 禁用AI功能

- 描述：是否禁用Zed中的所有AI功能
- 设置：`disable_ai`
- 默认值：`false`

**选项**

`boolean` 取值

## Direnv集成

- 描述：[direnv](https://direnv.net/) 集成配置。需要已安装 `direnv`。
  `direnv` 集成功能使得无需安装语言服务器，即可通过 `direnv` 配置设置的环境变量在 `$PATH` 中检测到它们。
  同时支持在任务中使用这些环境变量。
- 设置项：`load_direnv`
- 默认值：`"direct"`

**选项**

提供两种可选方案：

1. `shell_hook`：使用 shell 钩子加载 direnv。此方式依赖 direnv 在进入目录时自动激活，支持 POSIX shell 和 fish。
2. `direct`：通过 `direnv export json` 加载 direnv。此方式将直接加载 direnv 而不依赖 shell 钩子，但可能导致某些不一致性，适用于所有 shell 环境。

## 多缓冲区双击操作

- 描述：当在多缓冲区的某些摘录（单例缓冲区的部分内容）上双击时应执行的操作  
- 设置项：`double_click_in_multibuffer`  
- 默认值：`"select"`  

**选项**  

1. 作为常规缓冲区处理，选中整个单词（默认）：  

```json [settings]
{
  "double_click_in_multibuffer": "select"
}
```  

2. 将点击的摘录在新标签页中作为新缓冲区打开：  

```json [settings]
{
  "double_click_in_multibuffer": "open"
}
```  

若选择“打开”选项，可通过双击时按住 `alt` 实现常规选中效果。  

## 拖放目标区域尺寸  

- 描述：编辑器中拖放目标区域的相对尺寸（0-0.5），该区域用于将拖入的文件以分割窗格形式打开。例如：0.25 表示若拖放到窗格顶部/底部四分之一区域将创建垂直分割，拖放到左侧/右侧四分之一区域将创建水平分割。  
- 设置项：`drop_target_size`  
- 默认值：`0.2`  

**选项**  

`float` 取值范围介于 `0` 到 `0.5` 之间

## 编辑预测

- 描述：编辑预测相关设置。
- 设置项：`edit_predictions`
- 默认值：

```json [settings]
  "edit_predictions": {
    "disabled_globs": [
      "**/.env*",
      "**/*.pem",
      "**/*.key",
      "**/*.cert",
      "**/*.crt",
      "**/.dev.vars",
      "**/secrets.yml"
    ]
  }
```

**选项配置**

### 禁用通配符规则

- 描述：用于禁用编辑预测的通配符模式列表。此列表将追加到预设的合理默认通配符集中，您添加的任何额外规则都会与默认规则合并生效。
- 设置项：`disabled_globs`
- 默认值：`["**/.env*", "**/*.pem", "**/*.key", "**/*.cert", "**/*.crt", "**/.dev.vars", "**/secrets.yml"]`

**选项说明**

由`string`值组成的列表。

## 禁用编辑预测的范围

- 描述：应禁用编辑预测的语言作用域列表。
- 设置项：`edit_predictions_disabled_in`
- 默认值：`[]`

**选项说明**

由`string`值组成的列表

1. 不在注释中显示编辑预测：

```json [settings]
"disabled_in": ["comment"]
```

2. 不在字符串和注释中显示编辑预测：

```json [settings]
"disabled_in": ["comment", "string"]
```

3. 仅在Go语言中，不在字符串和注释中显示编辑预测：

```json [settings]
{
  "languages": {
    "Go": {
      "edit_predictions_disabled_in": ["comment", "string"]
    }
  }
}
```

## 当前行高亮

- 描述：如何在编辑器中高亮显示当前行。
- 设置：`current_line_highlight`
- 默认：`all`

**选项**

1. 不高亮当前行：

```json [settings]
"current_line_highlight": "none"
```

2. 高亮装订线区域：

```json [settings]
"current_line_highlight": "gutter"
```

3. 高亮编辑器区域：

```json [settings]
"current_line_highlight": "line"
```

4. 高亮整行：

```json [settings]
"current_line_highlight": "all"
```

## 选中内容高亮

- 描述：是否在编辑器中高亮显示所选文本的所有出现位置。
- 设置：`selection_highlight`
- 默认值：`true`

## 圆角选择

- 描述：文本选择区域是否应具有圆角。
- 设置：`rounded_selection`
- 默认值：`true`

## 光标闪烁

- 描述：光标是否闪烁。
- 设置：`cursor_blink`
- 默认值：`true`

**选项**

`boolean` 取值

## 光标形状

- 描述：默认编辑器的光标形状。
- 设置：`cursor_shape`
- 默认值：`bar`

**选项**

1. 垂直条状：

```json [settings]
"cursor_shape": "bar"
```

2. 包围后续字符的块状：

```json [settings]
"cursor_shape": "block"
```

3. 沿后续字符延伸的下划线/下横线：

```json [settings]
"cursor_shape": "underline"
```

4. 围绕后续字符绘制的方框：

```json [settings]
"cursor_shape": "hollow"
```

## 边距

- 描述：编辑器边栏的设置
- 设置项：`gutter`
- 默认值：

```json [settings]
{
  "gutter": {
    "line_numbers": true,
    "runnables": true,
    "breakpoints": true,
    "folds": true,
    "min_line_number_digits": 4
  }
}
```

**可选配置**

- `line_numbers`：是否在边栏显示行号
- `runnables`：是否在边栏显示运行按钮
- `breakpoints`：是否在边栏显示断点标记
- `folds`：是否在边栏显示折叠按钮
- `min_line_number_digits`：为边栏预留的最小字符宽度

## 鼠标隐藏

- 描述：决定在编辑器或输入框内何时隐藏鼠标光标
- 设置项：`hide_mouse`
- 默认值：`on_typing_and_movement`

**可选配置**

1. 永不隐藏鼠标光标：

```json [settings]
"hide_mouse": "never"
```

2. 仅在输入时隐藏：

```json [settings]
"hide_mouse": "on_typing"
```

3. 输入和光标移动时均隐藏：

```json [settings]
"hide_mouse": "on_typing_and_movement"
```

## 代码片段排序顺序

- 描述：决定代码片段相对于其他补全项的排序方式。
- 设置：`snippet_sort_order`
- 默认值：`inline`

**选项**

1. 将代码片段置于补全列表顶部：

```json [settings]
"snippet_sort_order": "top"
```

2. 正常显示代码片段，不设特殊偏好：

```json [settings]
"snippet_sort_order": "inline"
```

3. 将代码片段置于补全列表底部：

```json [settings]
"snippet_sort_order": "bottom"
```

4. 完全不在补全列表中显示代码片段：

```json [settings]
"snippet_sort_order": "none"
```

## 编辑器滚动条

- 描述：是否显示编辑器滚动条及其中的各种元素。
- 设置：`scrollbar`
- 默认值：

```json [settings]
"scrollbar": {
  "show": "auto",
  "cursors": true,
  "git_diff": true,
  "search_results": true,
  "selected_text": true,
  "selected_symbol": true,
  "diagnostics": "all",
  "axes": {
    "horizontal": true,
    "vertical": true,
  },
},
```

### 显示模式

- 描述：何时显示编辑器滚动条。
- 设置：`show`
- 默认：`auto`

**选项**

1. 若有重要信息时显示滚动条，或遵循系统配置行为：

```json [settings]
"scrollbar": {
  "show": "auto"
}
```

2. 匹配系统配置行为：

```json [settings]
"scrollbar": {
  "show": "system"
}
```

3. 始终显示滚动条：

```json [settings]
"scrollbar": {
  "show": "always"
}
```

4. 从不显示滚动条：

```json [settings]
"scrollbar": {
  "show": "never"
}
```

### 光标指示器

- 描述：是否在滚动条中显示光标位置。
- 设置：`cursors`
- 默认：`true`

**选项**

`boolean` 取值

### Git 差异指示器

- 描述：是否在滚动条中显示 Git 差异指示器。
- 设置：`git_diff`
- 默认值：`true`

**选项**

`boolean` 值

### 搜索结果指示器

- 描述：是否在滚动条中显示缓冲区搜索结果。
- 设置：`search_results`
- 默认值：`true`

**选项**

`boolean` 值

### 选中文本指示器

- 描述：是否在滚动条中显示选中文本的出现位置。
- 设置：`selected_text`
- 默认值：`true`

**选项**

`boolean` 值

### 选中符号指示器

- 描述：是否在滚动条中显示选中符号的出现位置。
- 设置：`selected_symbol`
- 默认值：`true`

**选项**

`boolean` 值

### 诊断信息

- 描述：在滚动条中显示哪些诊断指示器。
- 设置：`diagnostics`
- 默认值：`all`

**选项**

1. 显示所有诊断信息：

```json [settings]
{
  "show_diagnostics": "all"
}
```

2. 不显示任何诊断信息：

```json [settings]
{
  "show_diagnostics": "off"
}
```

3. 仅显示错误：

```json [settings]
{
  "show_diagnostics": "error"
}
```

4. 仅显示错误和警告：

```json [settings]
{
  "show_diagnostics": "warning"
}
```

5. 仅显示错误、警告和信息：

```json [settings]
{
  "show_diagnostics": "info"
}
```

### 坐标轴

- 描述：强制启用或禁用每个坐标轴的滚动条
- 设置：`axes`
- 默认值：

```json [settings]
"scrollbar": {
  "axes": {
    "horizontal": true,
    "vertical": true,
  },
}
```

#### 水平轴

- 描述：设为 false 时强制禁用水平滚动条，否则遵循其他设置
- 设置：`horizontal`
- 默认值：`true`

**选项**

`boolean` 取值

#### 垂直轴

- 描述：设为 false 时强制禁用垂直滚动条，否则遵循其他设置
- 设置：`vertical`
- 默认值：`true`

**选项**

`boolean` 取值

## 缩略图

- 描述：与编辑器缩略图相关的设置，用于概览文档内容。
- 设置项：`minimap`
- 默认值：

```json [settings]
{
  "minimap": {
    "show": "never",
    "thumb": "always",
    "thumb_border": "left_open",
    "current_line_highlight": null
  }
}
```

### 显示模式

- 描述：何时在编辑器中显示缩略图。
- 设置项：`show`
- 默认值：`never`

**选项**

1. 始终显示缩略图：

```json [settings]
{
  "show": "always"
}
```

2. 仅当编辑器滚动条可见时显示缩略图：

```json [settings]
{
  "show": "auto"
}
```

3. 从不显示缩略图：

```json [settings]
{
  "show": "never"
}
```

### 视窗指示器显示

- 描述：何时在缩略图中显示视窗指示器（当前可见编辑区域）。
- 设置项：`thumb`
- 默认值：`always`

**选项**

1. 悬停于缩略图时显示视窗指示器：

```json [settings]
{
  "thumb": "hover"
}
```

2. 始终显示视窗指示器：

```json [settings]
{
  "thumb": "always"
}
```

### 缩略图边框

- 描述：缩略图边框的显示样式。
- 设置：`thumb_border`
- 默认：`left_open`

**选项**

1. 缩略图四周均显示边框：

```json [settings]
{
  "thumb_border": "full"
}
```

2. 除左侧外，缩略图其他三边显示边框：

```json [settings]
{
  "thumb_border": "left_open"
}
```

3. 除右侧外，缩略图其他三边显示边框：

```json [settings]
{
  "thumb_border": "right_open"
}
```

4. 仅在缩略图左侧显示边框：

```json [settings]
{
  "thumb_border": "left_only"
}
```

5. 缩略图不显示任何边框：

```json [settings]
{
  "thumb_border": "none"
}
```

### 当前行高亮

- 描述：在缩略图中高亮当前行的方式。
- 设置：`current_line_highlight`
- 默认：`null`

**选项**

1. 继承编辑器的当前行高亮设置：

```json [settings]
{
  "minimap": {
    "current_line_highlight": null
  }
}
```

2. 在迷你地图中高亮显示当前行：

```json [settings]
{
  "minimap": {
    "current_line_highlight": "line"
  }
}
```

或

```json [settings]
{
  "minimap": {
    "current_line_highlight": "all"
  }
}
```

3. 不在迷你地图中高亮显示当前行：

```json [settings]
{
  "minimap": {
    "current_line_highlight": "gutter"
  }
}
```

或

```json [settings]
{
  "minimap": {
    "current_line_highlight": "none"
  }
}
```

## 编辑器标签栏

- 描述：与编辑器标签栏相关的设置。
- 设置：`tab_bar`
- 默认值：

```json [settings]
"tab_bar": {
  "show": true,
  "show_nav_history_buttons": true,
  "show_tab_bar_buttons": true
}
```

### 显示

- 描述：是否在编辑器中显示标签栏。
- 设置：`show`
- 默认值：`true`

**选项**

`boolean` 值

### 导航历史按钮

- 描述：是否显示导航历史按钮。
- 设置项：`show_nav_history_buttons`
- 默认值：`true`

**选项**

`boolean` 取值

### 标签栏按钮

- 描述：是否显示标签栏按钮。
- 设置项：`show_tab_bar_buttons`
- 默认值：`true`

**选项**

`boolean` 取值

## 编辑器标签页

- 描述：编辑器标签页的配置。
- 设置项：`tabs`
- 默认值：

```json [settings]
"tabs": {
  "close_position": "right",
  "file_icons": false,
  "git_status": false,
  "activate_on_close": "history",
  "show_close_button": "hover",
  "show_diagnostics": "off"
},
```

### 关闭按钮位置

- 描述：在标签页内显示关闭按钮的位置。
- 设置项：`close_position`
- 默认值：`right`

**选项**

1. 在右侧显示关闭按钮：

```json [settings]
{
  "close_position": "right"
}
```

2. 在左侧显示关闭按钮：

```json [settings]
{
  "close_position": "left"
}
```

### 文件图标

- 描述：是否在标签页中显示文件图标。
- 设置：`file_icons`
- 默认值：`false`

### Git状态显示

- 描述：是否在标签页中显示Git文件状态。
- 设置：`git_status`
- 默认值：`false`

### 关闭时激活

- 描述：关闭当前标签页后的操作方式。
- 设置：`activate_on_close`
- 默认值：`history`

**选项**

1. 激活之前打开的标签页：

```json [settings]
{
  "activate_on_close": "history"
}
```

2. 若存在则激活右侧相邻标签页：

```json [settings]
{
  "activate_on_close": "neighbour"
}
```

3. 若存在则激活左侧相邻标签页：

```json [settings]
{
  "activate_on_close": "left_neighbour"
}
```

### 显示关闭按钮

- 描述：控制标签页关闭按钮的显示行为。
- 设置：`show_close_button`
- 默认值：`hover`

**选项**

1. 仅在悬停标签页时显示：

```json [settings]
{
  "show_close_button": "hover"
}
```

2. 始终显示：

```json [settings]
{
  "show_close_button": "always"
}
```

3. 即使悬停也永不显示：

```json [settings]
{
  "show_close_button": "hidden"
}
```

### 显示诊断信息

- 描述：是否在标签页中显示诊断指示器。此设置仅在文件图标启用时生效，用于控制标记哪些存在诊断问题的文件。
- 设置：`show_diagnostics`
- 默认值：`off`

**选项**

1. 不标记任何文件：

```json [settings]
{
  "show_diagnostics": "off"
}
```

2. 仅标记存在错误的文件：

```json [settings]
{
  "show_diagnostics": "errors"
}
```

3. 标记存在错误和警告的文件：

```json [settings]
{
  "show_diagnostics": "all"
}
```

### 显示内联代码操作

- 描述：是否在缓冲区行首显示代码操作按钮。
- 设置：`inline_code_actions`
- 默认值：`true`

**选项**

`boolean` 取值

### 拖放选择功能

- 描述：是否允许在缓冲区中拖拽选择文本。`delay`表示启用拖拽功能前必须经过的毫秒数，否则将创建新的文本选区。
- 设置项：`drag_and_drop_selection`
- 默认值：

```json [settings]
"drag_and_drop_selection": {
  "enabled": true,
  "delay": 300
}
```

## 编辑器工具栏

- 描述：是否在编辑器工具栏中显示各类元素。
- 设置项：`toolbar`
- 默认值：

```json [settings]
"toolbar": {
  "breadcrumbs": true,
  "quick_actions": true,
  "selections_menu": true,
  "agent_review": true,
  "code_actions": false
},
```

**选项说明**

每个选项控制特定工具栏元素的显示状态。若所有元素均被隐藏，则编辑器工具栏将不会显示。

## 使用系统标签页

- 描述：是否根据用户的标签页偏好设置允许窗口自动合并标签页（仅限macOS系统）。
- 设置项：`use_system_window_tabs`
- 默认值：`false`

**选项说明**

此设置启用与 macOS 原生窗口标签页功能的集成。当设置为 `true` 时，Zed 窗口可以按照用户设定的系统级标签页偏好（如“始终”、“全屏时”或“从不”）分组到单个 macOS 窗口的标签页中。此设置仅在 macOS 上可用。

## 启用语言服务器

- 描述：是否使用语言服务器提供代码智能提示。
- 设置项：`enable_language_server`
- 默认值：`true`

**选项**

`boolean` 值

## 保存时确保末尾换行符

- 描述：移除文件末尾所有仅包含空白字符的行，并确保末尾仅有一个换行符。
- 设置项：`ensure_final_newline_on_save`
- 默认值：`true`

**选项**

`boolean` 值

## 展开摘要行数

- 描述：在多缓冲区中默认展开摘要的行数
- 设置项：`expand_excerpt_lines`
- 默认值：`5`

**选项**

正 `integer` 值

## 摘要上下文行数

- 描述：在多缓冲区显示摘录时提供的上下文行数。
- 设置项：`excerpt_context_lines`
- 默认值：`2`

**可选范围**

介于1到32之间的正`integer`整数值。超出此范围的数值将被自动限制在此范围内。

## 换行扩展注释

- 描述：当上一行是注释时，是否在新行自动开始注释。
- 设置项：`extend_comment_on_newline`
- 默认值：`true`

**可选值**

`boolean` 值

## 状态栏

- 描述：控制状态栏中的各种元素。请注意状态栏中的某些项目需在其他位置单独设置。
- 设置项：`status_bar`
- 默认值：

```json [settings]
"status_bar": {
  "active_language_button": true,
  "cursor_position_button": true
},
```

存在一项实验性设置，可完全隐藏状态栏。这会导致严重的可用性问题（您将无法使用Zed的诸多功能），但为那些将屏幕空间视为最高优先级的用户提供了该选项。

```json
"status_bar": {
  "experimental.show": false
}
```

## 语言服务器协议

- 说明：语言服务器的配置项
- 设置：`lsp`
- 默认值：`null`

**配置选项**

以下设置可针对特定语言服务器进行自定义：

- `initialization_options`
- `settings`

如需为某个语言服务器单独配置，请在该语言服务器名称对应的`lsp`值中添加配置项。

部分选项会通过`initialization_options`传递给语言服务器。这些属于必须在语言服务器启动时指定的参数，修改后需要重启语言服务器方能生效。

例如若需向`rust-analyzer`传递`check`参数，可使用如下配置：

```json [settings]
"lsp": {
  "rust-analyzer": {
    "initialization_options": {
      "check": {
        "command": "clippy" // rust-analyzer.check.command (default: "check")
      }
    }
  }
}
```

虽然其他选项可以在运行时更改，并应放置在`settings`下：

```json [settings]
"lsp": {
  "yaml-language-server": {
    "settings": {
      "yaml": {
        "keyOrdering": true // Enforces alphabetical ordering of keys in maps
      }
    }
  }
}
```

## 全局LSP设置

- 描述：适用于所有语言服务器的全局LSP设置配置
- 设置：`global_lsp_settings`
- 默认值：

```json [settings]
{
  "global_lsp_settings": {
    "button": true
  }
}
```

**选项**

- `button`：是否在状态栏中显示LSP状态按钮

## LSP高亮去抖动

- 描述：基于当前光标位置从语言服务器查询高亮内容前的防抖延迟时间（毫秒）。
- 设置项：`lsp_highlight_debounce`
- 默认值：`75`

**可选值**

`integer` 代表毫秒的数值

## 功能特性

- 描述：可全局启用或禁用的功能模块
- 设置项：`features`
- 默认值：

```json [settings]
{
  "features": {
    "edit_prediction_provider": "zed"
  }
}
```

### 编辑预测提供程序

- 描述：选择使用的编辑预测提供程序
- 设置项：`edit_prediction_provider`
- 默认值：`"zed"`

**可选方案**

1. 使用 Zeta 作为编辑预测提供程序：

```json [settings]
{
  "features": {
    "edit_prediction_provider": "zed"
  }
}
```

2. 使用 Copilot 作为编辑预测提供程序：

```json [settings]
{
  "features": {
    "edit_prediction_provider": "copilot"
  }
}
```

3. 使用 Supermaven 作为编辑预测提供程序：

```json [settings]
{
  "features": {
    "edit_prediction_provider": "supermaven"
  }
}
```

4. 在所有提供程序中关闭编辑预测

```json [settings]
{
  "features": {
    "edit_prediction_provider": "none"
  }
}
```

## 保存时格式化

- 描述：是否在保存前执行缓冲区格式化
- 设置：`format_on_save`
- 默认值：`on`

**选项**

1. `on`，启用保存时格式化并遵循`formatter`设置：

```json [settings]
{
  "format_on_save": "on"
}
```

2. `off`，禁用保存时格式化：

```json [settings]
{
  "format_on_save": "off"
}
```

## 格式化程序

- 描述：如何执行缓冲区格式化
- 设置：`formatter`
- 默认值：`auto`

**选项**

1. 要使用当前语言服务器，请使用`"language_server"`：

```json [settings]
{
  "formatter": "language_server"
}
```

2. 或者使用外部命令，请采用 `"external"`。指定要运行的格式化程序名称，以及传递给该程序的参数数组。缓冲区文本将通过标准输入传递给程序，格式化后的输出应写入标准输出。例如，以下命令将使用 [`sed(1)`](https://linux.die.net/man/1/sed) 去除行尾空格：

```json [settings]
{
  "formatter": {
    "external": {
      "command": "sed",
      "arguments": ["-e", "s/ *$//"]
    }
  }
}
```

3. 外部格式化工具可选择包含 `{buffer_path}` 占位符，运行时该占位符将包含被格式化缓冲区的路径。格式化工具通过标准输入接收文件内容，重新格式化后输出到标准输出，因此通常无法获知所处理文件的文件名。诸如 Prettier 之类的工具支持通过命令行参数接收文件路径，该参数可用于影响格式化决策。

警告：`{buffer_path}` 不应被用于指示格式化程序从文件名读取内容。您的格式化程序应仅从标准输入读取数据，且不应直接读取或写入文件。

```json [settings]
  "formatter": {
    "external": {
      "command": "prettier",
      "arguments": ["--stdin-filepath", "{buffer_path}"]
    }
  }
```

4. 若要使用已连接语言服务器提供的代码操作，请使用 `"code_actions"`：

```json [settings]
{
  "formatter": [
    // Use ESLint's --fix:
    { "code_action": "source.fixAll.eslint" },
    // Organize imports on save:
    { "code_action": "source.organizeImports" }
  ]
}
```

5. 若要连续使用多个格式化程序，请使用格式化程序数组：

```json [settings]
{
  "formatter": [
    { "language_server": { "name": "rust-analyzer" } },
    {
      "external": {
        "command": "sed",
        "arguments": ["-e", "s/ *$//"]
      }
    }
  ]
}
```

首先将使用 `rust-analyzer` 对代码进行格式化，随后调用 sed 命令。
如果任一格式化程序执行失败，后续程序仍会继续执行。

## 自动闭合

- 描述：在输入左圆括号、方括号、花括号、单引号或双引号时，是否自动添加匹配的闭合字符。
- 设置项：`use_autoclose`
- 默认值：`true`

**选项**

`boolean` 取值

## 始终将括号视为自动闭合

- 描述：控制编辑器如何处理自动闭合字符。
- 设置项：`always_treat_brackets_as_autoclosed`
- 默认值：`false`

**选项**

`boolean` 取值

**示例**

若该设置项配置为 `true`：

1. 在编辑器中输入：`)))`
2. 将光标移至起始位置：`^)))`
3. 再次输入：`)))`

最终结果仍为 `)))` 而非 `))))))`（默认情况下的结果）。

## 文件扫描排除项

- 设置项：`file_scan_exclusions`
- 说明：将被Zed完全排除的文件或文件通配符。这些文件在文件扫描、文件搜索过程中会被跳过，且不会显示在项目文件树中。该设置优先级高于`file_scan_inclusions`。
- 默认值：

```json [settings]
"file_scan_exclusions": [
  "**/.git",
  "**/.svn",
  "**/.hg",
  "**/.jj",
  "**/CVS",
  "**/.DS_Store",
  "**/Thumbs.db",
  "**/.classpath",
  "**/.settings"
],
```

请注意，在settings.json中指定`file_scan_exclusions`将会覆盖上述默认值。如需排除其他项目，您需要在设置中包含所有默认值。

## 文件扫描包含项

- 设置项：`file_scan_inclusions`
- 说明：即使被 git 忽略，仍会被 Zed 包含的文件或文件通配模式。适用于未被 git 跟踪但对项目仍重要的文件。注意：过于宽泛的通配模式会降低 Zed 的文件扫描速度。`file_scan_exclusions` 的优先级高于这些包含规则。
- 默认值：

```json [settings]
"file_scan_inclusions": [".env*"],
```

## 文件类型

- 设置项：`file_types`
- 说明：配置 Zed 如何根据文件名或扩展名选择对应语言。支持通配条目。
- 默认值：

```json [settings]
"file_types": {
  "JSONC": ["**/.zed/**/*.json", "**/zed/**/*.json", "**/Zed/**/*.json", "**/.vscode/**/*.json"],
  "Shell Script": [".env.*"]
}
```

**示例**

若要将所有 `.c` 文件识别为 C++ 文件，将名为 `MyLockFile` 的文件识别为 TOML 文件，将以 `Dockerfile` 开头的文件识别为 Dockerfile：

```json [settings]
{
  "file_types": {
    "C++": ["c"],
    "TOML": ["MyLockFile"],
    "Dockerfile": ["Dockerfile*"]
  }
}
```

## 诊断功能

- 描述：诊断相关功能的配置。
- 设置：`diagnostics`
- 默认值：

```json [settings]
{
  "diagnostics": {
    "include_warnings": true,
    "inline": {
      "enabled": false
    },
    "update_with_cursor": false,
    "primary_only": false,
    "use_rendered": false
  }
}
```

### 内联诊断

- 描述：是否以内联方式显示诊断信息。
- 设置：`inline`
- 默认值：

```json [settings]
{
  "diagnostics": {
    "inline": {
      "enabled": false,
      "update_debounce_ms": 150,
      "padding": 4,
      "min_column": 0,
      "max_severity": null
    }
  }
}
```

**选项**

1. 启用内联诊断。

```json [settings]
{
  "diagnostics": {
    "inline": {
      "enabled": true
    }
  }
}
```

2. 延迟诊断更新，直到最后一次诊断更新后的一段时间。

```json [settings]
{
  "diagnostics": {
    "inline": {
      "enabled": true,
      "update_debounce_ms": 150
    }
  }
}
```

3. 在源代码行末尾与诊断信息起始位置之间设置间距。

```json [settings]
{
  "diagnostics": {
    "inline": {
      "enabled": true,
      "padding": 4
    }
  }
}
```

4. 在指定列处水平对齐内联诊断信息。

```json [settings]
{
  "diagnostics": {
    "inline": {
      "enabled": true,
      "min_column": 80
    }
  }
}
```

5. 仅显示警告和错误类诊断信息。

```json [settings]
{
  "diagnostics": {
    "inline": {
      "enabled": true,
      "max_severity": "warning"
    }
  }
}
```

## Git

- 描述：Git相关功能的配置项。
- 设置：`git`
- 默认值：

```json [settings]
{
  "git": {
    "git_gutter": "tracked_files",
    "inline_blame": {
      "enabled": true
    },
    "branch_picker": {
      "show_author_name": true
    },
    "hunk_style": "staged_hollow"
  }
}
```

### Git 差异标识

- 描述：是否显示 Git 差异标识。
- 设置项：`git_gutter`
- 默认值：`tracked_files`

**选项**

1. 在已跟踪文件中显示 Git 差异标识

```json [settings]
{
  "git": {
    "git_gutter": "tracked_files"
  }
}
```

2. 隐藏 Git 差异标识

```json [settings]
{
  "git": {
    "git_gutter": "hide"
  }
}
```

### 差异标识防抖延迟

- 描述：设置防抖阈值（以毫秒为单位），超过该时长后变更将反映在 Git 差异标识中。
- 设置项：`gutter_debounce`
- 默认值：`null`

**选项**

`integer` 代表毫秒的数值

示例：

```json [settings]
{
  "git": {
    "gutter_debounce": 100
  }
}
```

### 行内 Git 追溯

- 描述：是否在当前聚焦行显示行内 Git 追溯信息。
- 设置项：`inline_blame`
- 默认值：

```json [settings]
{
  "git": {
    "inline_blame": {
      "enabled": true
    }
  }
}
```

**选项**

1. 禁用行内 Git 追溯：

```json [settings]
{
  "git": {
    "inline_blame": {
      "enabled": false
    }
  }
}
```

2. 仅在延迟后显示行内 Git 追溯信息（延迟从光标停止移动时开始计算）：

```json [settings]
{
  "git": {
    "inline_blame": {
      "delay_ms": 500
    }
  }
}
```

3. 在提交日期和作者旁显示提交摘要：

```json [settings]
{
  "git": {
    "inline_blame": {
      "show_commit_summary": true
    }
  }
}
```

4. 设置显示行内追溯信息的最小列宽：

```json [settings]
{
  "git": {
    "inline_blame": {
      "min_column": 80
    }
  }
}
```

5. 设置行尾与行内追溯提示之间的间距（以 em 为单位）：

```json [settings]
{
  "git": {
    "inline_blame": {
      "padding": 10
    }
  }
}
```

### 分支选择器

- 描述：与分支选择器相关的配置项
- 设置项：`branch_picker`
- 默认值：

```json [settings]
{
  "git": {
    "branch_picker": {
      "show_author_name": false
    }
  }
}
```

**选项**

1. 在分支选择器中显示作者姓名：

```json [settings]
{
  "git": {
    "branch_picker": {
      "show_author_name": true
    }
  }
}
```

### 代码块样式

- 描述：应为差异代码块使用何种样式
- 设置：`hunk_style`
- 默认值：

```json [settings]
{
  "git": {
    "hunk_style": "staged_hollow"
  }
}
```

**选项**

1. 以淡出效果和边框显示已暂存的代码块：

```json [settings]
{
  "git": {
    "hunk_style": "staged_hollow"
  }
}
```

2. 以淡出效果和边框显示未暂存的代码块：

```json [settings]
{
  "git": {
    "hunk_style": "unstaged_hollow"
  }
}
```

## 转到定义备选方案

- 描述：当{#action editor::GoToDefinition}操作未能找到定义时应执行的操作
- 设置：`go_to_definition_fallback`
- 默认值：`"find_all_references"`

**选项**

1. 不执行任何操作：

```json [settings]
{
  "go_to_definition_fallback": "none"
}
```

2. 查找相同符号的引用（默认）：

```json [settings]
{
  "go_to_definition_fallback": "find_all_references"
}
```

## 硬制表符

- 描述：使用制表符还是多个空格来缩进行。
- 设置：`hard_tabs`
- 默认值：`false`

**选项**

`boolean` 取值

## Helix 模式

- 描述：是否启用 Helix 模式。启用 `helix_mode` 也会同时启用 `vim_mode`。更多细节请参阅 [Helix 文档](./helix.md)。
- 设置：`helix_mode`
- 默认值：`false`

**选项**

`boolean` 取值

## 缩进参考线

- 描述：与缩进参考线相关的配置。缩进参考线可以为每种语言单独配置。
- 设置：`indent_guides`
- 默认值：

```json [settings]
{
  "indent_guides": {
    "enabled": true,
    "line_width": 1,
    "active_line_width": 1,
    "coloring": "fixed",
    "background_coloring": "disabled"
  }
}
```

**选项**

1. 禁用缩进参考线

```json [settings]
{
  "indent_guides": {
    "enabled": false
  }
}
```

2. 为特定语言启用缩进参考线

```json [settings]
{
  "languages": {
    "Python": {
      "indent_guides": {
        "enabled": true
      }
    }
  }
}
```

3. 启用智能缩进着色（彩虹缩进）
   不同缩进层级使用的颜色在主题中定义（主题键：`accents`），可通过主题覆盖进行自定义

```json [settings]
{
  "indent_guides": {
    "enabled": true,
    "coloring": "indent_aware"
  }
}
```

4. 启用智能缩进背景着色（彩虹缩进）
   不同缩进层级使用的颜色在主题中定义（主题键：`accents`），可通过主题覆盖进行自定义

```json [settings]
{
  "indent_guides": {
    "enabled": true,
    "coloring": "indent_aware",
    "background_coloring": "indent_aware"
  }
}
```

## 悬停弹窗已启用

- 描述：当鼠标悬停在编辑器中的符号上时，是否显示信息提示框。
- 设置项：`hover_popover_enabled`
- 默认值：`true`

**选项**

`boolean` 取值

## 悬停提示延迟

- 描述：显示信息提示框前需要等待的毫秒数。
- 设置项：`hover_popover_delay`
- 默认值：`300`

**选项**

`integer` 代表毫秒数的数值

## 图标主题

- 描述：图标主题设置可通过两种形式指定：既可以指定为图标主题名称，也可以指定为一个包含 `mode`、`dark` 和 `light` 图标主题的对象，用于 Zed 内部的文件/文件夹图标。
- 设置项：`icon_theme`
- 默认值：`Zed (Default)`

### 图标主题对象

- 描述：使用包含 `mode`、`dark` 和 `light` 的对象来指定图标主题。
- 设置项：`icon_theme`
- 默认值：

```json [settings]
"icon_theme": {
  "mode": "system",
  "dark": "Zed (Default)",
  "light": "Zed (Default)"
},
```

### 模式

- 描述：指定图标主题模式。
- 设置：`mode`
- 默认值：`system`

**选项**

1. 将图标主题设置为深色模式

```json [settings]
{
  "mode": "dark"
}
```

2. 将图标主题设置为浅色模式

```json [settings]
{
  "mode": "light"
}
```

3. 将图标主题设置为系统模式

```json [settings]
{
  "mode": "system"
}
```

### 深色

- 描述：深色图标主题的名称。
- 设置：`dark`
- 默认值：`Zed (Default)`

**选项**

在命令面板中运行 {#action icon_theme_selector::Toggle} 操作，查看当前有效的图标主题名称列表。

### 浅色

- 描述：浅色图标主题的名称。
- 设置：`light`
- 默认值：`Zed (Default)`

**选项**

在命令面板中运行 {#action icon_theme_selector::Toggle} 操作，查看当前有效的图标主题名称列表。

## 图片查看器

- 描述：图片查看器功能的设置
- 设置：`image_viewer`
- 默认值：

```json [settings]
{
  "image_viewer": {
    "unit": "binary"
  }
}
```

**选项**

### 单位

- 描述：图像文件大小的单位
- 设置：`unit`
- 默认：`"binary"`

**选项**

1. 使用二进制单位（KiB、MiB）：

```json [settings]
{
  "image_viewer": {
    "unit": "binary"
  }
}
```

2. 使用十进制单位（KB、MB）：

```json [settings]
{
  "image_viewer": {
    "unit": "decimal"
  }
}
```

## 内联提示

- 描述：用于在编辑器中显示带提示的额外文本的配置。
- 设置：`inlay_hints`
- 默认：

```json [settings]
"inlay_hints": {
  "enabled": false,
  "show_type_hints": true,
  "show_parameter_hints": true,
  "show_other_hints": true,
  "show_background": false,
  "edit_debounce_ms": 700,
  "scroll_debounce_ms": 50,
  "toggle_on_modifiers_press": null
}
```

**选项**

内联提示查询由两部分组成：编辑器（客户端）和LSP服务器。  
当上述内联设置更改为启用提示后，编辑器将开始查询特定类型的提示，并响应来自服务器的LSP提示刷新请求。  
此时，服务器可能会返回提示，也可能不会，具体取决于其实现方式，可能需要进一步配置，请参考相应的LSP服务器文档。

以下语言已在Zed中预配置了内联提示：

- [Go](https://docs.zed.dev/languages/go)  
- [Rust](https://docs.zed.dev/languages/rust)  
- [Svelte](https://docs.zed.dev/languages/svelte)  
- [TypeScript](https://docs.zed.dev/languages/typescript)  

使用`lsp`部分进行服务器配置。相应语言文档中提供了示例。

在Zed中，提示不会立即查询，系统采用两种防抖机制，任一种均可设置为0以禁用。  
与设置相关的提示更新不受防抖限制。

`toggle_on_modifiers_press` 的所有可能配置值为：

```json [settings]
"inlay_hints": {
  "toggle_on_modifiers_press": {
    "control": true,
    "shift": true,
    "alt": true,
    "platform": true,
    "function": true
  }
}
```

未指定的值默认为 `false`，如果所有修饰键均为 `false` 或未按下全部修饰键，提示将不会切换。

## 日志

- 描述：日志的配置项。
- 设置项：`journal`
- 默认值：

```json [settings]
"journal": {
  "path": "~",
  "hour_format": "hour12"
}
```

### 路径

- 描述：存储日志条目的目录路径。
- 设置项：`path`
- 默认值：`~`

**可选值**

`string` 类型的值

### 时间格式

- 描述：日志中显示小时的时间格式。
- 设置项：`hour_format`
- 默认值：`hour12`

**可选值**

1. 12小时制：

```json [settings]
{
  "hour_format": "hour12"
}
```

2. 24小时制：

```json [settings]
{
  "hour_format": "hour24"
}
```

## JSX 标签自动闭合

- 描述：是否自动闭合 JSX 标签
- 设置项：`jsx_tag_auto_close`
- 默认值：

```json [settings]
{
  "jsx_tag_auto_close": {
    "enabled": true
  }
}
```

**选项配置**

- `enabled`：是否启用 JSX 标签自动闭合功能

## 语言配置

- 描述：针对特定语言的配置选项
- 设置项：`languages`
- 默认值：`null`

**选项配置**

如需为特定语言覆盖默认设置，请在 `languages` 配置值中添加对应语言名称的条目。例如：

```json [settings]
"languages": {
  "C": {
    "format_on_save": "off",
    "preferred_line_length": 64,
    "soft_wrap": "preferred_line_length"
  },
  "JSON": {
    "tab_size": 4
  }
}
```

以下设置项支持针对不同语言进行独立配置：

- [`enable_language_server`](#启用语言服务器)
- [`ensure_final_newline_on_save`](#保存时确保末尾换行)
- [`format_on_save`](#保存时格式化)
- [`formatter`](#格式化程序)
- [`hard_tabs`](#硬制表符)
- [`preferred_line_length`](#首选行长度)
- [`remove_trailing_whitespace_on_save`](#保存时移除行末空白)
- [`show_edit_predictions`](#显示编辑预测)
- [`show_whitespaces`](#显示空白字符)
- [`whitespace_map`](#空白字符映射)
- [`soft_wrap`](#软换行)
- [`tab_size`](#制表符大小)
- [`use_autoclose`](#使用自动闭合)
- [`always_treat_brackets_as_autoclosed`](#始终将括号视为自动闭合)

这些配置项接受与同名根级设置相同的选项。

## 语言模型

- 描述：语言模型提供商的配置
- 设置项：`language_models`
- 默认值：

```json [settings]
{
  "language_models": {
    "anthropic": {
      "api_url": "https://api.anthropic.com"
    },
    "google": {
      "api_url": "https://generativelanguage.googleapis.com"
    },
    "ollama": {
      "api_url": "http://localhost:11434"
    },
    "openai": {
      "api_url": "https://api.openai.com/v1"
    }
  }
}
```

**选项配置**

用于配置各类AI模型供应商的API链接及认证设置。

## 行指示器格式

- 描述：状态栏中行指示器的显示格式
- 设置项：`line_indicator_format`
- 默认值：`"short"`

**可选格式**

1. 简略格式：

```json [settings]
{
  "line_indicator_format": "short"
}
```

2. 完整格式：

```json [settings]
{
  "line_indicator_format": "long"
}
```

## 联动编辑

- 描述：是否对关联范围进行联动编辑（需语言服务器支持）。例如，当编辑起始`<html>`标签时，对应的结束`</html>`标签内容也会同步更新。
- 设置项：`linked_edits`
- 默认值：`true`

**可选参数**

`boolean` 取值

## LSP文档色彩

- 描述：是否显示语言服务器提供的文档色彩信息
- 设置项：`lsp_document_colors`
- 默认值：`true`

**可选参数**

`boolean` 取值

## 最大标签页数量

- 描述：标签栏中显示的最大标签页数量
- 设置项：`max_tabs`
- 默认值：`null`

**可选参数**

正`integer`数值，或使用`null`表示无限制标签页

## 中键粘贴（仅限Linux系统）

- 描述：在Linux系统上启用中键粘贴功能
- 设置项：`middle_click_paste`
- 默认值：`true`

**可选参数**

`boolean` 取值

## 多光标修饰键

- 描述：决定使用何种修饰键配合鼠标添加多个光标。开启悬停链接鼠标手势时，系统将自动调整以避免与多光标修饰键冲突。
- 设置项：`multi_cursor_modifier`
- 默认值：`alt`

**选项**

1. 在Linux和Windows系统中映射为`Alt`，在macOS系统中映射为`Option`：

```json [settings]
{
  "multi_cursor_modifier": "alt"
}
```

2. 在Linux和Windows系统中映射为`Control`，在macOS系统中映射为`Command`：

```json [settings]
{
  "multi_cursor_modifier": "cmd_or_ctrl" // alias: "cmd", "ctrl"
}
```

## 节点配置

- 描述：Node.js集成功能配置
- 设置项：`node`
- 默认值：

```json [settings]
{
  "node": {
    "ignore_system_version": false,
    "path": null,
    "npm_path": null
  }
}
```

**选项**

- `ignore_system_version`：是否忽略系统自带的Node.js版本
- `path`：自定义Node.js可执行文件路径
- `npm_path`：自定义npm可执行文件路径

## 网络代理

- 描述：为 Zed 配置网络代理。
- 设置：`proxy`
- 默认值：`null`

**选项**

代理设置必须包含代理的 URL。

支持以下 URI 方案：

- `http`
- `https`
- `socks4` - 使用本地 DNS 的 SOCKS4 代理
- `socks4a` - 使用远程 DNS 的 SOCKS4 代理
- `socks5` - 使用本地 DNS 的 SOCKS5 代理
- `socks5h` - 使用远程 DNS 的 SOCKS5 代理

未指定方案时将默认使用 `http`。

默认情况下不使用代理，或 Zed 会尝试从环境变量中获取代理设置，例如 `http_proxy`、`HTTP_PROXY`、`https_proxy`、`HTTPS_PROXY`、`all_proxy`、`ALL_PROXY`、`no_proxy` 和 `NO_PROXY`。

例如，要设置 `http` 代理，请在设置中添加以下内容：

```json [settings]
{
  "proxy": "http://127.0.0.1:10809"
}
```

或设置 `socks5` 代理：

```json [settings]
{
  "proxy": "socks5h://localhost:10808"
}
```

若希望将某些主机排除在代理使用范围之外，请设置 `NO_PROXY` 环境变量。该变量接受以逗号分隔的主机名、主机后缀、IPv4/IPv6地址或网段列表，这些地址将不使用代理。例如，若您的环境包含 `NO_PROXY="google.com, 192.168.1.0/24"`，则 `192.168.1.*`、`google.com` 和 `*.google.com` 中的所有主机都将绕过代理。更多信息请参阅 [reqwest NoProxy 文档](https://docs.rs/reqwest/latest/reqwest/struct.NoProxy.html#method.from_string)。

## 最后窗口关闭时

- 说明：最后一个窗口关闭时的操作
- 设置项：`on_last_window_closed`
- 默认值：`"platform_default"`

**选项**

1. 使用平台默认行为：

```json [settings]
{
  "on_last_window_closed": "platform_default"
}
```

2. 始终退出应用程序：

```json [settings]
{
  "on_last_window_closed": "quit_app"
}
```

## 配置文件

- 描述：可在现有设置基础上应用的配置配置文件
- 设置项：`profiles`
- 默认值：`{}`

**选项说明**

用于定义设置配置文件的配置对象。示例：

```json [settings]
{
  "profiles": {
    "presentation": {
      "buffer_font_size": 20,
      "ui_font_size": 18,
      "theme": "One Light"
    }
  }
}
```

## 预览标签页

- 描述：
  预览标签页允许您在预览模式下打开文件，当您切换到其他文件时，除非您明确固定它们，否则这些文件会自动关闭。这有助于快速查看文件而不会使工作区变得杂乱。预览标签页会以斜体显示其文件名。\
  可通过以下几种方式将预览标签页转换为常规标签页：

  - 双击文件
  - 双击标签页标题
  - 使用 {#action project_panel::OpenPermanent} 操作
  - 编辑文件
  - 将文件拖拽到不同窗格

- 设置项：`preview_tabs`
- 默认值：

```json [settings]
"preview_tabs": {
  "enabled": true,
  "enable_preview_from_file_finder": false,
  "enable_preview_from_code_navigation": false,
}
```

### 从文件查找器启用预览

- 描述：决定从文件查找器中选择文件时是否以预览模式打开文件。
- 设置：`enable_preview_from_file_finder`
- 默认值：`false`

**选项**

`boolean` 取值

### 从代码导航启用预览

- 描述：决定在使用代码导航离开标签页时，预览标签页是否会被替换。
- 设置：`enable_preview_from_code_navigation`
- 默认值：`false`

**选项**

`boolean` 取值

## 文件查找器

### 文件图标

- 描述：是否在文件查找器中显示文件图标。
- 设置：`file_icons`
- 默认值：`true`

### 模态框最大宽度

- 描述：文件查找器模态框的最大宽度。可接受以下取值之一：`small`、`medium`、`large`、`xlarge` 和 `full`。
- 设置：`modal_max_width`
- 默认值：`small`

### 搜索时跳过活动文件焦点

- 描述：决定文件查找器是否应在搜索结果中跳过活动文件的焦点。
- 设置：`skip_focus_for_active_in_search`
- 默认值：`true`

## 水平窗格分割方向

- 描述：水平分割窗格时希望的方向
- 设置：`pane_split_direction_horizontal`
- 默认值：`"up"`

**选项**

1. 向上分割：

```json [settings]
{
  "pane_split_direction_horizontal": "up"
}
```

2. 向下分割：

```json [settings]
{
  "pane_split_direction_horizontal": "down"
}
```

## 垂直窗格分割方向

- 描述：垂直分割窗格时希望的方向
- 设置：`pane_split_direction_vertical`
- 默认值：`"left"`

**选项**

1. 向左分割：

```json [settings]
{
  "pane_split_direction_vertical": "left"
}
```

2. 向右分割：

```json [settings]
{
  "pane_split_direction_vertical": "right"
}
```

## 首选行长度

- 描述：在启用软换行的缓冲区中，指定进行软换行的列位置。
- 设置：`preferred_line_length`
- 默认值：`80`

**选项**

`integer` 取值

## 私有文件

- 描述：用于匹配文件路径的通配符模式，以确定文件是否为私有文件
- 设置：`private_files`
- 默认值：`["**/.env*", "**/*.pem", "**/*.key", "**/*.cert", "**/*.crt", "**/secrets.yml"]`

**选项**

`string` 通配符模式列表

## 默认在线项目

- 描述：是否默认显示在线项目视图。
- 设置：`projects_online_by_default`
- 默认值：`true`

**选项**

`boolean` 取值

## 读取SSH配置

- 描述：是否读取SSH配置文件
- 设置：`read_ssh_config`
- 默认值：`true`

**选项**

`boolean` 取值

## 隐藏私有值

- 描述：在私有文件中隐藏变量的可视显示值
- 设置：`redact_private_values`
- 默认值：`false`

**选项**

`boolean` 取值

## 相对行号

- 描述：是否在侧边栏显示相对行号
- 设置：`relative_line_numbers`
- 默认值：`false`

**选项**

`boolean` 取值

## 保存时移除行尾空白字符

- 描述：在保存缓冲区前是否移除行尾的所有空白字符
- 设置：`remove_trailing_whitespace_on_save`
- 默认值：`true`

**选项**

`boolean` 取值

## 调整停靠面板中所有窗格尺寸

- 描述：调整停靠面板尺寸时是否同时调整其中所有窗格的尺寸。可接受"left"、"right"和"bottom"的任意组合
- 设置：`resize_all_panels_in_dock`
- 默认值：`["left"]`

**选项**

包含以下任意组合的字符串列表：

- `"left"`：同时调整左侧停靠面板窗格
- `"right"`：同时调整右侧停靠面板窗格
- `"bottom"`：同时调整底部停靠面板窗格

## 文件重新打开时恢复

- 描述：再次打开文件时是否尝试恢复其先前状态。该状态按窗格存储。
- 设置项：`restore_on_file_reopen`
- 默认值：`true`

**可选配置**

`boolean` 取值

## 启动时恢复

- 描述：控制启动时的会话恢复行为。
- 设置项：`restore_on_startup`
- 默认值：`last_session`

**可选配置**

1. 恢复退出 Zed 时打开的所有工作区：

```json [settings]
{
  "restore_on_startup": "last_session"
}
```

2. 恢复最后关闭的工作区：

```json [settings]
{
  "restore_on_startup": "last_workspace"
}
```

3. 始终以空白编辑器启动：

```json [settings]
{
  "restore_on_startup": "none"
}
```

## 滚动至末行后

- 描述：编辑器是否允许滚动超过最后一行
- 设置项：`scroll_beyond_last_line`
- 默认值：`"one_page"`

**可选配置**

1. 允许滚动超过最后一行一页的距离：

```json [settings]
{
  "scroll_beyond_last_line": "one_page"
}
```

2. 编辑器将超出最后一行滚动相同行数：

```json [settings]
{
  "scroll_beyond_last_line": "vertical_scroll_margin"
}
```

3. 编辑器不会超出最后一行滚动：

```json [settings]
{
  "scroll_beyond_last_line": "off"
}
```

**选项**

`boolean` 数值

## 滚动灵敏度

- 描述：滚动灵敏度乘数。该乘数在滚动时同时应用于水平和垂直方向的增量值。
- 设置：`scroll_sensitivity`
- 默认值：`1.0`

**选项**

正数 `float` 数值

### 快速滚动灵敏度

- 描述：快速滚动的灵敏度乘数。该乘数在滚动时同时应用于水平和垂直方向的增量值。当用户按住alt或option键滚动时，会触发快速滚动。
- 设置：`fast_scroll_sensitivity`
- 默认值：`4.0`

**选项**

正数 `float` 数值

### 水平滚动边距

- 描述：使用鼠标滚动时，在光标两侧保留的字符数
- 设置项：`horizontal_scroll_margin`
- 默认值：`5`

**可选值**

非负`integer`数值

### 垂直滚动边距

- 描述：使用键盘滚动时，在光标上下方保留的行数
- 设置项：`vertical_scroll_margin`
- 默认值：`3`

**可选值**

非负`integer`数值

## 搜索

- 描述：打开新项目和缓冲区搜索时默认启用的搜索选项
- 设置项：`search`
- 默认值：

```json [settings]
"search": {
  "whole_word": false,
  "case_sensitive": false,
  "include_ignored": false,
  "regex": false
},
```

## 搜索环绕

- 描述：若禁用`search_wrap`，搜索结果不会在文件末尾处环绕
- 设置项：`search_wrap`
- 默认值：`true`

## 从光标位置初始化搜索词

- 描述：何时根据光标下的文本填充新搜索的查询内容。
- 设置：`seed_search_query_from_cursor`
- 默认：`always`

**选项**

1. `always` 始终用光标下的词语填充搜索查询
2. `selection` 仅当有文本被选中时填充搜索查询
3. `never` 从不填充搜索查询

## 启用智能大小写搜索

- 描述：启用后，根据查询内容自动调整搜索的大小写敏感性。如果搜索查询包含任何大写字母，则搜索变为区分大小写；如果仅包含小写字母，则搜索变为不区分大小写。\
  此功能适用于文件内搜索和项目范围搜索。
- 设置：`use_smartcase_search`
- 默认：`false`

**选项**

`boolean` 值

示例：

- 搜索 "function" 将匹配 "function"、"Function"、"FUNCTION" 等
- 搜索 "Function" 将仅匹配 "Function"，不匹配 "function" 或 "FUNCTION"

## 显示调用状态图标

- 描述：是否在状态栏中显示通话状态图标。
- 设置：`show_call_status_icon`
- 默认值：`true`

**选项**

`boolean` 取值

## 补全功能

- 描述：控制此语言的补全处理方式。
- 设置：`completions`
- 默认值：

```json [settings]
{
  "completions": {
    "words": "fallback",
    "words_min_length": 3,
    "lsp": true,
    "lsp_fetch_timeout_ms": 0,
    "lsp_insert_mode": "replace_suffix"
  }
}
```

### 词汇补全

- 描述：控制词汇补全方式。对于大型文档，可能无法获取全部词汇进行补全。
- 设置：`words`
- 默认值：`fallback`

**选项**

1. `enabled` - 始终获取文档词汇作为补全建议，与LSP补全同时提供
2. `fallback` - 仅当LSP响应出错或超时时，使用文档词汇显示补全建议
3. `disabled` - 从不获取文档词汇作为补全建议（仍可通过单独操作查询基于词汇的补全）

### 最小查询词长度

- 描述：自动触发基于单词的补全所需的最小字符数。
  低于该数值时，仍可通过对应的编辑器命令手动触发基于单词的补全功能。
- 设置项：`words_min_length`
- 默认值：`3`

**可选值**

正整数

### 语言服务器协议

- 描述：是否启用语言服务器协议补全功能。
- 设置项：`lsp`
- 默认值：`true`

**可选值**

`boolean` 值

### LSP请求超时时长（毫秒）

- 描述：获取LSP补全时，设定等待特定服务器响应的最长时间。设为0表示无限等待。
- 设置项：`lsp_fetch_timeout_ms`
- 默认值：`0`

**可选值**

`integer` 代表毫秒的数值

### LSP插入模式

- 描述：控制接受LSP补全时被替换的文本范围。
- 设置项：`lsp_insert_mode`
- 默认值：`replace_suffix`

**可选值**

1. `insert` - 使用LSP规范中描述的`insert`范围替换光标前的文本
2. `replace` - 使用LSP规范中描述的`replace`范围替换光标前后的文本
3. `replace_subsequence` - 若将被替换文本是补全文本的子序列，则行为类似`"replace"`，否则类似`"insert"`
4. `replace_suffix` - 若光标后文本是补全内容的后缀，则行为类似`"replace"`，否则类似`"insert"`

## 输入时显示补全建议

- 描述：是否在输入时显示补全建议
- 设置项：`show_completions_on_input`
- 默认值：`true`

**选项**

`boolean` 取值

## 显示补全文档

- 描述：是否在补全菜单中内联或并排显示项目的文档说明
- 设置项：`show_completion_documentation`
- 默认值：`true`

**选项**

`boolean` 取值

## 显示编辑预测

- 描述：是否在输入时自动显示编辑预测，还是通过触发`editor::ShowEditPrediction`手动显示。
- 设置项：`show_edit_predictions`
- 默认值：`true`

**选项**

`boolean` 取值

## 显示空白字符

- 描述：是否在编辑器中渲染空白字符。
- 设置项：`show_whitespaces`
- 默认值：`selection`

**选项**

1. `all`
2. `selection`
3. `none`
4. `boundary`

## 空白字符映射

- 描述：当启用显示空白字符时，指定用于渲染空白字符的符号。
- 设置项：`whitespace_map`
- 默认值：

```json [settings]
{
  "whitespace_map": {
    "space": "•",
    "tab": "→"
  }
}
```

## 软换行

- 描述：是否自动对文本行进行换行以适应编辑器/首选宽度。
- 设置项：`soft_wrap`
- 默认值：`none`

**选项**

1. `none` 通常避免换行，除非行过长
2. `prefer_line`（已弃用，与`none`相同）
3. `editor_width` 对超出编辑器宽度的行进行换行
4. `preferred_line_length` 对超出`preferred_line_length`配置值的行进行换行
5. `bounded` 在`editor_width`和`preferred_line_length`的最小值处换行

## 显示换行参考线

- 描述：是否在编辑器中显示换行参考线（垂直标尺）。若设置为true，当'soft_wrap'设为'preferred_line_length'时，会在'preferred_line_length'值处显示参考线，并显示'wrap_guides'设置指定的其他参考线。
- 设置项：`show_wrap_guides`
- 默认值：`true`

**选项**

`boolean` 取值

## 启用输入时格式化

- 描述：是否在每次输入LSP服务器能力定义的"触发"符号后，使用额外的LSP查询来格式化（及修正）代码
- 设置项：`use_on_type_format`
- 默认值：`true`

**选项**

`boolean` 取值

## 使用自动环绕

- 描述：在输入左括号、方括号、花括号、单引号或双引号字符时，是否自动环绕选中的文本。例如，当您选中文本并输入'('时，Zed将用()环绕该文本。
- 设置：`use_auto_surround`
- 默认值：`true`

**选项**

`boolean` 值

## 使用系统路径提示

- 描述：是否使用系统提供的"打开"和"另存为"对话框。当设置为false时，Zed将使用内置的键盘优先选择器。
- 设置：`use_system_path_prompts`
- 默认值：`true`

**选项**

`boolean` 值

## 使用系统提示

- 描述：是否使用系统提供的提示对话框，例如确认提示。当设置为false时，Zed将使用其内置提示。请注意，在Linux系统上，此选项将被忽略，Zed将始终使用内置提示。
- 设置：`use_system_prompts`
- 默认值：`true`

**选项**

`boolean` 值

## 换行参考线（垂直标尺）

- 描述：在何处显示垂直标尺作为换行参考线。通过将`show_wrap_guides`设置为`false`可禁用此功能。
- 设置项：`wrap_guides`
- 默认值：[]

**可选值**

`integer`列号列表

## 制表符宽度

- 描述：每个制表符对应的空格数量。
- 设置项：`tab_size`
- 默认值：`4`

**可选值**

`integer`数值

## 任务配置

- 描述：Zed内部可运行任务的配置选项
- 设置项：`tasks`
- 默认值：

```json [settings]
{
  "tasks": {
    "variables": {},
    "enabled": true,
    "prefer_lsp": false
  }
}
```

**配置选项**

- `variables`：任务配置的自定义变量
- `enabled`：是否启用任务功能
- `prefer_lsp`：是否优先使用LSP提供的任务而非Zed语言扩展任务

## 遥测数据

- 描述：控制Zed收集的信息类型
- 设置项：`telemetry`
- 默认值：

```json [settings]
"telemetry": {
  "diagnostics": true,
  "metrics": true
},
```

**配置选项**

### 诊断信息

- 描述：用于发送调试相关数据的设置，例如崩溃报告。
- 设置项：`diagnostics`
- 默认值：`true`

**选项**

`boolean` 取值

### 使用指标

- 描述：用于发送匿名使用数据的设置，例如您使用 Zed 时采用的语言环境。
- 设置项：`metrics`
- 默认值：`true`

**选项**

`boolean` 取值

## 终端

- 描述：终端相关配置。
- 设置项：`terminal`
- 默认值：

```json [settings]
{
  "terminal": {
    "alternate_scroll": "off",
    "blinking": "terminal_controlled",
    "copy_on_select": false,
    "keep_selection_on_copy": true,
    "dock": "bottom",
    "default_width": 640,
    "default_height": 320,
    "detect_venv": {
      "on": {
        "directories": [".env", "env", ".venv", "venv"],
        "activate_script": "default"
      }
    },
    "env": {},
    "font_family": null,
    "font_features": null,
    "font_size": null,
    "line_height": "comfortable",
    "minimum_contrast": 45,
    "option_as_meta": false,
    "button": true,
    "shell": "system",
    "toolbar": {
      "breadcrumbs": false
    },
    "working_directory": "current_project_directory",
    "scrollbar": {
      "show": null
    }
  }
}
```

### 终端：程序坞

- 描述：控制程序坞的位置
- 设置：`dock`
- 默认：`bottom`

**选项**

`"bottom"`、`"left"` 或 `"right"`

### 终端：备用滚动

- 描述：设置是否默认启用交替滚动模式（DECSET代码：`?1007`）。交替滚动模式会在备用屏幕（例如运行vim或less等应用程序时）将鼠标滚轮事件转换为上/下方向键输入。终端仍可通过ANSI转义码动态启用或禁用此模式。
- 设置项：`alternate_scroll`
- 默认值：`off`

**选项**

1. 默认关闭交替滚动模式

```json [settings]
{
  "terminal": {
    "alternate_scroll": "off"
  }
}
```

2. 默认开启交替滚动模式

```json [settings]
{
  "terminal": {
    "alternate_scroll": "on"
  }
}
```

### 终端：光标闪烁

- 描述：设置终端内的光标闪烁行为
- 设置项：`blinking`
- 默认值：`terminal_controlled`

**选项**

1. 始终禁止光标闪烁，忽略终端模式

```json [settings]
{
  "terminal": {
    "blinking": "off"
  }
}
```

2. 默认关闭光标闪烁，但允许终端启用闪烁功能

```json [settings]
{
  "terminal": {
    "blinking": "terminal_controlled"
  }
}
```

3. 始终闪烁光标，忽略终端模式

```json [settings]
{
  "terminal": {
    "blinking": "on"
  }
}
```

### 终端：选中时复制

- 描述：在终端中选中文本时是否自动复制到系统剪贴板。
- 设置项：`copy_on_select`
- 默认值：`false`

**可选值**

`boolean` 取值

**示例**

```json [settings]
{
  "terminal": {
    "copy_on_select": true
  }
}
```

### 终端：光标形状

- 描述：控制终端中光标的视觉形状。未明确设置时，默认显示为方块形状。
- 设置项：`cursor_shape`
- 默认值：`null`（默认为方块）

**可选值**

1. 包围后续字符的方块

```json [settings]
{
  "terminal": {
    "cursor_shape": "block"
  }
}
```

2. 竖线形状

```json [settings]
{
  "terminal": {
    "cursor_shape": "bar"
  }
}
```

3. 沿后续字符延伸的下划线

```json [settings]
{
  "terminal": {
    "cursor_shape": "underline"
  }
}
```

4. 围绕后续字符绘制的方框

```json [settings]
{
  "terminal": {
    "cursor_shape": "hollow"
  }
}
```

### 终端：复制时保留选中状态

- 描述：复制文本后是否在终端中保留选中内容。
- 设置：`keep_selection_on_copy`
- 默认值：`true`

**选项**

`boolean` 取值

**示例**

```json [settings]
{
  "terminal": {
    "keep_selection_on_copy": false
  }
}
```

### 终端：环境变量

- 描述：添加至此对象的任何键值对将被加入终端环境变量。键名必须唯一，使用`:`分隔单个变量中的多个值
- 设置：`env`
- 默认值：`{}`

**示例**

```json [settings]
{
  "terminal": {
    "env": {
      "ZED": "1",
      "KEY": "value1:value2"
    }
  }
}
```

### 终端：字体大小

- 描述：终端应使用的字体大小。未设置时默认与编辑器字体大小保持一致  
- 设置项：`font_size`  
- 默认值：`null`  

**可选值**  

`integer` 数值  

```json [settings]
{
  "terminal": {
    "font_size": 15
  }
}
```  

### 终端：字体族  

- 描述：终端应使用的字体。未设置时默认与编辑器字体保持一致  
- 设置项：`font_family`  
- 默认值：`null`  

**可选值**  

用户系统已安装的任意字体族名称  

```json [settings]
{
  "terminal": {
    "font_family": "Berkeley Mono"
  }
}
```  

### 终端：字体特性  

- 描述：终端应使用的字体特性。未设置时默认与编辑器字体特性保持一致  
- 设置项：`font_features`  
- 默认值：`null`  
- 适用平台：macOS 与 Windows  

**可选值**  

参见缓冲区字体特性  

```json [settings]
{
  "terminal": {
    "font_features": {
      "calt": false
      // See Buffer Font Features for more features
    }
  }
}
```

### 终端：行高

- 描述：设置终端的行高。
- 配置项：`line_height`
- 默认值：`standard`

**选项**

1. 使用黄金比例行高 1.618，`comfortable`适合阅读。

```json [settings]
{
  "terminal": {
    "line_height": "comfortable"
  }
}
```

2. 使用`standard`紧凑行高 1.3。此选项对文本用户界面（TUI）特别有用，尤其是当它们使用方框字符时。（默认选项）

```json [settings]
{
  "terminal": {
    "line_height": "standard"
  }
}
```

3. 使用自定义行高。

```json [settings]
{
  "terminal": {
    "line_height": {
      "custom": 2
    }
  }
}
```

### 终端：最小对比度

- 描述：控制终端前景色与背景色的最小对比度。使用APCA（可访问感知对比度算法）进行色彩调整。设置为0可禁用此功能。
- 配置项：`minimum_contrast`
- 默认值：`45`

**选项**

`integer`取值范围为0至106。常用推荐值：

- `0`：无对比度调整
- `45`：大段流畅文本的最低要求（默认）
- `60`：其他内容文本的最低要求
- `75`：正文文本的最低要求
- `90`：正文文本的推荐值

```json [settings]
{
  "terminal": {
    "minimum_contrast": 45
  }
}
```

### 终端：Option键作为Meta键

- 描述：将Option键重新解释为"meta"键，类似于Emacs中的功能。
- 设置：`option_as_meta`
- 默认值：`false`

**选项**

`boolean` 取值

```json [settings]
{
  "terminal": {
    "option_as_meta": true
  }
}
```

### 终端：Shell

- 描述：启动终端时使用的shell程序。
- 设置：`shell`
- 默认值：`system`

**选项**

1. 使用系统的默认终端配置（通常是`/etc/passwd`文件）。

```json [settings]
{
  "terminal": {
    "shell": "system"
  }
}
```

2. 要启动的程序：

```json [settings]
{
  "terminal": {
    "shell": {
      "program": "sh"
    }
  }
}
```

3. 带参数的程序：

```json [settings]
{
  "terminal": {
    "shell": {
      "with_arguments": {
        "program": "/bin/bash",
        "args": ["--login"]
      }
    }
  }
}
```

## 终端：检测虚拟环境 {#terminal-detect_venv}

- 描述：如果在终端工作目录（由工作目录解析确定）中发现[Python虚拟环境](https://docs.python.org/3/library/venv.html)，则自动激活该虚拟环境。
- 设置：`detect_venv`
- 默认值：

```json [settings]
{
  "terminal": {
    "detect_venv": {
      "on": {
        // Default directories to search for virtual environments, relative
        // to the current working directory. We recommend overriding this
        // in your project's settings, rather than globally.
        "directories": [".env", "env", ".venv", "venv"],
        // Can also be `csh`, `fish`, and `nushell`
        "activate_script": "default"
      }
    }
  }
}
```

禁用方式：

```json [settings]
{
  "terminal": {
    "detect_venv": "off"
  }
}
```

## 终端：工具栏

- 描述：是否在终端工具栏中显示各种元素。
- 设置：`toolbar`
- 默认值：

```json [settings]
{
  "terminal": {
    "toolbar": {
      "breadcrumbs": false
    }
  }
}
```

**选项**

目前仅提供`breadcrumbs`选项，该选项控制是否显示可通过`PROMPT_COMMAND`修改的终端标题。

如果终端标题为空，则不会显示面包屑导航。

需要在终端中运行的shell配置为发送标题。

设置标题的示例命令：`echo -e "\e]2;New Title\007";`

### 终端：按钮

- 描述：控制状态栏中终端按钮的显示或隐藏
- 设置：`button`
- 默认值：`true`

**选项**

`boolean`取值

```json [settings]
{
  "terminal": {
    "button": false
  }
}
```

### 终端：工作目录

- 描述：启动终端时使用哪个工作目录。
- 设置项：`working_directory`
- 默认值：`"current_project_directory"`

**选项**

1. 使用当前文件所属的项目目录。若失败则回退至首选项目目录方案

```json [settings]
{
  "terminal": {
    "working_directory": "current_project_directory"
  }
}
```

2. 使用当前工作空间中首个项目的目录。若失败则回退至使用本平台主目录

```json [settings]
{
  "terminal": {
    "working_directory": "first_project_directory"
  }
}
```

3. 始终使用本平台主目录（如能检测到）

```json [settings]
{
  "terminal": {
    "working_directory": "always_home"
  }
}
```

4. 始终使用指定目录。该路径将进行shell扩展。若路径无效，终端将默认使用本平台主目录

```json [settings]
{
  "terminal": {
    "working_directory": {
      "always": {
        "directory": "~/zed/projects/"
      }
    }
  }
}
```

## 交互式解释器

- 描述：交互式解释器设置。
- 设置项：`repl`
- 默认值：

```json [settings]
"repl": {
  // Maximum number of columns to keep in REPL's scrollback buffer.
  // Clamped with [20, 512] range.
  "max_columns": 128,
  // Maximum number of lines to keep in REPL's scrollback buffer.
  // Clamped with [4, 256] range.
  "max_lines": 32
},
```

## 主题

- 描述：主题设置可通过两种形式指定——既可使用主题名称，也可使用包含Zed界面`mode`、`dark`和`light`主题的对象。
- 设置项：`theme`
- 默认值：`One Dark`

### 主题对象

- 描述：使用包含`mode`、`dark`和`light`主题的对象来指定主题。
- 设置项：`theme`
- 默认值：

```json [settings]
"theme": {
  "mode": "system",
  "dark": "One Dark",
  "light": "One Light"
},
```

### 模式

- 描述：指定主题模式。
- 设置：`mode`
- 默认值：`system`

**选项**

1. 将主题设置为深色模式

```json [settings]
{
  "mode": "dark"
}
```

2. 将主题设置为浅色模式

```json [settings]
{
  "mode": "light"
}
```

3. 将主题设置为系统模式

```json [settings]
{
  "mode": "system"
}
```

### 深色主题

- 描述：用于界面的深色 Zed 主题名称。
- 设置：`dark`
- 默认值：`One Dark`

**选项**

在命令面板中运行 {#action theme_selector::Toggle} 操作可查看当前有效的主题名称列表。

### 浅色主题

- 描述：用于界面的浅色 Zed 主题名称。
- 设置：`light`
- 默认值：`One Light`

**选项**

在命令面板中运行 {#action theme_selector::Toggle} 操作可查看当前有效的主题名称列表。

## 标题栏

- 描述：是否在标题栏中显示各种元素
- 设置：`title_bar`
- 默认值：

```json [settings]
"title_bar": {
  "show_branch_icon": false,
  "show_branch_name": true,
  "show_project_items": true,
  "show_onboarding_banner": true,
  "show_user_picture": true,
  "show_sign_in": true,
  "show_menus": false
}
```

**选项**

- `show_branch_icon`：是否在标题栏的分支切换器旁显示分支图标
- `show_branch_name`：是否在标题栏显示分支名称按钮
- `show_project_items`：是否在标题栏显示项目托管平台和项目名称
- `show_onboarding_banner`：是否在标题栏显示新手引导横幅
- `show_user_picture`：是否在标题栏显示用户头像
- `show_sign_in`：是否在标题栏显示登录按钮
- `show_menus`：是否在标题栏显示菜单

## Vim模式

- 描述：是否启用Vim模式
- 设置：`vim_mode`
- 默认值：`false`

## 无标签页时关闭窗口

- 描述：当在无标签页窗口中使用“关闭活动项”时，是否应关闭窗口  
- 设置项：`when_closing_with_no_tabs`  
- 默认值：`"platform_default"`  

**选项**  

1. 使用平台默认行为：  

```json [settings]
{
  "when_closing_with_no_tabs": "platform_default"
}
```  

2. 始终关闭窗口：  

```json [settings]
{
  "when_closing_with_no_tabs": "close_window"
}
```  

3. 从不关闭窗口：  

```json [settings]
{
  "when_closing_with_no_tabs": "keep_window_open"
}
```  

## 项目面板  

- 描述：自定义项目面板  
- 设置项：`project_panel`  
- 默认值：

```json [settings]
{
  "project_panel": {
    "button": true,
    "default_width": 240,
    "dock": "left",
    "entry_spacing": "comfortable",
    "file_icons": true,
    "folder_icons": true,
    "git_status": true,
    "indent_size": 20,
    "auto_reveal_entries": true,
    "auto_fold_dirs": true,
    "drag_and_drop": true,
    "scrollbar": {
      "show": null
    },
    "sticky_scroll": true,
    "show_diagnostics": "all",
    "indent_guides": {
      "show": "always"
    },
    "hide_root": false,
    "hide_hidden": false,
    "starts_open": true,
    "open_file_on_paste": true
  }
}
```

### 程序坞

- 描述：控制程序坞的位置
- 设置：`dock`
- 默认：`left`

**选项**

1. 默认程序坞位置靠左

```json [settings]
{
  "dock": "left"
}
```

2. 默认程序坞位置靠右

```json [settings]
{
  "dock": "right"
}
```

### 工作区条目间距

- 描述：工作区条目之间的间距
- 设置：`entry_spacing`
- 默认：`comfortable`

**选项**

1. 舒适条目间距

```json [settings]
{
  "entry_spacing": "comfortable"
}
```

2. 标准条目间距

```json [settings]
{
  "entry_spacing": "standard"
}
```

### Git状态

- 描述：指示新建和更新的文件
- 设置项：`git_status`
- 默认值：`true`

**选项**

1. 默认启用Git状态

```json [settings]
{
  "git_status": true
}
```

2. 默认禁用Git状态

```json [settings]
{
  "git_status": false
}
```

### 默认宽度

- 描述：自定义项目面板占用的默认宽度
- 设置项：`default_width`
- 默认值：`240`

**选项**

`float` 数值

### 自动显示条目

- 描述：当对应项目条目激活时，是否在项目面板中自动显示。被Git忽略的条目永远不会自动显示。
- 设置项：`auto_reveal_entries`
- 默认值：`true`

**选项**

1. 启用自动显示条目

```json [settings]
{
  "auto_reveal_entries": true
}
```

2. 禁用自动显示条目

```json [settings]
{
  "auto_reveal_entries": false
}
```

### 自动折叠目录

- 描述：当目录内仅包含一个子目录时，是否自动折叠该目录。
- 设置项：`auto_fold_dirs`
- 默认值：`true`

**选项**

1. 启用自动折叠目录功能

```json [settings]
{
  "auto_fold_dirs": true
}
```

2. 禁用自动折叠目录功能

```json [settings]
{
  "auto_fold_dirs": false
}
```

### 缩进尺寸

- 描述：嵌套项目的缩进量（以像素为单位）。
- 设置项：`indent_size`
- 默认值：`20`

### 缩进参考线：显示

- 描述：是否在项目面板中显示缩进参考线。
- 设置项：`indent_guides`
- 默认值：

```json [settings]
"indent_guides": {
  "show": "always"
}
```

**选项**

1. 在项目面板中显示缩进参考线

```json [settings]
{
  "indent_guides": {
    "show": "always"
  }
}
```

2. 在项目面板中隐藏缩进参考线

```json [settings]
{
  "indent_guides": {
    "show": "never"
  }
}
```

### 滚动条：显示

- 描述：是否在项目面板中显示滚动条。可选值：null、"auto"、"system"、"always"、"never"。未设置时继承编辑器配置，详见其说明。
- 设置项：`scrollbar`
- 默认值：

```json [settings]
"scrollbar": {
  "show": null
}
```

**选项**

1. 在项目面板中显示滚动条

```json [settings]
{
  "scrollbar": {
    "show": "always"
  }
}
```

2. 在项目面板中隐藏滚动条

```json [settings]
{
  "scrollbar": {
    "show": "never"
  }
}
```

## 智能体

访问 [配置页面](./ai/configuration.md) 了解所有智能体相关设置的详细信息。

## 协作面板

- 描述：协作面板的自定义配置。
- 设置项：`collaboration_panel`
- 默认值：

```json [settings]
{
  "collaboration_panel": {
    "button": true,
    "dock": "left",
    "default_width": 240
  }
}
```

**选项**

- `button`：是否在状态栏显示协作面板按钮
- `dock`：协作面板的停靠位置。可选`left`或`right`
- `default_width`：协作面板的默认宽度

## 调试器

- 描述：调试器面板与设置的配置项
- 设置项：`debugger`
- 默认值：

```json [settings]
{
  "debugger": {
    "stepping_granularity": "line",
    "save_breakpoints": true,
    "dock": "bottom",
    "button": true
  }
}
```

关于Zed编辑器内调试功能的更多信息，请参阅[调试器文档](./debugger.md)。

## Git面板

- 描述：用于自定义Git面板行为的设置项
- 设置项：`git_panel`
- 默认值：

```json [settings]
{
  "git_panel": {
    "button": true,
    "dock": "left",
    "default_width": 360,
    "status_style": "icon",
    "fallback_branch_name": "main",
    "sort_by_path": false,
    "collapse_untracked_diff": false,
    "scrollbar": {
      "show": null
    }
  }
}
```

**选项**

- `button`：是否在状态栏显示Git面板按钮
- `dock`：Git面板的停靠位置。可选`left`或`right`
- `default_width`：Git面板的默认宽度
- `status_style`：Git状态显示方式。可选`label_color`或`icon`
- `fallback_branch_name`：当`init.defaultBranch`未设置时使用的默认分支名称
- `sort_by_path`：是否按路径而非状态（默认）对面板条目进行排序
- `collapse_untracked_diff`：是否在差异面板中折叠未跟踪文件
- `scrollbar`：Git面板中滚动条的显示时机

## 大纲面板

- 描述：自定义大纲面板
- 设置项：`outline_panel`
- 默认值：

```json [settings]
"outline_panel": {
  "button": true,
  "default_width": 300,
  "dock": "left",
  "file_icons": true,
  "folder_icons": true,
  "git_status": true,
  "indent_size": 20,
  "auto_reveal_entries": true,
  "auto_fold_dirs": true,
  "indent_guides": {
    "show": "always"
  },
  "scrollbar": {
    "show": null
  }
}
```

## 通话设置

- 功能说明：自定义参与通话时的行为模式
- 设置项：`calls`
- 默认值：

```json [settings]
"calls": {
  // Join calls with the microphone live by default
  "mute_on_join": false,
  // Share your project when you are the first to join a channel
  "share_on_join": false
},
```

## 无用代码淡化

- 功能说明：对未使用代码的淡化程度控制
- 设置项：`unnecessary_code_fade`
- 默认值：`0.3`

**参数选项**

取值范围为`0.0`至`0.9`的浮点数值，其中：

- `0.0`表示无淡化效果（未使用代码与已使用代码显示效果一致）
- `0.9`表示最大淡化程度（未使用代码呈现极浅色调但仍保持可见）

**应用示例**

```json [settings]
{
  "unnecessary_code_fade": 0.5
}
```

## 界面字体族

- 描述：用于界面文本的字体名称。
- 设置项：`ui_font_family`
- 默认值：`.ZedSans`。当前映射至 [IBM Plex](https://www.ibm.com/plex/)。

**可选值**

系统中安装的任何字体族名称，使用 `".ZedSans"` 可调用 Zed 提供的默认字体，或使用 `".SystemUIFont"` 调用系统默认界面字体（适用于 macOS 和 Windows）。

## 界面字体特性

- 描述：为界面文本启用的 OpenType 字体特性。
- 设置项：`ui_font_features`
- 默认值：

```json [settings]
"ui_font_features": {
  "calt": false
}
```

- 适用平台：macOS 和 Windows。

**可选值**

Zed 支持为指定界面字体启用或禁用所有 OpenType 特性，同时支持设置字体特性数值。

例如，要禁用字体连字功能，请在设置中添加以下内容：

```json [settings]
{
  "ui_font_features": {
    "calt": false
  }
}
```

您还可以设置其他OpenType特性，例如将`cv01`设为`7`：

```json [settings]
{
  "ui_font_features": {
    "cv01": 7
  }
}
```

## 界面字体回退方案

- 说明：用于界面文本的字体回退方案。
- 设置项：`ui_font_fallbacks`
- 默认值：`null`
- 适用平台：macOS 和 Windows。

**选项设置**

例如，要使用`Nerd Font`作为回退字体，请在设置中添加以下内容：

```json [settings]
{
  "ui_font_fallbacks": ["Nerd Font"]
}
```

## 界面字体大小

- 说明：界面文本的默认字体尺寸。
- 设置项：`ui_font_size`
- 默认值：`16`

**可选范围**

`integer`取值范围从`6`到`100`像素（含边界值）

## 界面字体粗细

- 说明：界面文本的默认字体粗细程度。
- 设置项：`ui_font_weight`
- 默认值：`400`

**可选范围**

`integer`取值介于`100`到`900`之间

## 配置示例：

```json [settings]
// ~/.config/zed/settings.json
{
  "theme": "cave-light",
  "tab_size": 2,
  "preferred_line_length": 80,
  "soft_wrap": "none",

  "buffer_font_size": 18,
  "buffer_font_family": ".ZedMono",

  "autosave": "on_focus_change",
  "format_on_save": "off",
  "vim_mode": false,
  "projects_online_by_default": true,
  "terminal": {
    "font_family": "FiraCode Nerd Font Mono",
    "blinking": "off"
  },
  "languages": {
    "C": {
      "format_on_save": "on",
      "formatter": "language_server",
      "preferred_line_length": 64,
      "soft_wrap": "preferred_line_length"
    }
  }
}
```