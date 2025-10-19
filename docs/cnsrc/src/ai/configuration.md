# 配置

在 Zed 中使用 AI 功能时，您可以从多个维度进行配置：

1. 可使用的 LLM 服务提供商
   - Zed 托管的模型，需要[身份验证](../accounts.md)和[订阅](./subscription.md)
   - [使用您自己的 API 密钥](./llm-providers.md)，无需订阅
   - 使用[外部代理如 Claude Code](./external-agents.md)，无需订阅
2. [模型参数与使用方式](./agent-settings.md#model-settings)
3. [与智能助手面板的交互设置](./agent-settings.md#agent-panel-settings)

## 完全关闭 AI 功能

我们充分尊重希望完全不使用 AI 功能的用户。
要实现这一点，请在您的 `settings.json` 中添加以下配置项：

```json [settings]
{
  "disable_ai": true
}
```

请阅读[这篇博客文章](https://zed.dev/blog/disable-ai-features)，详细了解我们在鼓励用户探索 AI 辅助编程的同时，推出此功能的初衷。