# 诊断功能

Zed 从语言服务器获取诊断信息，同时支持 LSP 的推送和拉取两种模式，这使得它能与所有现有语言服务器兼容。

# 常规诊断

默认情况下，Zed 会将所有诊断信息以文本下划线的形式显示在编辑器和滚动条中。

编辑器诊断可通过

```json [settings]
"diagnostics_max_severity": null
```

编辑器设置进行筛选（可选值：`"off"`、`"error"`、`"warning"`、`"info"`、`"hint"`、`null`（默认值，显示全部诊断））。

滚动条诊断则通过

```json [settings]
"scrollbar": {
  "diagnostics": "all",
}
```

配置进行设置（可选值：`"none"`、`"error"`、`"warning"`、`"information"`、`"all"`（默认值））。

诊断信息可悬停显示完整渲染后的提示框。
或者，可使用`editor::GoToDiagnostic`和`editor::GoToPreviousDiagnostic`在编辑器内切换诊断项，并为当前活动诊断显示弹出框。

# 行内诊断（错误透镜）

Zed支持在代码右侧以透镜形式显示诊断信息。
该功能默认关闭，可通过编辑器菜单临时开启（或关闭），或通过

```json [settings]
"diagnostics": {
  "inline": {
    "enabled": true,
    "max_severity": null, // same values as the `diagnostics_max_severity` from the editor settings
  }
}
```

永久启用

# 其他界面区域

## 项目面板

项目面板中的文件条目可根据其内部诊断的严重程度进行色彩标记。

配置方式：

```json [settings]
"project_panel": {
  "show_diagnostics": "all",
}
```

配置项（可选值：`"off"`、`"errors"`、`"all"`（默认值））

## 编辑器标签页

与项目面板类似，编辑器标签页可通过

```json [settings]
"tabs": {
  "show_diagnostics": "off",
}
```

配置项进行色彩设置（可选值：`"off"`（默认）、`"errors"`、`"all"`）