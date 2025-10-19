# GDScript

Zed 编辑器中的 Godot [GDScript](https://gdscript.com/) 语言支持由社区维护的 [GDScript 扩展](https://github.com/grndctrl/zed-gdscript)提供。
问题反馈请至：[https://github.com/grndctrl/zed-gdscript/issues](https://github.com/grndctrl/zed-gdscript/issues)

- 语法分析器：[PrestonKnopp/tree-sitter-gdscript](https://github.com/PrestonKnopp/tree-sitter-gdscript) 与 [PrestonKnopp/tree-sitter-godot-resource](https://github.com/PrestonKnopp/tree-sitter-godot-resource)
- 语言服务器：[gdscript-language-server](https://github.com/godotengine/godot)

## 环境配置

1. 下载并安装 [macOS 版 Godot](https://godotengine.org/download/macos/)。
2. 解压 Godot.app 并将其拖入您的 /Applications 文件夹。
3. 打开 Godot.app 并打开您的项目（示例项目亦可）
4. 在 Godot 编辑器中：编辑器菜单 -> 编辑器设置；左侧边栏向下滚动至 `Text Editor -> External`
   1. 使用外部编辑器："✅ 开启"
   2. 执行路径：`/Applications/Zed.app/Contents/MacOS/zed`
   3. 执行参数：`{project} {file}:{line}:{col}`
   4. 关闭设置以保存。
5. 在 Godot 中双击任意 \*.gd 脚本，Zed 将自动启动

<!--
TBD: GDScript Linux setup
-->

## 使用说明

当 Godot 运行时，GDScript 扩展将连接至 Godot 运行时提供的语言服务器，并提供 `jump to definition`、按住 cmd 键时的悬停状态提示及其他语言服务器功能。

> 注意：若 Zed 已通过现有工作空间运行，从 Godot 启动将失败。退出 Zed 后即可恢复正常使用。