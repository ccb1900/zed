# Yarn

[Yarn](https://yarnpkg.com/) 是一款多功能包管理器，能够提升 JavaScript 及其他语言项目的依赖管理效率与工作流效能。它确保依赖树确定性，提供离线支持，并增强构建过程的安全性保障。

## 环境配置

1. 执行 `yarn dlx @yarnpkg/sdks base` 命令生成 `.yarn/sdks` 目录
2. 在 [LSP 初始化选项](../configuring-zed.md#lsp) 中将语言服务器（如 VTSLS）的 TypeScript SDK 路径设置为 `.yarn/sdks/typescript/lib` 目录。具体配置方式因语言服务器而异：以 VTSLS 为例，需配置 [`typescript.tsdk`](https://github.com/yioneko/vtsls/blob/6adfb5d3889ad4b82c5e238446b27ae3ee1e3767/packages/service/configuration.schema.json#L5) 参数
3. 完成！此时代码跳转、自动补全、悬停文档等语言服务器功能即可正常使用