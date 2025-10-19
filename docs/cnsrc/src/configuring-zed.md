# 配置 Zed

Zed 专为可配置性而设计：我们力求完美契合您的工作流程与个人偏好。我们提供的默认设置旨在为大多数用户打造舒适的入门体验，但更期待您通过个性化调整使其臻于完美。

除本文所述的设置外，您可能还需要更改[主题](./themes.md)、配置[快捷键](./key-bindings.md)、设置[任务](./tasks.md)或安装[扩展插件](https://github.com/zed-industries/extensions)。

## 配置文件

<!--
TBD: Settings files. Rewrite with "remote settings" in mind (e.g. [[CODE_BLOCK_0]] on the remote host).
Consider renaming [[CODE_BLOCK_1]] to [[CODE_BLOCK_2]].

TBD: Add settings documentation about how settings are merged as overlays. E.g. project>本地>默认。请注意映射类设置会合并生效，而数组类设置会被替换且必须包含默认值。
-->

您的设置文件可通过 {#kb zed::OpenSettings} 打开。默认情况下它位于 `~/.config/zed/settings.json`，但如果您在 Linux 环境中设置了 XDG_CONFIG_HOME，则路径会变为 `$XDG_CONFIG_HOME/zed/settings.json`。

该配置会与项目内的所有本地配置合并。您可以通过命令面板执行 {#action zed::OpenProjectSettings} 来打开项目设置。这将创建一个包含 `.zed/settings.json` 的 `.zed` 目录。

虽然大多数项目仅需在根目录存放一个设置文件，但您可以根据需要为子目录添加更多本地设置文件。并非所有设置都能在本地文件中配置，仅限影响编辑器及语言工具行为的设置。例如您可以设置 `tab_size`、`formatter` 等，但不能设置 `theme`、`vim_mode` 及类似配置。

配置文件的语法是支持 `//` 注释的 JSON 超集。

## 默认设置

您可以通过命令面板运行 {#action zed::OpenDefaultSettings} 查看当前 Zed 的默认设置。

提供语言服务器的扩展也可能为这些语言服务器提供默认设置。

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

- 描述：活动窗格周围边框的尺寸。设置为 0 时，活动窗格不显示边框。边框采用内嵌绘制方式。
- 设置项：`border_size`
- 默认值：`0.0`

**可选值**

非负 `float` 数值

### 非活动窗格透明度

- 描述：非活动面板的不透明度。若设为1.0，非活动面板与活动面板具有相同透明度；若设为0，非活动面板内容将完全不可见。数值范围限定在[0.0, 1.0]区间。
- 设置项：`inactive_opacity`
- 默认值：`1.0`

**可选参数**

`float` 数值

## 底部停靠布局

- 描述：控制底部停靠区相对于左右停靠区的布局方式。
- 设置项：`bottom_dock_layout`
- 默认值：`"contained"`

**布局选项**

1. 底部停靠区包含模式：为左右停靠区保留窗口完整高度

```json [settings]
{
  "bottom_dock_layout": "contained"
}
```

2. 底部停靠区全宽模式：占据窗口全部宽度，压缩左右停靠区宽度

```json [settings]
{
  "bottom_dock_layout": "full"
}
```

3. 底部停靠区左对齐模式：压缩左侧停靠区，为右侧停靠区保留窗口完整高度

```json [settings]
{
  "bottom_dock_layout": "left_aligned"
}
```

4. 将底部停靠区右对齐，使左侧停靠区占据窗口完整高度，右侧停靠区将被截断。

```json [settings]
{
  "bottom_dock_layout": "right_aligned"
}
```

## 代理字体大小

- 描述：代理面板中文本的字体大小。未设置时继承界面字体大小。
- 设置项：`agent_font_size`
- 默认值：`null`

**可选范围**

`integer` 取值范围为 `6` 到 `100` 像素（含边界值）

## 允许重新换行

- 描述：控制在当前语言作用域中允许 {#action editor::Rewrap} 操作的位置
- 设置项：`allow_rewrap`
- 默认值：`"in_comments"`

**选项**

1. 仅允许在注释中重新换行：

```json [settings]
{
  "allow_rewrap": "in_comments"
}
```

2. 仅允许在选中内容中重新换行：

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

注意：在 Vim 模式下此设置无效，因为该模式下已允许在任何位置进行重新换行。

## 自动缩进

- 说明：输入时是否应根据上下文调整缩进。此设置可按语言分别指定。
- 配置项：`auto_indent`
- 默认值：`true`

**可选值**

`boolean` 取值

## 粘贴时自动缩进

- 说明：是否应根据上下文调整粘贴内容的缩进
- 配置项：`auto_indent_on_paste`
- 默认值：`true`

**可选值**

`boolean` 取值

## 自动安装扩展

- 说明：定义需要自动安装或禁止安装的扩展
- 配置项：`auto_install_extension`
- 默认值：`{ "html": true }`

**配置方式**

您可通过查看[扩展安装目录](./extensions/installing-extensions.md#installation-location)下的子文件夹名称，获取当前已安装扩展的标识名：

macOS系统：

```sh
ls ~/Library/Application\ Support/Zed/extensions/installed/
```

Linux系统：

```sh
ls ~/.local/share/zed/extensions/installed
```

定义应安装（`true`）或永不安装（`false`）的扩展程序。

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

- 描述：何时自动保存编辑中的缓冲区。
- 设置：`autosave`
- 默认值：`off`

**选项**

1. 禁用自动保存，设置为`off`：

```json [settings]
{
  "autosave": "off"
}
```

2. 焦点变化时自动保存，使用`on_focus_change`：

```json [settings]
{
  "autosave": "on_focus_change"
}
```

3. 活动窗口切换时自动保存，使用`on_window_change`：

```json [settings]
{
  "autosave": "on_window_change"
}
```

4. 无操作间隔后自动保存，使用`after_delay`：

```json [settings]
{
  "autosave": {
    "after_delay": {
      "milliseconds": 1000
    }
  }
}
```

注意：当未保存的标签页关闭时，即使早于配置的无操作间隔时间，也会触发保存操作。

## 点击时自动滚动

- 描述：点击可见文本区域边缘附近时是否滚动。
- 设置：`autoscroll_on_clicks`
- 默认值：`false`

**选项**

`boolean` 值

## 自动签名帮助

- 描述：在括号内时，在编辑器中显示方法签名。
- 设置：`auto_signature_help`
- 默认值：`false`

**选项**

`boolean` 值

### 编辑后显示签名帮助

- 描述：是否在完成或插入括号对后显示签名帮助。如果启用了 `auto_signature_help`，此设置也将被视为启用。
- 设置：`show_signature_help_after_edits`
- 默认值：`false`

**选项**

`boolean` 值

## 自动更新

- 描述：是否自动检查更新。
- 设置：`auto_update`
- 默认值：`true`

**选项**

`boolean` 值

## 基础键位映射

- 描述：基础按键绑定方案。基础键位映射可通过用户自定义键位映射覆盖。
- 设置：`base_keymap`
- 默认值：`VSCode`

**选项**

1. VS Code

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

## 编辑器字体族

- 描述：用于在编辑器中渲染文本的字体名称。
- 设置项：`buffer_font_family`
- 默认值：`.ZedMono`（当前关联至[Lilex](https://lilex.myrt.co)字体）

**可选值**

用户系统已安装的任意字体系列名称，或`".ZedMono"`

## 编辑器字体特性

- 描述：为编辑器文本启用的OpenType特性
- 设置项：`buffer_font_features`
- 默认值：`null`
- 支持平台：macOS与Windows系统

**可选值**

Zed 支持所有 OpenType 字体特性，这些特性可以针对特定缓冲区或终端字体启用或禁用，同时支持设置字体特性的数值。

例如，要禁用字体连字，请在设置中添加以下内容：

```json [settings]
{
  "buffer_font_features": {
    "calt": false
  }
}
```

您还可以设置其他 OpenType 特性，例如将 `cv01` 设置为 `7`：

```json [settings]
{
  "buffer_font_features": {
    "cv01": 7
  }
}
```

## 缓冲区字体回退

- 描述：设置缓冲区文本的字体回退列表，该列表将与平台的默认回退字体合并。
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

- 描述：编辑器中文本的默认字体大小。
- 设置项：`buffer_font_size`
- 默认值：`15`

**选项说明**

字体大小从`6`到`100`像素（含边界值）

## 缓冲区字体粗细

- 描述：编辑器中文本的默认字体粗细。
- 设置：`buffer_font_weight`
- 默认值：`400`

**选项**

`integer` 取值范围在`100`到`900`之间

## 缓冲区行高

- 描述：编辑器中文本的默认行高。
- 设置：`buffer_line_height`
- 默认值：`"comfortable"`

**选项**

`"standard"`、`"comfortable"`或`{ "custom": float }`（`1`为紧凑模式，`2`为宽松模式）

## 居中布局

- 描述：居中布局模式的配置。
- 设置：`centered_layout`
- 默认值：

```json [settings]
"centered_layout": {
  "left_padding": 0.2,
  "right_padding": 0.2,
}
```

**选项**

`left_padding`和`right_padding`选项用于定义启用居中布局模式时，中央窗格相对于工作区的左右内边距宽度。有效取值范围为`0`至`0.4`。

## 文件删除时关闭

- 描述：当磁盘上对应文件被删除时，是否自动关闭编辑器标签页。
- 设置项：[[代码块_0]]
- 默认值：[[代码块_1]]

**选项参数**

[[代码块_2]] 取值说明

启用该设置后，系统将自动关闭已从文件系统中删除的文件标签页。该功能特别适用于需要频繁创建和删除临时文件或草稿文件的工作流程。禁用时（默认设置），即使文件已被删除，其标签页仍会保持打开状态，并在标签标题上显示删除线标识。

注意：即使启用此设置，含未保存更改的脏文件也不会被自动关闭，以确保您不会丢失未保存的工作内容。

## 退出确认

- 描述：关闭应用程序前是否提示用户进行确认。
- 设置项：[[代码块_3]]
- 默认值：[[代码块_4]]

**选项参数**

[[代码块_5]] 取值说明

## 诊断最高级别

- 描述：选择用于过滤编辑器中显示诊断信息的级别
- 设置项：[[代码块_5]]
- 默认值：[[代码块_6]]

**选项**

1. 允许所有诊断信息（默认）：

[[代码块_0]]

2. 仅显示错误：

[[代码块_1]]

3. 显示错误与警告：

[[代码块_2]]

4. 显示错误、警告及信息：

[[代码块_3]]

5. 显示全部（包含提示）：

[[代码块_4]]

## 禁用AI功能

- 描述：是否在Zed中禁用所有AI功能
- 设置项：[[代码块_7]]
- 默认值：[[代码块_8]]

**选项**

[[代码块_9]] 取值

## Direnv集成

- 描述：[direnv](https://direnv.net/) 集成设置。需要安装 `direnv`。
  `direnv` 集成使得可以使用由 `direnv` 配置设置的环境变量来检测 `$PATH` 中的某些语言服务器，而无需安装它们。
  它还允许在任务中使用这些环境变量。
- 设置：`load_direnv`
- 默认值：`"direct"`

**选项**

有两个选项可供选择：

1. `shell_hook`：使用 shell 钩子加载 direnv。这依赖于 direnv 在进入目录时激活。支持 POSIX shell 和 fish。
2. `direct`：使用 `direnv export json` 加载 direnv。这将直接加载 direnv，而不依赖 shell 钩子，但可能会导致一些不一致。这使得 direnv 可以与任何 shell 一起工作。

## 多缓冲区中的双击

- 描述：当多缓冲区中的某些摘录（单例缓冲区片段）被双击时应执行的操作
- 设置项：[[代码块2]]
- 默认值：[[代码块3]]

**选项**

1. 作为常规缓冲区处理，选中整个单词（默认）：

[[代码块0]]

2. 将点击的摘录在新标签页中作为新缓冲区打开：

[[代码块1]]

若选择"打开"选项，可通过双击时按住[[代码块4]]实现常规选择行为。

## 拖放目标区域尺寸

- 描述：编辑器中拖放目标区域的相对尺寸（0-0.5），该区域用于将拖入的文件在分屏窗格中打开。例如：0.25表示若拖放到窗格顶部/底部四分之一区域将创建垂直分屏，拖放到左侧/右侧四分之一区域将创建水平分屏。
- 设置项：[[代码块5]]
- 默认值：[[代码块6]]

**选项**

[[代码块7]] 取值范围在[[代码块8]]至[[代码块9]]之间

## 编辑预测

- 描述：编辑预测相关设置。
- 设置项：[[代码块_1]]
- 默认值：

[[代码块_0]]

**选项配置**

### 禁用文件匹配模式

- 描述：用于禁用编辑预测功能的文件匹配模式列表。该列表将追加至预设的合理默认模式集，您添加的任何新模式都会与之合并生效。
- 设置项：[[代码块_2]]
- 默认值：[[代码块_3]]

**选项说明**

由[[代码块_4]]值组成的列表。

## 禁用编辑预测的范围

- 描述：应禁用编辑预测功能的语言作用域列表。
- 设置项：[[代码块_5]]
- 默认值：[[代码块_6]]

**选项说明**

由[[代码块_7]]值组成的列表

1. 注释中不显示编辑预测：

[[代码块_0]]

2. 不在字符串和注释中显示编辑预测：

[[代码块_1]]

3. 仅在Go语言中，不在字符串和注释中显示编辑预测：

[[代码块_2]]

## 当前行高亮

- 描述：如何在编辑器中高亮显示当前行。
- 设置：[[代码块_7]]
- 默认值：[[代码块_8]]

**选项**

1. 不高亮当前行：

[[代码块_3]]

2. 高亮装订线区域：

[[代码块_4]]

3. 高亮编辑器区域：

[[代码块_5]]

4. 高亮整行：

[[代码块_6]]

## 选中内容高亮

- 描述：是否在编辑器中高亮显示所选文本的所有出现位置。
- 设置：`selection_highlight`
- 默认值：`true`

## 圆角选择

- 描述：文本选择是否应具有圆角。
- 设置：`rounded_selection`
- 默认值：`true`

## 光标闪烁

- 描述：光标是否闪烁。
- 设置：`cursor_blink`
- 默认值：`true`

**选项**

`boolean` 值

## 光标形状

- 描述：默认编辑器的光标形状。
- 设置：`cursor_shape`
- 默认值：`bar`

**选项**

1. 垂直条：

```json [settings]
"cursor_shape": "bar"
```

2. 包围后续字符的块状：

```json [settings]
"cursor_shape": "block"
```

3. 沿后续字符延伸的下划线/底线：

```json [settings]
"cursor_shape": "underline"
```

4. 围绕后续字符绘制的方框：

```json [settings]
"cursor_shape": "hollow"
```

## 边栏

- 描述：编辑器边栏的设置
- 设置项：[[代码块_3]]
- 默认值：

[[代码块_0]]

**选项**

- [[代码块_4]]：是否在边栏显示行号
- [[代码块_5]]：是否在边栏显示运行按钮
- [[代码块_6]]：是否在边栏显示断点标记
- [[代码块_7]]：是否在边栏显示折叠按钮
- [[代码块_8]]：为边栏保留的最小字符空间

## 鼠标隐藏

- 描述：决定在编辑器或输入框内何时隐藏鼠标光标
- 设置项：[[代码块_9]]
- 默认值：[[代码块_10]]

**选项**

1. 从不隐藏鼠标光标：

[[代码块_1]]

2. 仅在输入时隐藏：

[[代码块_2]]

3. 在输入和光标移动时均隐藏：

## 代码片段排序顺序

- 描述：决定代码片段相对于其他补全项的排序方式。
- 设置：`snippet_sort_order`
- 默认值：`inline`

**选项**

1. 将代码片段置于补全列表顶部：

```json [settings]
"snippet_sort_order": "top"
```

2. 正常显示代码片段，不做特殊排序：

```json [settings]
"snippet_sort_order": "inline"
```

3. 将代码片段置于补全列表底部：

```json [settings]
"snippet_sort_order": "bottom"
```

4. 不在补全列表中显示代码片段：

```json [settings]
"snippet_sort_order": "none"
```

## 编辑器滚动条

- 描述：是否显示编辑器滚动条及其中的各种元素。
- 设置：`scrollbar`
- 默认值：

### 显示模式

- 描述：何时显示编辑器滚动条。
- 设置：`show`
- 默认值：`auto`

**选项**

1. 如果有重要信息则显示滚动条，或遵循系统配置的行为：

```json [settings]
"scrollbar": {
  "show": "auto"
}
```

2. 匹配系统配置的行为：

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
- 默认值：`true`

**选项**

`boolean` 值

### Git 差异指示器

- 描述：是否在滚动条中显示 Git 差异指示器。
- 设置：[[代码块_1]]
- 默认值：[[代码块_2]]

**选项**

[[代码块_3]] 值

### 搜索结果指示器

- 描述：是否在滚动条中显示缓冲区搜索结果。
- 设置：[[代码块_4]]
- 默认值：[[代码块_5]]

**选项**

[[代码块_6]] 值

### 选中文本指示器

- 描述：是否在滚动条中显示选中文本的出现位置。
- 设置：[[代码块_7]]
- 默认值：[[代码块_8]]

**选项**

[[代码块_9]] 值

### 选中符号指示器

- 描述：是否在滚动条中显示选中符号的出现位置。
- 设置：[[代码块_10]]
- 默认值：[[代码块_11]]

**选项**

[[代码块_12]] 值

### 诊断信息

- 描述：在滚动条中显示哪些诊断指示器。
- 设置：[[代码块_13]]
- 默认值：[[代码块_14]]

**选项**

1. 显示所有诊断信息：

[[代码块_0]]

2. 不显示任何诊断信息：

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

#### 水平方向

- 描述：设为 false 时，强制禁用水平滚动条。否则遵循其他设置。
- 设置：`horizontal`
- 默认值：`true`

**选项**

`boolean` 值

#### 垂直方向

- 描述：设为 false 时，强制禁用垂直滚动条。否则遵循其他设置。
- 设置：`vertical`
- 默认值：`true`

**选项**

`boolean` 值

## 缩略图

- 描述：与编辑器小地图相关的设置，用于提供文档概览。
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

- 描述：何时在编辑器中显示小地图。
- 设置项：`show`
- 默认值：`never`

**选项**

1. 始终显示小地图：

```json [settings]
{
  "show": "always"
}
```

2. 仅当编辑器滚动条可见时显示小地图：

```json [settings]
{
  "show": "auto"
}
```

3. 从不显示小地图：

```json [settings]
{
  "show": "never"
}
```

### 缩略图显示

- 描述：何时在小地图中显示可视区域指示器（当前编辑器可见区域的缩略图）。
- 设置项：`thumb`
- 默认值：`always`

**选项**

1. 悬停在小地图时显示缩略图：

```json [settings]
{
  "thumb": "hover"
}
```

2. 始终显示缩略图：

### 缩略图边框

- 描述：缩略图边框的显示样式。
- 设置项：`thumb_border`
- 默认值：`left_open`

**可选方案**

1. 缩略图四周均显示边框：

```json [settings]
{
  "thumb_border": "full"
}
```

2. 除左侧外其余三边显示边框：

```json [settings]
{
  "thumb_border": "left_open"
}
```

3. 除右侧外其余三边显示边框：

```json [settings]
{
  "thumb_border": "right_open"
}
```

4. 仅左侧显示边框：

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

- 描述：在小地图中高亮当前行的方式。
- 设置项：`current_line_highlight`
- 默认值：`null`

**可选方案**

1. 继承编辑器的当前行高亮设置：

[[代码块0]]

2. 在迷你地图中高亮当前行：

[[代码块1]]

或

[[代码块2]]

3. 不在迷你地图中高亮当前行：

[[代码块3]]

或

[[代码块4]]

## 编辑器标签栏

- 描述：与编辑器标签栏相关的设置。
- 设置项：[[代码块6]]
- 默认值：

[[代码块5]]

### 显示设置

- 描述：是否在编辑器中显示标签栏。
- 设置项：[[代码块7]]
- 默认值：[[代码块8]]

**可选值**

[[代码块9]] 取值

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

- 描述：是否在标签页显示文件图标。
- 设置：[[代码块_4]]
- 默认值：[[代码块_5]]

### Git状态显示

- 描述：是否在标签页中显示Git文件状态。
- 设置：[[代码块_6]]
- 默认值：[[代码块_7]]

### 关闭时激活规则

- 描述：关闭当前标签页后的操作设置。
- 设置：[[代码块_8]]
- 默认值：[[代码块_9]]

**选项**

1. 激活之前打开的标签页：

[[代码块_0]]

2. 若存在则激活右侧相邻标签页：

[[代码块_1]]

3. 若存在则激活左侧相邻标签页：

[[代码块_2]]

### 关闭按钮显示

- 描述：控制标签页关闭按钮的显示行为。
- 设置：[[代码块_10]]
- 默认值：[[代码块_11]]

**选项**

1. 仅在悬停时显示关闭按钮：

[[代码块_3]]

2. 始终显示关闭按钮：

3. 即使悬停也不显示：

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

`boolean` 值

### 拖放选择

- 描述：是否允许在缓冲区中拖放文本选择。`delay` 表示允许拖放操作前必须经过的毫秒数，否则将创建新的文本选区。
- 设置：`drag_and_drop_selection`
- 默认值：

```json [settings]
"drag_and_drop_selection": {
  "enabled": true,
  "delay": 300
}
```

## 编辑器工具栏

- 描述：是否在编辑器工具栏中显示各类元素。
- 设置：`toolbar`
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

每个选项控制特定工具栏元素的显示。若所有元素均被隐藏，则编辑器工具栏将不会显示。

## 使用系统标签页

- 描述：是否允许窗口根据用户的标签页偏好进行合并（仅限 macOS 系统）。
- 设置：`use_system_window_tabs`
- 默认值：`false`

**选项说明**

此设置启用与 macOS 原生窗口标签页功能的集成。当设置为 `true` 时，Zed 窗口可以按照用户设定的系统级标签页偏好（如“始终”、“全屏时”或“从不”）分组到单个 macOS 窗口的标签页中。此设置仅在 macOS 上可用。

## 启用语言服务器

- 描述：是否使用语言服务器提供代码智能提示。
- 设置：`enable_language_server`
- 默认值：`true`

**选项**

`boolean` 值

## 保存时确保末尾换行

- 描述：移除文件末尾所有仅包含空白字符的行，并确保末尾仅有一个换行符。
- 设置：`ensure_final_newline_on_save`
- 默认值：`true`

**选项**

`boolean` 值

## 展开摘录行数

- 描述：在多缓冲区中展开摘录的默认行数
- 设置：`expand_excerpt_lines`
- 默认值：`5`

**选项**

正 `integer` 值

## 摘录上下文行数

- 描述：在多缓冲区中显示摘录时，提供的上下文行数。
- 设置：[[代码块_1]]
- 默认值：[[代码块_2]]

**选项**

介于1到32之间的正[[代码块_3]]整数值。超出此范围的值将被限制在此范围内。

## 在新行上扩展注释

- 描述：当上一行也是注释时，是否在新行开始处添加注释。
- 设置：[[代码块_4]]
- 默认值：[[代码块_5]]

**选项**

[[代码块_6]]值

## 状态栏

- 描述：控制状态栏中的各种元素。请注意，状态栏中的某些项目在其他地方有各自的设置。
- 设置：[[代码块_7]]
- 默认值：

[[代码块_0]]

有一个实验性设置可以完全隐藏状态栏。这会导致严重的可用性问题（您将无法使用 Zed 的许多功能），但为那些最看重屏幕空间的人提供了这个选项。

```json
"status_bar": {
  "experimental.show": false
}
```

## 语言服务器协议

- 描述：语言服务器的配置。
- 设置：`lsp`
- 默认值：`null`

**选项**

以下设置可以针对特定语言服务器进行覆盖：

- `initialization_options`
- `settings`

要覆盖某个语言服务器的配置，请在该语言服务器名称下添加一个条目到 `lsp` 值中。

某些选项通过 `initialization_options` 传递给语言服务器。这些选项必须在语言服务器启动时指定，更改后需要重新启动语言服务器。

例如，要将 `check` 选项传递给 `rust-analyzer`，请使用以下配置：

虽然其他选项可以在运行时更改，并应放置在 `settings` 下：

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

## 全局 LSP 设置

- 描述：适用于所有语言服务器的全局 LSP 配置
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

- `button`：是否在状态栏中显示 LSP 状态按钮

## LSP 高亮防抖

- 描述：基于当前光标位置从语言服务器查询高亮内容前的防抖延迟时间（毫秒）。
- 设置项：[[代码块_3]]
- 默认值：[[代码块_4]]

**可选值**

[[代码块_5]] 表示毫秒的数值

## 功能特性

- 描述：可全局启用或禁用的功能
- 设置项：[[代码块_6]]
- 默认值：

[[代码块_0]]

### 编辑预测提供程序

- 描述：选择使用的编辑预测提供程序
- 设置项：[[代码块_7]]
- 默认值：[[代码块_8]]

**可选方案**

1. 使用 Zeta 作为编辑预测提供程序：

[[代码块_1]]

2. 使用 Copilot 作为编辑预测提供程序：

[[代码块_2]]

3. 使用 Supermaven 作为编辑预测提供程序：

4. 关闭所有提供程序的编辑预测功能

## 保存时格式化

- 描述：是否在保存前执行缓冲区格式化。
- 设置：editor.formatOnSave
- 默认值：false

**选项**

1. true，启用保存时格式化并遵循editor.formatOnSaveMode设置：

{
  "editor.formatOnSave": true
}

2. false，禁用保存时格式化：

{
  "editor.formatOnSave": false
}

## 格式化程序

- 描述：如何执行缓冲区格式化。
- 设置：editor.defaultFormatter
- 默认值：null

**选项**

1. 要使用当前语言服务器，请设置：

{
  "editor.defaultFormatter": "esbenp.prettier-vscode"
}

2. 或者使用外部命令，请执行 `"external"`。指定要运行的格式化程序名称，以及传递给该程序的参数数组。缓冲区文本将通过标准输入传递给程序，格式化后的输出应写入标准输出。例如，以下命令将使用 [`sed(1)`](https://linux.die.net/man/1/sed) 去除行尾空格：

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

3. 外部格式化程序可选择包含 `{buffer_path}` 占位符，运行时该占位符将填入被格式化缓冲区的路径。格式化程序通过标准输入接收文件内容，重新格式化后输出至标准输出，因此通常无法获知所格式化文件的名称。像 Prettier 这样的工具支持通过命令行参数接收文件路径，该参数可用于影响格式化决策。

警告：`{buffer_path}` 不应被用于指示格式化程序从文件名读取内容。您的格式化程序应仅从标准输入读取数据，且不应直接读取或写入文件。

```json [settings]
  "formatter": {
    "external": {
      "command": "prettier",
      "arguments": ["--stdin-filepath", "{buffer_path}"]
    }
  }
```

4. 若要使用连接的语言服务器提供的代码操作，请使用 `"code_actions"`：

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

首先将使用 `rust-analyzer` 格式化代码，随后调用 sed 命令。
若任一格式化程序执行失败，后续程序仍会继续运行。

## 自动闭合

- 功能说明：输入左括号、方括号、花括号、单引号或双引号时，是否自动添加对应的闭合字符。
- 设置项：`use_autoclose`
- 默认值：`true`

**选项参数**

`boolean` 取值列表

## 始终将括号视为自动闭合

- 功能说明：控制编辑器处理自动闭合字符的方式。
- 设置项：`always_treat_brackets_as_autoclosed`
- 默认值：`false`

**选项参数**

`boolean` 取值列表

**示例说明**

若设置项配置为 `true`：

1. 在编辑器中输入：`)))`
2. 将光标移至起始位置：`^)))`
3. 再次输入：`)))`

最终结果仍保持为 `)))`，而不会变成默认情况下的 `))))))`。

## 文件扫描排除项

- 设置项：`file_scan_exclusions`
- 描述：Zed 将完全忽略的文件或文件通配符。这些文件在文件扫描、文件搜索过程中会被跳过，且不会显示在项目文件树中。此设置优先级高于 `file_scan_inclusions`。
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

请注意，在 settings.json 中指定 `file_scan_exclusions` 将覆盖上述默认值。如需排除额外文件，您需要在设置中包含所有默认值。

## 文件扫描包含项

- 设置项：`file_scan_inclusions`
- 说明：即使被 git 忽略，仍会被 Zed 包含的文件或文件通配模式。适用于未被 git 跟踪但对项目仍重要的文件。注意过于宽泛的通配模式可能会降低 Zed 的文件扫描速度。`file_scan_exclusions` 的优先级高于这些包含规则。
- 默认值：

```json [settings]
"file_scan_inclusions": [".env*"],
```

## 文件类型

- 设置项：`file_types`
- 说明：配置 Zed 如何根据文件名或扩展名选择对应语言。支持通配符条目。
- 默认值：

```json [settings]
"file_types": {
  "JSONC": ["**/.zed/**/*.json", "**/zed/**/*.json", "**/Zed/**/*.json", "**/.vscode/**/*.json"],
  "Shell Script": [".env.*"]
}
```

**示例**

若要将所有 `.c` 文件识别为 C++ 语言，将名为 `MyLockFile` 的文件识别为 TOML 语言，将以 `Dockerfile` 开头的文件识别为 Dockerfile 语言：

## 诊断设置

- 描述：诊断相关功能的配置。
- 设置项：`diagnostics`
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
- 设置项：`inline`
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

2. 延迟诊断更新，在上次诊断更新后一段时间再进行更新。

3. 设置源代码行尾与诊断信息起始位置之间的间距。

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

4. 在指定列水平对齐内联诊断信息。

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

5. 仅显示警告和错误诊断信息。

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

- 描述：Git相关功能的配置。
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

### Git 侧边栏标记

- 描述：是否显示 Git 侧边栏标记。
- 设置项：[[代码块_4]]
- 默认值：[[代码块_5]]

**选项**

1. 在已跟踪文件中显示 Git 侧边栏标记

[[代码块_0]]

2. 隐藏 Git 侧边栏标记

[[代码块_1]]

### 标记更新防抖延迟

- 描述：设置防抖阈值（以毫秒为单位），超过该时间后 Git 侧边栏标记将反映更改。
- 设置项：[[代码块_6]]
- 默认值：[[代码块_7]]

**选项**

[[代码块_8]] 代表毫秒的数值

示例：

[[代码块_2]]

### 行内 Git 追溯

- 描述：是否在当前聚焦行显示行内 Git 追溯信息。
- 设置项：[[代码块_9]]
- 默认值：

[[代码块_3]]

**选项**

1. 禁用行内 Git 追溯：

[[代码块0]]

2. 仅在延迟后显示行内 Git 追溯信息（延迟从光标停止移动时开始计算）：

[[代码块1]]

3. 在提交日期和作者旁显示提交摘要：

[[代码块2]]

4. 将此设置为显示行内追溯信息的最小列位置：

[[代码块3]]

5. 设置行尾与行内追溯提示之间的内边距，以 em 为单位：

[[代码块4]]

### 分支选择器

- 描述：与分支选择器相关的配置。
- 设置：[[代码块5]]
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

### 差异块样式

- 描述：我们应该为差异块使用何种样式。
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

1. 显示已暂存的差异块为淡出并带边框：

```json [settings]
{
  "git": {
    "hunk_style": "staged_hollow"
  }
}
```

2. 显示未暂存的差异块为淡出并带边框：

```json [settings]
{
  "git": {
    "hunk_style": "unstaged_hollow"
  }
}
```

## 转到定义备选方案

- 描述：当 {#action editor::GoToDefinition} 操作未能找到定义时，应执行什么操作
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
- 默认：`false`

**选项**

`boolean` 值

## Helix 模式

- 描述：是否启用 Helix 模式。启用 `helix_mode` 也会同时启用 `vim_mode`。更多详情请参阅 [Helix 文档](./helix.md)。
- 设置：`helix_mode`
- 默认：`false`

**选项**

`boolean` 值

## 缩进参考线

- 描述：与缩进参考线相关的配置。缩进参考线可以为每种语言单独配置。
- 设置：`indent_guides`
- 默认：

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

3. 启用智能缩进色彩标识（彩虹缩进）
   不同缩进层级使用的颜色由主题定义（主题键：`accents`），可通过主题覆盖进行自定义

```json [settings]
{
  "indent_guides": {
    "enabled": true,
    "coloring": "indent_aware"
  }
}
```

4. 启用智能缩进背景色标识（彩虹缩进）
   不同缩进层级使用的颜色由主题定义（主题键：`accents`），可通过主题覆盖进行自定义

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

- 描述：在编辑器中鼠标悬停在符号上时是否显示信息提示框。
- 设置项：[[代码块_1]]
- 默认值：[[代码块_2]]

**选项**

[[代码块_3]] 值

## 悬停弹窗延迟

- 描述：显示信息提示框前等待的毫秒数。
- 设置项：[[代码块_4]]
- 默认值：[[代码块_5]]

**选项**

[[代码块_6]] 代表毫秒的数值

## 图标主题

- 描述：图标主题设置可通过两种形式指定：直接使用图标主题名称，或使用包含Zed内部文件/文件夹的[[代码块_7]]、[[代码块_8]]和[[代码块_9]]图标主题的对象。
- 设置项：[[代码块_10]]
- 默认值：[[代码块_11]]

### 图标主题对象

- 描述：使用包含[[代码块_12]]、[[代码块_13]]和[[代码块_14]]的对象来指定图标主题。
- 设置项：[[代码块_15]]
- 默认值：

[[代码块_0]]

### 模式

- 描述：指定图标主题模式。
- 设置：[[代码块_3]]
- 默认：[[代码块_4]]

**选项**

1. 将图标主题设为深色模式

[[代码块_0]]

2. 将图标主题设为浅色模式

[[代码块_1]]

3. 将图标主题设为系统模式

[[代码块_2]]

### 深色

- 描述：深色图标主题的名称。
- 设置：[[代码块_5]]
- 默认：[[代码块_6]]

**选项**

在命令面板中运行 {#操作 图标主题选择器::切换} 操作，查看当前有效的图标主题名称列表。

### 浅色

- 描述：浅色图标主题的名称。
- 设置：[[代码块_7]]
- 默认：[[代码块_8]]

**选项**

在命令面板中运行 {#操作 图标主题选择器::切换} 操作，查看当前有效的图标主题名称列表。

## 图片查看器

- 描述：图片查看器功能设置
- 设置：[[代码块_9]]
- 默认：

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

- 描述：在编辑器中显示带提示的额外文本的配置。
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

内联提示查询由两部分组成：编辑器（客户端）和 LSP 服务器。
当启用上述内联提示设置后，编辑器将开始查询特定类型的提示，并响应服务器发出的 LSP 提示刷新请求。
此时，服务器根据其实现可能返回提示也可能不返回，可能需要进一步配置，请参阅相应的 LSP 服务器文档。

以下语言已由 Zed 预配置内联提示：

- [Go](https://docs.zed.dev/languages/go)
- [Rust](https://docs.zed.dev/languages/rust)
- [Svelte](https://docs.zed.dev/languages/svelte)
- [TypeScript](https://docs.zed.dev/languages/typescript)

使用 `lsp` 章节进行服务器配置。相应语言文档中提供了配置示例。

Zed 不会立即查询提示，系统采用两种防抖机制，任一种均可设置为 0 来禁用该机制。
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

未指定的值默认为 `false`，若所有修饰键均为 `false` 或未完全按下所有修饰键，提示将不会切换。

## 日志

- 描述：日志相关配置。
- 设置项：`journal`
- 默认值：

```json [settings]
"journal": {
  "path": "~",
  "hour_format": "hour12"
}
```

### 路径

- 描述：日志条目存储目录的路径。
- 设置项：`path`
- 默认值：`~`

**选项**

`string` 值

### 时间格式

- 描述：日志中显示小时数的格式。
- 设置项：`hour_format`
- 默认值：`hour12`

**选项**

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
- 设置项：[[代码块_2]]
- 默认值：

[[代码块_0]]

**选项配置**

- [[代码块_3]]：是否启用 JSX 标签自动闭合功能

## 语言配置

- 描述：针对特定语言的配置选项
- 设置项：[[代码块_4]]
- 默认值：[[代码块_5]]

**配置说明**

如需为特定语言覆盖默认设置，请在[[代码块_6]]配置值中新增对应语言条目。配置示例：

[[代码块_1]]

以下设置项支持针对不同语言进行独立配置：

- [[[代码块0]]](#启用语言服务器)
- [[[代码块1]]](#保存时确保末尾换行)
- [[[代码块2]]](#保存时格式化)
- [[[代码块3]]](#格式化工具)
- [[[代码块4]]](#硬制表符)
- [[[代码块5]]](#首选行长度)
- [[[代码块6]]](#保存时移除尾部空白)
- [[[代码块7]]](#显示编辑预测)
- [[[代码块8]]](#显示空白字符)
- [[[代码块9]]](#空白字符映射)
- [[[代码块10]]](#软换行)
- [[[代码块11]]](#制表符大小)
- [[[代码块12]]](#使用自动闭合)
- [[[代码块13]]](#始终将括号视为自动闭合)

这些选项采用与根级别同名设置相同的配置项。

## 语言模型

- 描述：语言模型提供商的配置
- 设置：[[代码块14]]
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

**选项**

配置各种AI模型提供商，包括API网址和认证设置。

## 行指示器格式

- 描述：状态栏中行指示器的格式
- 设置：`line_indicator_format`
- 默认：`"short"`

**选项**

1. 短格式：

```json [settings]
{
  "line_indicator_format": "short"
}
```

2. 长格式：

```json [settings]
{
  "line_indicator_format": "long"
}
```

## 链接编辑

- 描述：是否对关联范围进行联动编辑，前提是语言服务器支持此功能。例如，在编辑起始`<html>`标签时，闭合`</html>`标签的内容也会同步修改。
- 设置：`linked_edits`
- 默认值：`true`

**选项**

`boolean` 值

## LSP 文档颜色

- 描述：是否显示来自语言服务器的文档颜色信息
- 设置：`lsp_document_colors`
- 默认值：`true`

**选项**

`boolean` 值

## 最大标签页数

- 描述：标签栏中显示的最大标签页数量
- 设置：`max_tabs`
- 默认值：`null`

**选项**

正`integer`值或`null`表示无限制标签页

## 中键粘贴（仅限 Linux）

- 描述：在 Linux 系统上启用中键粘贴功能
- 设置：`middle_click_paste`
- 默认值：`true`

**选项**

`boolean` 值

## 多光标修饰键

- 描述：确定用于通过鼠标添加多个光标的修饰键。开放悬停链接鼠标手势将相应调整，以避免与多光标修饰键冲突。
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

- 描述：Node.js集成配置
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

- `ignore_system_version`：是否忽略系统Node.js版本
- `path`：自定义Node.js二进制文件路径
- `npm_path`：自定义npm二进制文件路径

## 网络代理

- 描述：为 Zed 配置网络代理。
- 设置：[[代码块_2]]
- 默认值：[[代码块_3]]

**选项**

代理设置必须包含代理的 URL。

支持以下 URI 方案：

- [[代码块_4]]
- [[代码块_5]]
- [[代码块_6]] - 使用本地 DNS 的 SOCKS4 代理
- [[代码块_7]] - 使用远程 DNS 的 SOCKS4 代理
- [[代码块_8]] - 使用本地 DNS 的 SOCKS5 代理
- [[代码块_9]] - 使用远程 DNS 的 SOCKS5 代理

未指定方案时，将使用 [[代码块_10]]。

默认情况下，不使用代理，或 Zed 会尝试从环境变量中获取代理设置，例如 [[代码块_11]]、[[代码块_12]]、[[代码块_13]]、[[代码块_14]]、[[代码块_15]]、[[代码块_16]]、[[代码块_17]] 和 [[代码块_18]]。

例如，要设置 [[代码块_19]] 代理，请将以下内容添加到您的设置中：

[[代码块_0]]

或设置 [[代码块_20]] 代理：

[[代码块_1]]

若需排除特定主机不使用代理，请设置 `NO_PROXY` 环境变量。该变量接收以逗号分隔的主机名列表、主机后缀、IPv4/IPv6地址或网段，这些目标将不经过代理。例如，若您的环境包含 `NO_PROXY="google.com, 192.168.1.0/24"`，则 `192.168.1.*`、`google.com` 和 `*.google.com` 内的所有主机都将绕过代理。更多细节请参阅 [reqwest NoProxy 文档](https://docs.rs/reqwest/latest/reqwest/struct.NoProxy.html#method.from_string)。

## 最后窗口关闭时

- 功能说明：最后一个窗口关闭时的操作设置
- 配置项：`on_last_window_closed`
- 默认值：`"platform_default"`

**选项说明**

1. 采用平台默认行为：

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

## 配置档案

- 描述：可在现有设置基础上应用的配置档案
- 设置项：[[代码块1]]
- 默认值：[[代码块2]]

**选项**

用于定义设置档案的配置对象。示例：

[[代码块0]]

## 预览标签页

- 描述：
  预览标签页允许您以预览模式打开文件，当您切换到其他文件时，除非明确固定，否则这些文件会自动关闭。这有助于快速查看文件而不会使工作区杂乱无章。预览标签页中的文件名会以斜体显示。\
  可通过以下方式将预览标签页转换为常规标签页：

  - 双击文件
  - 双击标签页标题
  - 使用 {#action project_panel::OpenPermanent} 操作
  - 编辑文件
  - 将文件拖拽至其他窗格

- 设置项：[[代码块3]]
- 默认值：

### 从文件查找器启用预览

- 描述：决定从文件查找器中选择文件时是否以预览模式打开文件。
- 设置：`workbench.list.openMode`
- 默认值：`singleClick`

**选项**

`singleClick` 和 `doubleClick` 值

### 从代码导航启用预览

- 描述：决定在使用代码导航离开标签页时，预览标签页是否会被替换。
- 设置：`workbench.editor.enablePreviewFromCodeNavigation`
- 默认值：`true`

**选项**

`true` 和 `false` 值

## 文件查找器

### 文件图标

- 描述：是否在文件查找器中显示文件图标。
- 设置：`workbench.list.fileIcons`
- 默认值：`true`

### 模态框最大宽度

- 描述：文件查找器模态框的最大宽度。可以取以下值之一：`small`、`medium`、`large`、`xlarge` 和 `full`。
- 设置：`workbench.list.modalMaxWidth`
- 默认值：`medium`

### 搜索时跳过活动文件焦点

- 描述：决定文件查找器是否应在搜索结果中跳过活动文件的焦点。
- 设置：`skip_focus_for_active_in_search`
- 默认值：`true`

## 水平窗格分割方向

- 描述：水平分割窗格的方向
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

- 描述：垂直分割窗格的方向
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

- 描述：在启用软换行的缓冲区中，进行软换行的列位置。
- 设置：[[代码块_0]]
- 默认值：[[代码块_1]]

**选项**

[[代码块_2]] 值

## 私有文件

- 描述：用于匹配文件路径的通配符，以确定文件是否为私有文件
- 设置：[[代码块_3]]
- 默认值：[[代码块_4]]

**选项**

[[代码块_5]] 通配符模式列表

## 默认在线项目

- 描述：是否默认显示在线项目视图。
- 设置：[[代码块_6]]
- 默认值：[[代码块_7]]

**选项**

[[代码块_8]] 值

## 读取SSH配置

- 描述：是否读取SSH配置文件
- 设置：[[代码块_9]]
- 默认值：[[代码块_10]]

**选项**

[[代码块_11]] 值

## 隐藏私有值

- 描述：在私有文件中隐藏变量的可视显示值
- 设置：[[代码块_12]]
- 默认值：[[代码块_13]]

**选项**

[[代码块_14]] 值

## 相对行号

- 描述：是否在侧边栏显示相对行号
- 设置项：`relative_line_numbers`
- 默认值：`false`

**可选值**

`boolean` 取值

## 保存时移除行尾空格

- 描述：在保存缓冲区前是否移除所有行尾空格
- 设置项：`remove_trailing_whitespace_on_save`
- 默认值：`true`

**可选值**

`boolean` 取值

## 停靠栏内面板统一调整尺寸

- 描述：调整停靠栏尺寸时是否同步调整其内部所有面板尺寸。可组合使用"left"、"right"和"bottom"参数
- 设置项：`resize_all_panels_in_dock`
- 默认值：`["left"]`

**可选值**

字符串列表，可包含以下任意组合：

- `"left"`：同步调整左侧停靠栏面板
- `"right"`：同步调整右侧停靠栏面板
- `"bottom"`：同步调整底部停靠栏面板

## 文件重开时恢复

- 描述：再次打开文件时是否尝试恢复其之前的状态。状态按窗格存储。
- 设置：[[代码块_4]]
- 默认值：[[代码块_5]]

**选项**

[[代码块_6]] 取值

## 启动时恢复

- 描述：控制启动时的会话恢复行为。
- 设置：[[代码块_7]]
- 默认值：[[代码块_8]]

**选项**

1. 恢复退出 Zed 时打开的所有工作区：

[[代码块_0]]

2. 恢复最后关闭的工作区：

[[代码块_1]]

3. 始终以空编辑器启动：

[[代码块_2]]

## 滚动超出末行

- 描述：编辑器是否允许滚动超出最后一行
- 设置：[[代码块_9]]
- 默认值：[[代码块_10]]

**选项**

1. 允许滚动超出末行一页：

[[代码块_3]]

2. 编辑器将在最后一行之后滚动，滚动行数与 `vertical_scroll_margin` 相同：

```json [settings]
{
  "scroll_beyond_last_line": "vertical_scroll_margin"
}
```

3. 编辑器不会在最后一行之后滚动：

```json [settings]
{
  "scroll_beyond_last_line": "off"
}
```

**选项**

`boolean` 值

## 滚动灵敏度

- 描述：滚动灵敏度倍数。此倍数在滚动时同时应用于水平和垂直方向的增量值。
- 设置：`scroll_sensitivity`
- 默认值：`1.0`

**选项**

正 `float` 值

### 快速滚动灵敏度

- 描述：快速滚动的灵敏度倍数。此倍数在滚动时同时应用于水平和垂直方向的增量值。当用户在滚动时按住 Alt 或 Option 键时，会触发快速滚动。
- 设置：`fast_scroll_sensitivity`
- 默认值：`4.0`

**选项**

正 `float` 值

### 水平滚动边距

- 描述：使用鼠标滚动时，在光标两侧保留的字符数量
- 设置项：[[代码块1]]
- 默认值：[[代码块2]]

**可选范围**

非负[[代码块3]]数值

### 垂直滚动边距

- 描述：使用键盘滚动时，在光标上下方保留的行数
- 设置项：[[代码块4]]
- 默认值：[[代码块5]]

**可选范围**

非负[[代码块6]]数值

## 搜索设置

- 描述：打开新项目和缓冲区搜索时默认启用的搜索选项
- 设置项：[[代码块7]]
- 默认值：

[[代码块0]]

## 搜索循环

- 描述：若禁用[[代码块8]]，搜索结果到达文件末尾时不会循环继续
- 设置项：[[代码块9]]
- 默认值：[[代码块10]]

## 从光标位置初始化搜索词

- 描述：何时根据光标下的文本填充新搜索的查询。
- 设置：`seed_search_query_from_cursor`
- 默认：`always`

**选项**

1. `always` 始终使用光标下的单词填充搜索查询
2. `selection` 仅当有文本被选中时填充搜索查询
3. `never` 从不填充搜索查询

## 使用智能大小写搜索

- 描述：启用后，根据查询自动调整搜索的大小写敏感性。如果搜索查询包含任何大写字母，则搜索变为区分大小写；如果仅包含小写字母，则搜索变为不区分大小写。\
  这适用于文件内搜索和项目范围搜索。
- 设置：`use_smartcase_search`
- 默认：`false`

**选项**

`boolean` 值

示例：

- 搜索 "function" 将匹配 "function"、"Function"、"FUNCTION" 等。
- 搜索 "Function" 将仅匹配 "Function"，不匹配 "function" 或 "FUNCTION"

## 显示调用状态图标

- 描述：是否在状态栏显示通话状态图标。
- 设置：[[代码块1]]
- 默认值：[[代码块2]]

**选项**

[[代码块3]] 值

## 自动补全

- 描述：控制此语言的自动补全处理方式。
- 设置：[[代码块4]]
- 默认值：

[[代码块0]]

### 单词补全

- 描述：控制单词补全方式。对于大型文档，可能无法获取所有单词进行补全。
- 设置：[[代码块5]]
- 默认值：[[代码块6]]

**选项**

1. [[代码块7]] - 始终获取文档单词进行补全，同时提供LSP补全
2. [[代码块8]] - 仅当LSP响应错误或超时时，使用文档单词显示补全
3. [[代码块9]] - 从不获取或补全文档单词（基于单词的补全仍可通过单独操作查询）

### 最小单词查询长度

- 说明：自动触发基于单词的补全所需的最小字符数。
  低于该数值时，仍可通过对应的编辑器命令手动触发基于单词的补全。
- 设置项：`words_min_length`
- 默认值：`3`

**可选值**

正整数

### 语言服务器协议

- 说明：是否启用语言服务器协议补全功能。
- 设置项：`lsp`
- 默认值：`true`

**可选值**

`boolean` 值

### LSP请求超时时长（毫秒）

- 说明：获取LSP补全时，设定等待特定服务器响应的最长时间。设为0时表示无限等待。
- 设置项：`lsp_fetch_timeout_ms`
- 默认值：`0`

**可选值**

`integer` 代表毫秒的数值

### LSP插入模式

- 说明：控制接受LSP补全时被替换的文本范围。
- 设置项：`lsp_insert_mode`
- 默认值：`replace_suffix`

**可选值**

1. `insert` - 使用LSP规范中描述的`insert`范围替换光标前的文本
2. `replace` - 使用LSP规范中描述的`replace`范围替换光标前后的文本
3. `replace_subsequence` - 当被替换文本是补全文本的子序列时，行为类似`"replace"`，否则类似`"insert"`
4. `replace_suffix` - 当光标后文本是补全内容的后缀时，行为类似`"replace"`，否则类似`"insert"`

## 输入时显示补全建议

- 描述：是否在输入时显示补全建议
- 设置项：`show_completions_on_input`
- 默认值：`true`

**选项**

`boolean` 取值

## 显示补全文档

- 描述：是否在补全菜单中显示内联及并排文档说明
- 设置项：`show_completion_documentation`
- 默认值：`true`

**选项**

`boolean` 取值

## 显示编辑预测

- 描述：是否在输入时自动显示编辑预测，或通过触发`editor::ShowEditPrediction`手动显示。
- 设置：`show_edit_predictions`
- 默认值：`true`

**选项**

`boolean` 值

## 显示空白字符

- 描述：是否在编辑器中渲染空白字符。
- 设置：`show_whitespaces`
- 默认值：`selection`

**选项**

1. `all`
2. `selection`
3. `none`
4. `boundary`

## 空白字符映射

- 描述：当启用显示空白字符时，指定用于渲染空白字符的符号。
- 设置：`whitespace_map`
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

- 描述：是否自动换行以适应编辑器或首选宽度。
- 设置：`soft_wrap`
- 默认值：`none`

**选项**

1. `none` 通常避免换行，除非行过长
2. `prefer_line`（已弃用，等同于 `none`）
3. `editor_width` 对超出编辑器宽度的行进行换行
4. `preferred_line_length` 对超出 `preferred_line_length` 配置值的行进行换行
5. `bounded` 在 `editor_width` 与 `preferred_line_length` 的最小值处换行

## 显示换行参考线

- 说明：是否在编辑器中显示换行参考线（垂直标尺）。设为 true 时，若 'soft_wrap' 设置为 'preferred_line_length'，将在 'preferred_line_length' 值处显示参考线，并同时显示 'wrap_guides' 设置中指定的其他参考线。
- 设置项：`show_wrap_guides`
- 默认值：`true`

**选项**

`boolean` 值

## 启用输入时格式化

- 说明：是否在每次输入由 LSP 服务器能力定义的“触发”符号后，使用额外的 LSP 查询来格式化（及修正）代码
- 设置项：`use_on_type_format`
- 默认值：`true`

**选项**

`boolean` 值

## 自动环绕功能

- 功能说明：在输入左圆括号、方括号、花括号、单引号或双引号时，是否自动环绕所选文本。例如，当您选中文本并输入“（”时，Zed 将用括号将文本环绕起来。
- 设置项：`use_auto_surround`
- 默认值：`true`

**可选值**

`boolean` 取值

## 使用系统路径提示

- 功能说明：是否在打开和另存为操作时使用系统提供的对话框。若设为 false，Zed 将使用内置的键盘优先选择器。
- 设置项：`use_system_path_prompts`
- 默认值：`true`

**可选值**

`boolean` 取值

## 使用系统提示框

- 功能说明：是否在提示操作（如确认提示）时使用系统提供的对话框。若设为 false，Zed 将使用内置提示框。注意：在 Linux 系统上此选项将被忽略，Zed 始终使用内置提示框。
- 设置项：`use_system_prompts`
- 默认值：`true`

**可选值**

`boolean` 取值

## 换行参考线（垂直标尺）

- 描述：在何处显示垂直标尺作为换行参考线。通过将`show_wrap_guides`设置为`false`来禁用此功能。
- 设置：`wrap_guides`
- 默认值：[]

**选项**

`integer`列编号列表

## 制表符大小

- 描述：每个制表符所使用的空格数量。
- 设置：`tab_size`
- 默认值：`4`

**选项**

`integer`数值

## 任务

- 描述：可在Zed内部运行的任务配置
- 设置：`tasks`
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

**选项**

- `variables`：任务配置的自定义变量
- `enabled`：是否启用任务功能
- `prefer_lsp`：是否优先使用LSP提供的任务而非Zed语言扩展任务

## 遥测

- 描述：控制Zed收集的信息类型
- 设置：`telemetry`
- 默认值：

```json [settings]
"telemetry": {
  "diagnostics": true,
  "metrics": true
},
```

**选项**

### 诊断信息

- 描述：用于发送调试相关数据的设置，例如崩溃报告。
- 设置项：`diagnostics`
- 默认值：`true`

**选项**

`boolean` 取值

### 使用统计

- 描述：用于发送匿名使用数据的设置，例如您使用 Zed 时所用的语言。
- 设置项：`metrics`
- 默认值：`true`

**选项**

`boolean` 取值

## 终端

- 描述：终端的配置。
- 设置项：`terminal`
- 默认值：

### 终端：程序坞

- 描述：控制程序坞的位置
- 设置：`dock`
- 默认：`bottom`

**选项**

`"bottom"`、`"left"` 或 `"right"`

### 终端：备用滚动

- 描述：设置是否默认启用交替滚动模式（DECSET代码：`?1007`）。该模式可在备用屏幕（如运行vim或less等应用程序时）将鼠标滚轮事件转换为上下按键操作。终端仍可通过ANSI转义码动态启用或禁用此模式。
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
- 设置：`copy_on_select`
- 默认值：`false`

**选项**

`boolean` 值

**示例**

```json [settings]
{
  "terminal": {
    "copy_on_select": true
  }
}
```

### 终端：光标形状

- 描述：控制终端中光标的视觉形状。未明确设置时，默认为块状。
- 设置：`cursor_shape`
- 默认值：`null`（默认为块状）

**选项**

1. 包围后续字符的块状

```json [settings]
{
  "terminal": {
    "cursor_shape": "block"
  }
}
```

2. 竖线状

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

### 终端：复制时保持选中状态

- 描述：复制文本后是否在终端中保持选中状态。
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

- 描述：添加到此对象的任何键值对将被添加到终端的环境变量中。键必须唯一，使用 `:` 分隔单个变量中的多个值
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

- 说明：终端使用的字体大小。未设置时默认与编辑器字体大小一致
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

### 终端：字体系列

- 说明：终端使用的字体。未设置时默认与编辑器字体一致
- 设置项：`font_family`
- 默认值：`null`

**可选值**

用户系统已安装的任何字体系列名称

```json [settings]
{
  "terminal": {
    "font_family": "Berkeley Mono"
  }
}
```

### 终端：字体特性

- 说明：终端使用的字体特性。未设置时默认与编辑器字体特性一致
- 设置项：`font_features`
- 默认值：`null`
- 适用平台：macOS 与 Windows

**可选值**

参考缓冲区字体特性

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
- 设置：`line_height`
- 默认值：`standard`

**选项**

1. 使用适合阅读的行高，1.618。

```json [settings]
{
  "terminal": {
    "line_height": "comfortable"
  }
}
```

2. 使用紧凑的行高，1.3。此选项适用于TUI（文本用户界面），特别是使用框字符的情况。（默认）

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

- 描述：控制终端中前景色与背景色的最小对比度。使用APCA（可访问感知对比度算法）进行颜色调整。设置为0可禁用此功能。
- 设置：`minimum_contrast`
- 默认值：`45`

**选项**

`integer` 取值范围为0到106。常用推荐值：

- `0`：无对比度调整
- `45`：大段流畅文本最低要求（默认）
- `60`：其他内容文本最低要求
- `75`：正文文本最低要求
- `90`：正文文本推荐值

```json [settings]
{
  "terminal": {
    "minimum_contrast": 45
  }
}
```

### 终端：Option键作为Meta键

- 功能说明：将Option键重新解释为“meta”键，类似Emacs中的功能。
- 设置项：`option_as_meta`
- 默认值：`false`

**选项设置**

`boolean` 取值

```json [settings]
{
  "terminal": {
    "option_as_meta": true
  }
}
```

### 终端：Shell设置

- 功能说明：启动终端时使用的shell程序。
- 设置项：`shell`
- 默认值：`system`

**选项设置**

1. 使用系统默认终端配置（通常为`/etc/passwd`文件）。

```json [settings]
{
  "terminal": {
    "shell": "system"
  }
}
```

2. 指定启动程序：

```json [settings]
{
  "terminal": {
    "shell": {
      "program": "sh"
    }
  }
}
```

3. 指定带参数的程序：

## 终端：检测虚拟环境 {#terminal-detect_venv}

- 功能说明：在终端工作目录中（通过工作目录解析）若发现 [Python 虚拟环境](https://docs.python.org/3/library/venv.html)，则自动激活该环境。
- 设置项：`detect_venv`
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

## 终端：工具栏

- 描述：是否在终端工具栏中显示各类元素。
- 设置：`workbench.panel.toolbar.show`
- 默认值：

```json
{
  "workbench.panel.toolbar.show": true
}
```

**选项**

目前仅提供 `title` 选项，用于控制是否显示可通过 `terminal.integrated.tabs.title` 修改的终端标题。

若终端标题为空，则不会显示路径导航栏。

需要在终端中运行的 shell 配置为发送标题信息。

设置标题的示例命令：`echo -e "\033]0;My Title\007"`

### 终端：按钮

- 描述：控制状态栏中终端按钮的显示或隐藏
- 设置：`workbench.activityBar.statusbar.visible`
- 默认值：`true`

**选项**

`true` 或 `false` 值

```json
{
  "workbench.activityBar.statusbar.visible": true
}
```

### 终端：工作目录

- 描述：启动终端时使用的工作目录。
- 设置：[[代码块_3]]
- 默认值：[[代码块_4]]

**选项**

1. 使用当前文件的项目目录。若失败，将回退至首个项目目录策略。

[[代码块_0]]

2. 使用此工作空间中的首个项目目录。若失败，将回退至使用本平台的主目录。

[[代码块_1]]

3. 始终使用本平台的主目录（若能找到）。

[[代码块_2]]

4. 始终使用指定目录。此路径将进行 Shell 扩展。若该路径不是有效目录，终端将默认使用本平台的主目录。

## 交互式解释器

- 描述：交互式解释器设置。
- 配置项：`repl`
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

- 描述：主题设置支持两种形式——可直接指定主题名称，或通过包含`mode`、`dark`和`light`主题配置的对象来定义Zed界面主题。
- 配置项：`theme`
- 默认值：`One Dark`

### 主题对象

- 描述：通过包含`mode`、`dark`和`light`主题配置的对象来定义主题。
- 配置项：`theme`
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
- 设置：[[代码块_3]]
- 默认：[[代码块_4]]

**选项**

1. 将主题设置为深色模式

[[代码块_0]]

2. 将主题设置为浅色模式

[[代码块_1]]

3. 将主题设置为系统模式

[[代码块_2]]

### 深色

- 描述：用于界面的深色 Zed 主题名称。
- 设置：[[代码块_5]]
- 默认：[[代码块_6]]

**选项**

在命令面板中运行 {#操作 主题选择器::切换} 操作，查看当前有效的主题名称列表。

### 浅色

- 描述：用于界面的浅色 Zed 主题名称。
- 设置：[[代码块_7]]
- 默认：[[代码块_8]]

**选项**

在命令面板中运行 {#操作 主题选择器::切换} 操作，查看当前有效的主题名称列表。

## 标题栏

- 描述：是否在标题栏中显示各种元素
- 设置：[[代码块_9]]
- 默认：

**选项**

- `show_branch_icon`：是否在标题栏的分支切换器旁显示分支图标
- `show_branch_name`：是否在标题栏显示分支名称按钮
- `show_project_items`：是否在标题栏显示项目托管平台与名称
- `show_onboarding_banner`：是否在标题栏显示新手引导横幅
- `show_user_picture`：是否在标题栏显示用户头像
- `show_sign_in`：是否在标题栏显示登录按钮
- `show_menus`：是否在标题栏显示菜单栏

## Vim模式

- 说明：是否启用Vim编辑模式
- 设置项：`vim_mode`
- 默认值：`false`

## 无标签页时关闭窗口

- 描述：当窗口无标签页时，使用“关闭活动项”是否应关闭窗口
- 设置：[[代码块_3]]
- 默认值：[[代码块_4]]

**选项**

1. 使用平台默认行为：

[[代码块_0]]

2. 始终关闭窗口：

[[代码块_1]]

3. 从不关闭窗口：

[[代码块_2]]

## 项目面板

- 描述：自定义项目面板
- 设置：[[代码块_5]]
- 默认值：

### 停靠栏

- 描述：控制停靠栏的位置
- 设置：`dock`
- 默认：`left`

**选项**

1. 默认停靠栏位置在左侧

```json [settings]
{
  "dock": "left"
}
```

2. 默认停靠栏位置在右侧

```json [settings]
{
  "dock": "right"
}
```

### 条目间距

- 描述：工作树条目之间的间距
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

### Git状态显示

- 功能说明：标识新建及更新的文件
- 设置项：`git_status`
- 默认值：`true`

**选项配置**

1. 默认启用Git状态显示

```json [settings]
{
  "git_status": true
}
```

2. 默认禁用Git状态显示

```json [settings]
{
  "git_status": false
}
```

### 默认宽度设置

- 功能说明：自定义项目面板的默认占用宽度
- 设置项：`default_width`
- 默认值：`240`

**取值说明**

`float` 数值范围

### 条目自动显隐

- 功能说明：当对应项目条目激活时，是否在项目面板中自动显示。被Git忽略的条目永远不会自动显示。
- 设置项：`auto_reveal_entries`
- 默认值：`true`

**选项配置**

1. 启用条目自动显示

```json [settings]
{
  "auto_reveal_entries": true
}
```

2. 禁用条目自动显示

### 自动折叠目录

- 描述：当目录内仅包含一个子目录时，是否自动折叠该目录。
- 设置：`auto_fold_dirs`
- 默认值：`true`

**选项**

1. 启用自动折叠目录

```json [settings]
{
  "auto_fold_dirs": true
}
```

2. 禁用自动折叠目录

```json [settings]
{
  "auto_fold_dirs": false
}
```

### 缩进尺寸

- 描述：嵌套项目的缩进量（以像素为单位）。
- 设置：`indent_size`
- 默认值：`20`

### 缩进参考线：显示

- 描述：是否在项目面板中显示缩进参考线。
- 设置：`indent_guides`
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

- 描述：是否在项目面板中显示滚动条。可选值：null、"auto"、"system"、"always"、"never"。若未设置则继承编辑器配置，详见其说明。
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

访问[配置页面](./ai/configuration.md)（位于AI版块下），了解更多关于所有智能体相关设置的信息。

## 协作面板

- 描述：协作面板的自定义设置。
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

- 描述：调试器面板及设置的相关配置
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

关于Zed内置调试功能的更多信息，请参阅[调试器页面](./debugger.md)。

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
- `status_style`：Git状态的显示方式。可选`label_color`或`icon`
- `fallback_branch_name`：当`init.defaultBranch`未设置时使用的默认分支名称
- `sort_by_path`：是否按路径而非默认状态排序面板条目
- `collapse_untracked_diff`：是否在差异面板中折叠未跟踪文件
- `scrollbar`：Git面板中滚动条的显示时机

## 大纲面板

- 描述：自定义大纲面板
- 设置项：`outline_panel`
- 默认值：

## 通话设置

- 描述：自定义参与通话时的行为模式
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

- 描述：对未使用代码的淡化程度设置
- 设置项：`unnecessary_code_fade`
- 默认值：`0.3`

**可选参数**

取值范围在`0.0`至`0.9`之间的浮点数值，其中：

- `0.0`表示不进行淡化处理（未使用代码与正常代码显示效果一致）
- `0.9`表示最大程度淡化（未使用代码呈现极浅色调但仍保持可见）

**示例说明**

## 界面字体族

- 描述：用于界面文本的字体名称。
- 设置项：`ui_font_family`
- 默认值：`theme`。当前默认关联至 [IBM Plex](https://www.ibm.com/plex/) 字体。

**可选值**

可指定系统已安装的任意字体族名称，设为 `theme` 使用 Zed 提供的默认字体，或设为 `system` 使用系统默认界面字体（适用于 macOS 和 Windows 系统）。

## 界面字体特性

- 描述：为界面文本启用的 OpenType 字体特性。
- 设置项：`ui_font_features`
- 默认值：

```json
{}
```

- 适用平台：macOS 和 Windows。

**配置说明**

Zed 支持为指定界面字体启用或禁用所有 OpenType 特性，并可设置字体特性数值。

例如，若要禁用字体连字特性，请在设置中添加以下配置：

```json
{
  "ui_font_features": {
    "liga": 0
  }
}
```

您还可以设置其他 OpenType 特性，例如将 `cv01` 设置为 `7`：

```json [settings]
{
  "ui_font_features": {
    "cv01": 7
  }
}
```

## 界面字体备选方案

- 描述：用于界面文本的字体备选方案。
- 设置项：`ui_font_fallbacks`
- 默认值：`null`
- 适用平台：macOS 和 Windows。

**选项设置**

例如，要使用 `Nerd Font` 作为备选字体，请在设置中添加以下内容：

```json [settings]
{
  "ui_font_fallbacks": ["Nerd Font"]
}
```

## 界面字体大小

- 描述：界面文本的默认字体大小。
- 设置项：`ui_font_size`
- 默认值：`16`

**选项设置**

`integer` 取值范围为 `6` 到 `100` 像素（含边界值）

## 界面字体粗细

- 描述：界面文本的默认字体粗细。
- 设置项：`ui_font_weight`
- 默认值：`400`

**选项设置**

`integer` 取值范围在 `100` 到 `900` 之间

## 配置示例：

[[代码块_0]]