# 大纲面板

除了模态大纲视图（`cmd-shift-o`）之外，Zed 还提供了大纲面板功能。您可以通过`cmd-shift-b`（或通过命令面板输入`outline panel: toggle focus`），或点击状态栏中的`Outline Panel`按钮来启用大纲面板。

当查看“单例”缓冲区（即标签页中的单个文件）时，大纲面板的工作方式与模态大纲视图类似——它会显示当前缓冲区符号的树状结构，这些符号由 tree-sitter 解析器提供。点击任意条目即可跳转到文件中对应的段落。随着光标在文件中移动，大纲视图也会自动滚动到与当前光标位置对应的段落。

![在单例缓冲区中使用大纲面板](https://zed.dev/img/outline-panel/singleton.png)

## 多缓冲区使用场景

当应用于多缓冲区时，大纲面板的功能优势尤为突出。以下是其多样化应用的几个示例：

### 项目搜索结果

通过大纲面板快速概览整个项目的搜索结果分布。

![在项目搜索多缓冲区中使用大纲面板](https://zed.dev/img/outline-panel/project-search.png)

### 项目诊断

查看语言服务器报告的所有错误和警告摘要。

![在查看项目诊断多缓冲区时使用大纲面板](https://zed.dev/img/outline-panel/project-diagnostics.png)

### 查找所有引用

使用`editor: find all references`功能时，可快速浏览所有引用位置。

![在查看`find all references`多缓冲区时使用大纲面板](https://zed.dev/img/outline-panel/find-all-references.png)

大纲视图能帮助您快速定位代码的特定部分，并在处理多缓冲区中的大型结果集时保持上下文连贯性。