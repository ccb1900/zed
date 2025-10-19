# 多缓冲区编辑

Zed赋予您的一项超能力是能够同时编辑多个文件。当与多光标功能结合使用时，这能让大规模代码重构变得显著更快。

## 在多缓冲区中编辑

<div class="video" style="position: relative; padding-top: 71.71314741035857%;">
  <iframe
    src="https://customer-snccc0j9v3kfzkif.cloudflarestream.com/bda0a6584c19f4b39e58a263c0ae4358/iframe?muted=true&preload=true&loop=true&autoplay=true&poster=https%3A%2F%2Fcustomer-snccc0j9v3kfzkif.cloudflarestream.com%2Fbda0a6584c19f4b39e58a263c0ae4358%2Fthumbnails%2Fthumbnail.jpg%3Ftime%3D%26height%3D600&controls=false"
    style="border: none; position: absolute; top: 0; left: 0; height: 100%; width: 100%;"
    allow="accelerometer; gyroscope; autoplay; encrypted-media; picture-in-picture;"
    allowfullscreen="true"
  ></iframe>
</div>

编辑多缓冲区与编辑普通文件相同。您所做的更改将反映在编辑器其余部分中该文件的已打开副本中，并且您可以使用`editor: Save`（在macOS上绑定为`cmd-s`，在Windows/Linux上为`ctrl-s`，或在Vim模式下为`:w`）保存所有文件。

在多缓冲区中，使用多光标同时编辑每个文件通常很有用。如果您想编辑几个实例，可以使用鼠标（在macOS上为`option-click`，在Windows/Linux上为`alt-click`）或键盘选择它们。在macOS上使用`cmd-d`，在Windows/Linux上使用`ctrl-d`，或在Vim模式下使用`gl`将选择光标下单词的下一个匹配项。

当您想要编辑所有匹配项时，可以通过运行`editor: Select All Matches`命令（在macOS上为`cmd-shift-l`，在Windows/Linux上为`ctrl-shift-l`，或在Vim模式下为`g a`）来选择它们。

## 导航到源文件

虽然您可以在多缓冲区中轻松编辑文件，但直接导航到源文件通常更为便利。您可以通过点击摘录之间的任意分隔线，或将光标置于某个摘录中并执行`editor: open excerpts`命令来实现。需要注意的是，若使用多个光标，该命令将在多缓冲区中打开每个光标对应的源文件。

此外，若您习惯使用鼠标操作，可通过启用`"double_click_in_multibuffer": "open"`设置实现双击摘录即可打开文件的功能。

## 项目搜索

执行`pane: Toggle Search`命令（macOS系统使用`cmd-shift-f`，Windows/Linux系统使用`ctrl-shift-f`，Vim模式使用`g/`）即可启动搜索。搜索完成后，结果将显示在新的多缓冲区中，整个项目中每个匹配行都会生成独立摘录。

## 诊断信息

若已安装语言服务器，诊断面板将展示项目中的所有错误。您可通过点击状态栏图标开启，或执行以下命令：macOS系统使用` ('cmd-shift-m`，Windows/Linux系统使用`ctrl-shift-m`，Vim模式使用`:clist`。

## 引用查找

若已安装语言服务器，可通过`editor: Find References`命令（macOS系统使用`cmd-click`，Windows/Linux系统使用`ctrl-click`，Vim模式使用`g A`）查找光标所在符号的所有引用。

根据语言服务器的不同，当存在多个可能定义时，`editor: Go To Definition`与`editor: Go To Type Definition`等命令亦会开启多缓冲区界面。