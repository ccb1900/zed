# 大语言模型服务提供商

要在 Zed 中使用 AI 功能，您需要至少配置一个大语言模型服务提供商。

您可以通过订阅 [Zed 的任一方案](./plans-and-usage.md)，或使用已持有的支持服务商的 API 密钥来实现此配置。

## 使用自有密钥 {#use-your-own-keys}

若您已拥有现有 LLM 服务商（如 Anthropic 或 OpenAI）的 API 密钥，可将其添加到 Zed 中，即可**_免费_**使用智能助手面板的全部功能。

要将现有 API 密钥添加到指定服务商，请前往智能助手面板设置（`agent: open settings`），找到目标服务商，将密钥粘贴至输入框后按回车键确认。

> 注意：API 密钥不会以明文形式存储于您的 `settings.json` 中，而是保存在操作系统的安全凭证存储内。

## 支持的服务商

Zed 提供了丰富的"使用自有密钥"型 LLM 服务商选项

- [Amazon Bedrock](#amazon-bedrock)
- [Anthropic](#anthropic)
- [DeepSeek](#deepseek)
- [GitHub Copilot Chat](#github-copilot-chat)
- [Google AI](#google-ai)
- [LM Studio](#lmstudio)
- [Mistral](#mistral)
- [Ollama](#ollama)
- [OpenAI](#openai)
- [OpenAI API 兼容](#openai-api-compatible)
- [OpenRouter](#openrouter)
- [Vercel](#vercel-v0)
- [xAI](#xai)

### Amazon Bedrock {#amazon-bedrock}

> 支持使用具备流式工具调用功能的模型。
> 更多详情请参阅 [Amazon Bedrock 工具使用文档](https://docs.aws.amazon.com/bedrock/latest/userguide/conversation-inference-supported-models-features.html)。

使用 Amazon Bedrock 的模型需要 AWS 身份验证。
请确保您的凭证已配置以下权限：

- `bedrock:InvokeModelWithResponseStream`
- `bedrock:InvokeModel`

您的 IAM 策略应类似如下：

```json [settings]
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "bedrock:InvokeModel",
        "bedrock:InvokeModelWithResponseStream"
      ],
      "Resource": "*"
    }
  ]
}
```

完成此步骤后，请选择以下两种身份验证方式之一：

#### 通过命名配置文件进行身份验证（推荐）

1. 确保已安装 AWS CLI 并使用命名配置文件完成配置
2. 打开您的 `settings.json` (`zed: open settings`) 文件，在 `language_models` 配置段中包含 `bedrock` 密钥并设置以下参数：
   ```json [settings]
   {
     "language_models": {
       "bedrock": {
         "authentication_method": "named_profile",
         "region": "your-aws-region",
         "profile": "your-profile-name"
       }
     }
   }
   ```

#### 通过静态凭证进行身份验证

虽然可以通过代理面板设置界面直接输入您的AWS访问密钥和密钥进行配置，但我们建议使用命名配置文件以遵循更佳的安全实践。
具体操作如下：

1. 在[IAM控制台](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/users)中创建可供您使用的IAM用户
2. 为该用户创建安全凭证，妥善保存并确保其安全性
3. 使用(`agent: open settings`)打开代理配置，进入Amazon Bedrock版块
4. 将第2步获取的凭证分别填入对应的**访问密钥ID**、**秘密访问密钥**和**区域**字段

#### 跨区域推理

Zed对Amazon Bedrock的实现针对所有支持的模型与区域组合均采用[跨区域推理](https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html)方案。
通过跨区域推理功能，您可以将流量分发至多个AWS区域，从而实现更高的吞吐量。

例如，若您使用`Claude Sonnet 3.7 Thinking`中的`us-east-1`，请求可能会在美国各区域进行处理，即：`us-east-1`、`us-east-2`或`us-west-2`。
跨区域推理请求将始终限定在数据原始所在地理分区内的AWS区域范围。
例如，在美国发起的请求将仅在美国境内的AWS区域进行处理。

虽然数据仅存储在源区域，但在跨区域推理过程中，您输入的提示词和输出结果可能会离开源区域。
所有数据都将在亚马逊安全网络中以加密形式传输。

我们将尽最大努力为每个模型提供跨区域推理支持，具体请参阅[跨区域推理方法代码](https://github.com/zed-industries/zed/blob/main/crates/bedrock/src/models.rs#L297)。

有关最新支持的区域和模型，请参阅《跨区域推理支持模型与区域指南》(https://docs.aws.amazon.com/bedrock/latest/userguide/inference-profiles-support.html)。

### Anthropic {#anthropic}

您可以通过Agent面板中的模型下拉菜单选择使用Anthropic模型。

1. 注册Anthropic账号并[创建API密钥](https://console.anthropic.com/settings/keys)
2. 确保您的Anthropic账户拥有可用额度
3. 打开设置界面（`agent: open settings`）并进入Anthropic版块
4. 输入您的Anthropic API密钥

请注意：即使您已订阅Claude Pro服务，仍需[购买额外额度](https://console.anthropic.com/settings/plans)才能通过API使用该服务。

如果定义了`ANTHROPIC_API_KEY`环境变量，Zed也将自动调用该变量。

#### 自定义模型 {#anthropic-custom-models}

您可以通过在Zed的`settings.json`中添加以下配置，为Anthropic服务商添加自定义模型：

```json [settings]
{
  "language_models": {
    "anthropic": {
      "available_models": [
        {
          "name": "claude-3-5-sonnet-20240620",
          "display_name": "Sonnet 2024-June",
          "max_tokens": 128000,
          "max_output_tokens": 2560,
          "cache_configuration": {
            "max_cache_anchors": 10,
            "min_total_token": 10000,
            "should_speculate": false
          },
          "tool_override": "some-model-that-supports-toolcalling"
        }
      ]
    }
  }
}
```

自定义模型将显示在代理面板的模型下拉列表中。

如果模型支持[扩展思考](https://docs.anthropic.com/en/docs/about-claude/models/extended-thinking-models)功能，您可以通过将模型配置中的模式更改为`thinking`来启用该功能，例如：

```json [settings]
{
  "name": "claude-sonnet-4-latest",
  "display_name": "claude-sonnet-4-thinking",
  "max_tokens": 200000,
  "mode": {
    "type": "thinking",
    "budget_tokens": 4096
  }
}
```

### DeepSeek {#deepseek}

1. 访问 DeepSeek 平台并[创建 API 密钥](https://platform.deepseek.com/api_keys)
2. 打开设置视图 (`agent: open settings`) 并进入 DeepSeek 部分
3. 输入您的 DeepSeek API 密钥

DeepSeek API 密钥将保存在您的密钥链中。

如果定义了 `DEEPSEEK_API_KEY` 环境变量，Zed 也会使用它。

#### 自定义模型 {#deepseek-custom-models}

Zed 代理预配置为使用常见模型（DeepSeek Chat、DeepSeek Reasoner）的最新版本。
如果您希望使用替代模型或自定义 API 端点，可以通过将以下内容添加到您的 Zed `settings.json` 来实现：

```json [settings]
{
  "language_models": {
    "deepseek": {
      "api_url": "https://api.deepseek.com",
      "available_models": [
        {
          "name": "deepseek-chat",
          "display_name": "DeepSeek Chat",
          "max_tokens": 64000
        },
        {
          "name": "deepseek-reasoner",
          "display_name": "DeepSeek Reasoner",
          "max_tokens": 64000,
          "max_output_tokens": 4096
        }
      ]
    }
  }
}
```

自定义模型将显示在智能体面板的模型下拉列表中。
如需使用自定义端点，您也可以修改`api_url`。

### GitHub Copilot 聊天功能 {#github-copilot-chat}

通过在智能体面板的模型下拉列表中选择GitHub Copilot，即可在Zed智能体中使用GitHub Copilot聊天功能。

1. 打开设置界面（`agent: open settings`）并进入GitHub Copilot聊天功能分区
2. 点击`Sign in to use GitHub Copilot`，按照弹窗提示完成操作步骤。

或者，您也可以通过 `GH_COPILOT_TOKEN` 环境变量提供 OAuth 令牌。

> **注意**：如果在下拉菜单中看不到特定模型，您可能需要前往 [GitHub Copilot 设置](https://github.com/settings/copilot/features)启用这些模型。

若要在 Zed 中使用 Copilot Enterprise（包括智能助手和代码补全功能），请按照[配置 GitHub Copilot Enterprise](./edit-prediction.md#github-copilot-enterprise) 中的说明配置企业端点。

### Google AI {#google-ai}

通过在智能助手面板的下拉菜单中选择 Gemini 模型，即可在 Zed 智能助手中使用该模型。

1. 访问 Google AI Studio 网站并[创建 API 密钥](https://aistudio.google.com/app/apikey)
2. 打开设置界面（`agent: open settings`）并进入 Google AI 设置板块
3. 输入您的 Google AI API 密钥后按回车键

Google AI API 密钥将保存至您的密钥链中。

如果定义了`GEMINI_API_KEY`环境变量，Zed也会使用它。更多信息请参阅Gemini文档中的[使用Gemini API密钥](https://ai.google.dev/gemini-api/docs/api-key)。

#### 自定义模型 {#google-ai-custom-models}

默认情况下，Zed将使用`stable`版本的模型，但您可以使用特定版本的模型，包括[实验模型](https://ai.google.dev/gemini-api/docs/models/experimental-models)。您可以通过在模型中添加`mode`配置来启用[思考模式](https://ai.google.dev/gemini-api/docs/thinking)（如果模型支持）。这对于控制推理令牌使用量和响应速度非常有用。如果未指定，Gemini将自动选择思考预算。

以下是一个可以添加到Zed `settings.json`中的自定义Google AI模型示例：

```json [settings]
{
  "language_models": {
    "google": {
      "available_models": [
        {
          "name": "gemini-2.5-flash-preview-05-20",
          "display_name": "Gemini 2.5 Flash (Thinking)",
          "max_tokens": 1000000,
          "mode": {
            "type": "thinking",
            "budget_tokens": 24000
          }
        }
      ]
    }
  }
}
```

自定义模型将显示在代理面板的模型下拉列表中。

### LM Studio {#lmstudio}

1. 下载并安装[最新版LM Studio](https://lmstudio.ai/download)
2. 在应用中点击`cmd/ctrl-shift-m`并至少下载一个模型（例如qwen2.5-coder-7b）。您也可以通过LM Studio命令行获取模型：

   ```sh
   lms get qwen2.5-coder-7b
   ```

3. 通过执行以下命令确保LM Studio API服务器正在运行：

   ```sh
   lms server start
   ```

提示：将 [LM Studio 设为登录项](https://lmstudio.ai/docs/advanced/headless#run-the-llm-service-on-machine-login) 可自动运行 LM Studio 服务器。

### Mistral {#mistral}

1. 访问 Mistral 平台并 [创建 API 密钥](https://console.mistral.ai/api-keys/)
2. 打开配置视图（`agent: open settings`）并导航至 Mistral 部分
3. 输入您的 Mistral API 密钥

Mistral API 密钥将保存至您的密钥链中。

如果定义了 `MISTRAL_API_KEY` 环境变量，Zed 也会使用该变量。

#### 自定义模型 {#mistral-custom-models}

Zed 智能体预配置了多个 Mistral 模型（codestral-latest、mistral-large-latest、mistral-medium-latest、mistral-small-latest、open-mistral-nemo 和 open-codestral-mamba）。
所有默认模型均支持工具调用。
如需使用替代模型或自定义参数，可通过在 Zed `settings.json` 中添加以下配置实现：

```json [settings]
{
  "language_models": {
    "mistral": {
      "api_url": "https://api.mistral.ai/v1",
      "available_models": [
        {
          "name": "mistral-tiny-latest",
          "display_name": "Mistral Tiny",
          "max_tokens": 32000,
          "max_output_tokens": 4096,
          "max_completion_tokens": 1024,
          "supports_tools": true,
          "supports_images": false
        }
      ]
    }
  }
}
```

自定义模型将显示在代理面板的模型下拉列表中。

### Ollama {#ollama}

请从[ollama.com/download](https://ollama.com/download)下载并安装Ollama（适用于Linux或macOS），并通过`ollama --version`确保其正在运行。

1. 下载一个[可用模型](https://ollama.com/models)，例如针对`mistral`的模型：

   ```sh
   ollama pull mistral
   ```

2. 确保Ollama服务器正在运行。您可以通过运行Ollama.app（macOS）或执行以下命令来启动：

   ```sh
   ollama serve
   ```

3. 在智能体面板中，通过模型下拉菜单选择任意一个Ollama模型。

#### Ollama上下文长度 {#ollama-context}

Zed已根据常见模型的能力预设了最大上下文长度（`max_tokens`）。
Zed向Ollama发起的API请求会将其作为`num_ctx`参数包含在内，但默认值不会超过`16384`，因此拥有约16GB内存的用户可以直接使用大多数模型。

完整默认值请参阅[ollama.rs中的get_max_tokens函数](https://github.com/zed-industries/zed/blob/main/crates/ollama/src/ollama.rs)。

> **注意**：智能体面板中显示的词元计数仅为估算值，与模型原生分词器的计数会存在差异。

根据您的硬件配置或使用场景，您可能希望通过settings.json为特定模型限制或增加上下文长度：

```json [settings]
{
  "language_models": {
    "ollama": {
      "api_url": "http://localhost:11434",
      "available_models": [
        {
          "name": "qwen2.5-coder",
          "display_name": "qwen 2.5 coder 32K",
          "max_tokens": 32768,
          "supports_tools": true,
          "supports_thinking": true,
          "supports_images": true
        }
      ]
    }
  }
}
```

如果您设置的上下文长度超出硬件承载能力，Ollama会记录错误信息。
您可以通过运行以下命令查看这些日志：`tail -f ~/.ollama/logs/ollama.log`（macOS系统）或`journalctl -u ollama -f`（Linux系统）。
根据您设备的可用内存情况，可能需要将上下文长度调整为更小的数值。

您也可以选择为每个可用模型指定`keep_alive`的值。
该值可以是整数（秒），也可以是类似"5m"、"10m"、"1h"、"1d"等表示时长的字符串。
例如，`"keep_alive": "120s"`将允许远程服务器在120秒后卸载模型（释放GPU显存）。

`supports_tools`选项控制模型是否使用附加工具。
如果在Ollama目录中模型标记有`tools`，则应提供此选项，并且可以使用内置配置文件`Ask`和`Write`。
如果在Ollama目录中模型未标记`tools`，仍可通过`true`值提供此选项；但请注意，只有`Minimal`内置配置文件会生效。

`supports_thinking`选项控制模型在生成最终答案前是否执行显式的"思考"（推理）过程。
如果在Ollama目录中模型标记有`thinking`，设置此选项后即可在Zed中使用该功能。

`supports_images` 选项可启用模型的视觉功能，使其能够处理对话上下文中包含的图像。
如果模型在 Ollama 目录中被标记为 `vision`，请设置此选项后即可在 Zed 中使用。

#### Ollama 身份验证

除了在自有硬件上运行 Ollama（通常无需身份验证）之外，Zed 还支持连接远程 Ollama 实例。此类连接需使用 API 密钥进行身份验证。

[Ollama Turbo])(https://ollama.com/turbo) 便是此类服务之一。配置 Zed 使用 Ollama Turbo 的步骤如下：

1. 登录 Ollama 账户并订阅 Ollama Turbo 服务
2. 访问 [ollama.com/settings/keys](https://ollama.com/settings/keys) 创建 API 密钥
3. 打开设置界面（`agent: open settings`）并进入 Ollama 设置分区
4. 粘贴您的 API 密钥后按回车键确认
5. 在 API URL 字段中输入 `https://ollama.com`

若已定义 `OLLAMA_API_KEY` 环境变量，Zed 也会自动调用该配置。

### OpenAI {#openai}

1. 访问OpenAI平台并[创建API密钥](https://platform.openai.com/account/api-keys)
2. 确保您的OpenAI账户有可用额度
3. 打开设置界面（`agent: open settings`）并进入OpenAI版块
4. 输入您的OpenAI API密钥

该API密钥将安全存储于您的密钥链中。

若已定义`OPENAI_API_KEY`环境变量，Zed也会自动调用该变量。

#### 自定义模型 {#openai-custom-models}

Zed智能助手默认配置支持主流模型的最新版本（GPT-5、GPT-5 mini、o4-mini、GPT-4.1及其他模型）。
如需使用替代模型（例如预览版本），或需要自定义请求参数，请将以下配置添加到Zed的`settings.json`文件中：

```json [settings]
{
  "language_models": {
    "openai": {
      "available_models": [
        {
          "name": "gpt-5",
          "display_name": "gpt-5 high",
          "reasoning_effort": "high",
          "max_tokens": 272000,
          "max_completion_tokens": 20000
        },
        {
          "name": "gpt-4o-2024-08-06",
          "display_name": "GPT 4o Summer 2024",
          "max_tokens": 128000
        }
      ]
    }
  }
}
```

您必须在`max_tokens`参数中提供模型的上下文窗口大小；该信息可在[OpenAI模型文档](https://platform.openai.com/docs/models)中查询。

对于OpenAI的`o1`模型，还应设置`max_completion_tokens`参数，以避免产生高昂的推理令牌费用。
自定义模型将显示在智能体面板的下拉菜单中。

### OpenAI API 兼容性 {#openai-api-compatible}

Zed支持通过为OpenAI提供商指定自定义`api_url`和`available_models`来使用[兼容OpenAI的API](https://platform.openai.com/docs/api-reference/chat)。  
这对于连接到其他托管服务（如Together AI、Anyscale等）或本地模型非常有用。  

您可以通过用户界面或编辑`settings.json`来添加自定义的、兼容OpenAI的模型。  

要通过用户界面操作，请进入Agent Panel设置（`agent: open settings`），在“LLM Providers”部分标题右侧找到“Add Provider”按钮。  
然后在弹出的窗口中填写可用的输入字段。  

要通过`settings.json`操作，请在`language_models`下添加以下代码片段：

默认情况下，与OpenAI兼容的模型继承以下功能：

- `tools`：true（支持工具/函数调用）
- `images`：false（不支持图像输入）
- `parallel_tool_calls`：false（不支持`parallel_tool_calls`参数）
- `prompt_cache_key`：false（不支持`prompt_cache_key`参数）

请注意，LLM API 密钥不会保存在您的设置文件中。
因此，请确保已将其设置在环境变量中（`<PROVIDER_NAME>_API_KEY=<your api key>`），以便您的设置能够读取。在上面的示例中，应为`TOGETHER_AI_API_KEY=<your api key>`。

### OpenRouter {#openrouter}

OpenRouter 通过单一 API 提供对多种 AI 模型的访问。它支持兼容模型的工具调用功能。

1. 访问 [OpenRouter](https://openrouter.ai) 并创建账户
2. 在您的 [OpenRouter 密钥页面](https://openrouter.ai/keys) 生成 API 密钥
3. 打开设置界面（`agent: open settings`）并进入 OpenRouter 版块
4. 输入您的 OpenRouter API 密钥

该 API 密钥将安全保存在您的密钥链中。

如果定义了 `OPENROUTER_API_KEY` 环境变量，Zed 也会自动调用该变量。

#### 自定义模型 {#openrouter-custom-models}

您可以通过在 Zed `settings.json` 中添加以下配置来为 OpenRouter 服务商添加自定义模型：

```json [settings]
{
  "language_models": {
    "open_router": {
      "api_url": "https://openrouter.ai/api/v1",
      "available_models": [
        {
          "name": "google/gemini-2.0-flash-thinking-exp",
          "display_name": "Gemini 2.0 Flash (Thinking)",
          "max_tokens": 200000,
          "max_output_tokens": 8192,
          "supports_tools": true,
          "supports_images": true,
          "mode": {
            "type": "thinking",
            "budget_tokens": 8000
          }
        }
      ]
    }
  }
}
```

每个模型可用的配置选项包括：

- `name`（必填）：OpenRouter使用的模型标识符
- `display_name`（选填）：用户界面中显示的可读名称
- `max_tokens`（必填）：模型的上下文窗口大小
- `max_output_tokens`（选填）：模型能生成的最大令牌数
- `max_completion_tokens`（选填）：最大补全令牌数
- `supports_tools`（选填）：模型是否支持工具/函数调用
- `supports_images`（选填）：模型是否支持图像输入
- `mode`（选填）：思考模型的特殊模式配置

您可以在[OpenRouter模型页面](https://openrouter.ai/models)查看可用模型及其规格说明。

自定义模型将显示在智能体面板的模型下拉列表中。

#### 供应商路由

您可以通过每个模型条目中的`provider`对象，可选地控制OpenRouter如何在底层上游供应商之间路由特定自定义模型请求。

支持的字段（均为可选）：

- `order`：优先尝试的服务商标识数组，按顺序排列（例如`["anthropic", "openai"]`）
- `allow_fallbacks`（默认值：`true`）：当首选服务商不可用时是否允许使用备用服务商
- `require_parameters`（默认值：`false`）：仅使用支持您提供的所有参数的服务商
- `data_collection`（默认值：`allow`）：`"allow"`或`"disallow"`（控制是否使用可能存储数据的服务商）
- `only`：本次请求允许使用的服务商标识白名单
- `ignore`：需要跳过的服务商标识
- `quantizations`：限制使用特定量化变体（例如`["int4","int8"]`）
- `sort`：候选服务商的排序策略（例如`"price"`或`"throughput"`）

为模型添加路由配置的示例：

```json [settings]
{
  "language_models": {
    "open_router": {
      "api_url": "https://openrouter.ai/api/v1",
      "available_models": [
        {
          "name": "openrouter/auto",
          "display_name": "Auto Router (Tools Preferred)",
          "max_tokens": 2000000,
          "supports_tools": true,
          "provider": {
            "order": ["anthropic", "openai"],
            "allow_fallbacks": true,
            "require_parameters": true,
            "only": ["anthropic", "openai", "google"],
            "ignore": ["cohere"],
            "quantizations": ["int8"],
            "sort": "price",
            "data_collection": "allow"
          }
        }
      ]
    }
  }
}
```

这些路由控制功能让您能够精细调整成本、性能和可靠性之间的权衡，而无需更改在用户界面中选择的模型名称。

### Vercel v0 {#vercel-v0}

[Vercel v0](https://vercel.com/docs/v0/api) 是一款专为生成全栈应用打造的专家模型，具备框架感知的智能补全功能，特别针对 Next.js 和 Vercel 等现代技术栈进行了优化。
该模型支持文本与图像输入，并能提供流畅的实时流式响应。

v0 系列模型兼容 [OpenAI API 标准](/#openai-api-compatible)，但在 Zed 面板设置界面中，Vercel 被列为首选服务提供商。

若要在 Zed 中使用该服务，请先确保已创建 [v0 API 密钥](https://v0.dev/chat/settings/keys)。
获取密钥后，直接将其粘贴至面板设置界面的 Vercel 服务商配置区域。

随后您将在智能体面板的模型下拉菜单中看到 `v0-1.5-md` 选项。

### xAI {#xai}

Zed 为 [xAI](https://x.ai/) 模型提供原生支持。您可使用自己的 API 密钥接入 Grok 系列模型。

1. [在 xAI 控制台创建 API 密钥](https://console.x.ai/team/default/api-keys)
2. 打开设置界面（`agent: open settings`）并进入 **xAI** 版块
3. 输入您的 xAI API 密钥

xAI API 密钥将保存在您的密钥链中。如果定义了 `XAI_API_KEY` 环境变量，Zed 也会使用该变量。

> **注意：** 虽然 xAI API 与 OpenAI 兼容，但 Zed 将其作为专属服务商提供原生支持。为获得最佳体验，我们建议使用专属的 `x_ai` 服务商配置，而非 [OpenAI API 兼容](#openai-api-compatible) 方案。

#### 自定义模型 {#xai-custom-models}

Zed 智能助手已预置常用 Grok 模型。如需使用替代模型或自定义参数，可通过在 Zed `settings.json` 中添加以下配置实现：

```json [settings]
{
  "language_models": {
    "x_ai": {
      "api_url": "https://api.x.ai/v1",
      "available_models": [
        {
          "name": "grok-1.5",
          "display_name": "Grok 1.5",
          "max_tokens": 131072,
          "max_output_tokens": 8192
        },
        {
          "name": "grok-1.5v",
          "display_name": "Grok 1.5V (Vision)",
          "max_tokens": 131072,
          "max_output_tokens": 8192,
          "supports_images": true
        }
      ]
    }
  }
}
```

## 自定义供应商端点 {#custom-provider-endpoint}

您可以为不同的供应商使用自定义API端点，只要该端点与供应商的API结构兼容即可。
具体操作方式是在您的`settings.json`中添加以下配置：

```json [settings]
{
  "language_models": {
    "some-provider": {
      "api_url": "http://localhost:11434"
    }
  }
}
```

目前，`some-provider`可以是以下任意值：`anthropic`、`google`、`ollama`、`openai`。

这正是支撑各类模型运行的同一套基础设施，例如[兼容OpenAI的模型](#openai-api-compatible)也基于此构建。