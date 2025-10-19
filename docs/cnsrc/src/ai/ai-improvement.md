# Zed AI 改进方案

## 智能体面板

### 选择性加入机制

当您通过以下任一方式使用智能体面板时：

- [Zed托管模型](./subscription.md)
- [通过API密钥连接非Zed AI服务](./llm-providers.md)
- 使用[外部智能体](./external-agents.md)

Zed不会持久存储用户内容，也不会使用用户内容来评估和/或改进我们的AI功能，除非用户明确与Zed共享内容。每次共享均为选择性加入机制，单次共享不会导致未来内容或数据被自动共享。

> 请注意：对回复进行评分时，与该回复相关的数据将被发送至Zed服务器
> **_若不希望数据持久存储在Zed服务器，请勿进行评分操作_**。在您未明确对回复评分的情况下，我们不会收集用于改进智能体服务的数据。

当通过Zed托管模型使用上游服务时，我们已获得服务提供商保证：您的用户内容不会被用于模型训练。

| 服务商    | 无训练保证                                | 零数据保留 (ZDR)                                                                                                                                         |
| --------- | ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Anthropic | [是](https://www.anthropic.com/legal/commercial-terms) | [是](https://privacy.anthropic.com/en/articles/8956058-i-have-a-zero-data-retention-agreement-with-anthropic-what-products-does-it-apply-to)              |
| Google    | [是](https://cloud.google.com/terms/service-terms)     | **否**，正在推进中                                                                                                                                        |
| OpenAI    | [是](https://openai.com/enterprise-privacy/)           | [是](https://platform.openai.com/docs/guides/your-data)                                                                                                   |

> 目前 Zed 通过 [Google AI Studio](https://ai.google.dev/aistudio) 支持 Gemini 模型，但该平台**_不_**支持 ZDR（零数据保留）。我们正在向支持 ZDR 的 [Vertex AI](https://cloud.google.com/vertex-ai?hl=zh-cn) 平台迁移，迁移完成后将为所有使用 Zed 托管版 Google/Gemini 模型的用户提供 ZDR 功能。

> 若上游模型供应商的 ZDR 功能对您至关重要，_请暂时不要使用 Gemini 模型_。但请注意，所有由 Zed 托管的模型供应商均不会将您的数据用于训练目的。

当您使用自有 API 密钥或外部智能体时，**Zed 无法控制相关服务提供商对您数据的使用方式**。
您应查阅与各服务提供商签订的协议，了解适用的具体条款与条件。

### 我们收集的数据

对于您明确与我们共享的提示词，Zed 可能会存储这些提示词的副本及智能体面板具体使用情况的相关数据。

这些数据包括：

- 提供给智能体的提示
- 您包含的任何注释
- 关于智能线程的产品遥测数据
- 关于您Zed安装的元数据

### 数据处理

收集的数据存储在Snowflake中，这是一个我们用于追踪其他指标的私有数据库。我们会定期审查这些数据，以改进整体智能方法，并通过系统提示、工具使用等方式优化产品。我们确保所有包含的数据均经过匿名化处理，不包含任何敏感信息（访问令牌、用户ID、电子邮箱地址等）。

## 编辑预测功能

默认情况下，使用Zed编辑预测功能时，Zed不会持久存储用户内容，也不会将用户内容用于模型训练。

### 自愿参与

从事开源许可项目的用户可选择自愿提供模型改进反馈。此参与选项按项目逐个设置。若您参与多个开源项目并希望提供模型改进反馈，需为每个项目单独选择参与。

在处理您未选择加入的其他项目时，Zed不会持久存储用户内容，也不会将用户内容用于模型训练。

您可以通过以下文件查看Zed如何精确检测开源许可证：[license_detection.rs](https://github.com/zed-industries/zed/blob/main/crates/zeta/src/license_detection.rs)。

### 排除条款

即使您已选择加入模型改进反馈，Zed仍会主动将特定文件完全排除在预测性编辑功能之外。

您可通过命令面板打开`zed: open default settings`来查看此排除列表：

用户可以通过在 Zed 的 settings.json 中的 [`edit_predictions.disabled_globs`](https://zed.dev/docs/configuring-zed#edit-predictions) 添加额外路径和/或文件扩展名来显式排除它们：

```json [settings]
{
  "edit_predictions": {
    "disabled_globs": ["secret_dir/*", "**/*.log"]
  }
}
```

### 我们收集的数据

对于您已选择加入的开源项目，Zed 可能会存储向 Zed AI 预测服务发送的请求和响应的副本。

这些数据包括：

- 编辑预测
- 光标周围的缓冲区部分内容
- 最近的若干次编辑记录
- 当前缓冲区大纲结构
- 来自语言服务器的诊断信息（错误、警告等）

### 数据处理规范

收集的数据将存储于Snowflake私有数据库，该数据库同时记录其他指标数据。我们会定期审查这些数据以筛选训练样本，纳入模型训练数据集。我们确保所有入选数据均经过匿名化处理，不包含任何敏感信息（访问令牌、用户ID、电子邮箱等）。该训练数据集已公开于[huggingface.co/datasets/zed-industries/zeta](https://huggingface.co/datasets/zed-industries/zeta)。

### 模型输出

我们使用该训练数据集对[Qwen2.5-Coder-7B](https://huggingface.co/Qwen/Qwen2.5-Coder-7B)进行微调，最终模型发布于[huggingface.co/zed-industries/zeta](https://huggingface.co/zed-industries/zeta)。

## 适用条款

更多详情，请参阅 [Zed 服务条款](https://zed.dev/terms-of-service)。