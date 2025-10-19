# 自动补全功能

Zed 支持两种自动补全来源：

1. 由 Zed 自动安装的语言服务器（LSP）或通过 [Zed 语言扩展](languages.md) 提供的“代码补全”
2. 由 Zed 自研的 Zeta 模型或外部服务商（如 [GitHub Copilot](#github-copilot) 或 [Supermaven](#supermaven)）提供的“编辑预测”

## 语言服务器代码补全 {#code-completions}

当有适用的语言服务器可用时，Zed 将提供当前文件中变量名、函数及其他符号的补全建议。您可以通过在 Zed `settings.json` 文件中添加以下配置来禁用此功能：

```json [settings]
"show_completions_on_input": false
```

您可以通过 `ctrl-space` 快捷键或从命令面板触发 `editor::ShowCompletions` 操作来手动唤出补全建议。

更多相关信息请参阅：

- [配置支持的语言](./configuring-languages.md)
- [Zed 支持的语言列表](./languages.md)

## 编辑预测 {#edit-predictions}

Zed 内置支持通过 [Zeta](https://huggingface.co/zed-industries/zeta)（Zed 的开源开放数据模型）一次性预测多个编辑内容。
编辑预测会随着您的输入实时显示，大多数情况下只需按下 `tab` 即可采纳建议。

关于如何设置和配置 Zed 编辑预测功能的更多信息，请参阅 [编辑预测文档](./ai/edit-prediction.md)。