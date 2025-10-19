# 主题

扩展中的 `themes` 目录应包含一个或多个主题文件。

每个主题文件都应遵循 [`https://zed.dev/schema/themes/v0.2.0.json`](https://zed.dev/schema/themes/v0.2.0.json) 中指定的 JSON 架构规范。

关于创建主题的更多详情，请参阅[此博客文章](https://zed.dev/blog/user-themes-now-in-preview)。

## 主题 JSON 结构

Zed 主题的结构定义于 [Zed 主题 JSON 架构](https://zed.dev/schema/themes/v0.2.0.json)。

Zed 主题由主题家族对象构成，包含：

- `name`：主题家族的名称
- `author`：主题家族作者的名字
- `themes`：属于该主题家族的主题数组

主题对象的核心组成部分包括：

1. 主题元数据：

   - `name`：主题名称
   - `appearance`：值为 "light" 或 "dark"

2. 位于 `style` 下的样式属性，例如：

- `background`：主背景色
- `foreground`：主文本色
- `accent`：用于高亮和强调的强调色

3. 语法高亮：

   - `syntax`：包含各种语法元素（如关键词、字符串、注释）颜色定义的对象

4. 界面元素：

   - 各种界面组件的颜色，例如：
     - `element.background`：界面元素的背景色
     - `border`：不同状态（正常、聚焦、选中）下的边框颜色
     - `text`：不同状态（正常、弱化、强调）下的文本颜色

5. 编辑器特定颜色：

   - 与编辑器相关元素的颜色，例如：
     - `editor.background`：编辑器背景色
     - `editor.gutter`：边栏颜色
     - `editor.line_number`：行号颜色

6. 终端颜色：
   - 集成终端的 ANSI 颜色定义

我们建议您查阅[现有主题](https://github.com/zed-industries/zed/tree/main/assets/themes)，以便更全面地了解可自定义的样式范围。