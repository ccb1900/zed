# Rego

Zed 编辑器中的 Rego 语言支持由社区维护的 [Rego 扩展](https://github.com/StyraInc/zed-rego)提供。

- 语法分析器：[FallenAngel97/tree-sitter-rego](https://github.com/FallenAngel97/tree-sitter-rego)
- 语言服务器：[StyraInc/regal](https://github.com/StyraInc/regal)

## 安装说明

该扩展主要基于 [Regal](https://docs.styra.com/regal/language-server) 语言服务器实现，需先安装 Regal 才能充分发挥扩展功能。请参阅 [入门指南](https://docs.styra.com/regal#getting-started) 获取详细信息。

## 配置说明

扩展功能通过 `.regal/config.yaml` 文件进行配置。以下为示例配置：禁用 `todo-comment` 规则，自定义 `line-length` 规则，并为 `opa-fmt` 规则忽略测试文件：

阅读 Regal 的 [配置文档](https://docs.styra.com/regal#configuration) 了解更多信息。