# 外部智能体

Zed 通过[智能体客户端协议（ACP）](https://agentclientprotocol.com)支持基于终端的智能体。

目前，[Gemini CLI](https://github.com/google-gemini/gemini-cli)作为参考实现。
[Claude Code](https://www.anthropic.com/claude-code)也默认包含在内，您还可以[添加自定义的ACP兼容智能体](#添加自定义智能体)。

Zed对外部智能体的支持严格基于用户界面；计费和法律/条款安排直接由您与智能体提供商协商。Zed不对外部智能体的使用收费，我们的[零数据保留协议/隐私保障](./ai-improvement.md) **_仅_** 适用于Zed托管的模型。

## Gemini CLI {#gemini-cli}

Zed提供了在[智能体面板](./agent-panel.md)中直接运行[Gemini CLI](https://github.com/google-gemini/gemini-cli)的功能。

在底层，我们在后台运行 Gemini CLI，并通过 ACP 与其通信。
这意味着您正在运行真正的 Gemini CLI，并享受其所有优势，同时还能在编辑器中查看文件并与之交互。

### 快速开始

自 [Zed Stable v0.201.5](https://zed.dev/releases/stable/0.201.5) 版本起，您可以直接在 Zed 中使用 Gemini CLI。首先通过 {#kb agent::ToggleFocus} 打开智能助手面板，然后点击右上角的 `+` 按钮开启新的 Gemini CLI 会话。

如需将其绑定至键盘快捷键，可通过 `zed: open keymap` 命令编辑 `keymap.json` 文件，并添加以下内容：

```json [keymap]
[
  {
    "bindings": {
      "cmd-alt-g": ["agent::NewExternalAgentThread", { "agent": "gemini" }]
    }
  }
]
```

#### 安装说明

首次创建 Gemini CLI 会话时，Zed 会自动安装 [@google/gemini-cli](https://github.com/zed-industries/claude-code-acp)。该安装包仅供 Zed 使用，并会在您使用智能助手时保持最新版本。

即使您已全局安装 Gemini CLI，Zed 默认仍会使用其托管版本。但您可以通过在配置中添加以下设置，指定使用 `PATH` 中的版本：

```json [settings]
{
  "agent_servers": {
    "gemini": {
      "ignore_system_version": false
    }
  }
}
```

#### 身份验证

当 Gemini CLI 正常运行后，系统将提示您选择身份验证方式。

多数用户应点击"通过 Google 登录"。这将弹出浏览器窗口，直接与 Gemini CLI 完成认证。在此过程中，Zed 不会获取您的 OAuth 或访问令牌。

你也可以使用“Gemini API密钥”。如果选择此选项，且已设置`GEMINI_API_KEY`，我们将直接调用该密钥。否则Zed会提示你输入API密钥，该密钥将安全存储在你的密钥链中，并用于在Zed内启动Gemini CLI。

“Vertex AI”选项适用于正在使用[Vertex AI](https://cloud.google.com/vertex-ai)并已完成环境配置的用户。

更多信息请参阅[Gemini CLI文档](https://github.com/google-gemini/gemini-cli/blob/main/docs/index.md)。

### 使用方式

与Zed原生智能助手类似，你可以通过Gemini CLI完成各类任务。为提供上下文，你可以通过@符号关联文件、近期会话、代码符号或获取网络信息。

> 请注意，部分原生助手功能暂未在Gemini CLI中实现：编辑历史消息、恢复过往会话以及检查点功能。我们计划在近期版本中增加这些特性。

## Claude代码

与 Gemini CLI 类似，您也可以通过 Zed 的[智能助手面板](./agent-panel.md)直接运行 [Claude Code](https://www.anthropic.com/claude-code)。
底层实现上，Zed 通过[专用适配器](https://github.com/zed-industries/claude-code-acp)运行 Claude Code，并通过 ACP 协议与之通信。

### 快速开始

使用 {#kb agent::ToggleFocus} 打开智能助手面板，然后点击右上角的 `+` 按钮开启新的 Claude Code 会话。

如需将其绑定至快捷键，可通过 `zed: open keymap` 命令编辑 `keymap.json` 文件并添加以下配置：

```json [keymap]
[
  {
    "bindings": {
      "cmd-alt-c": ["agent::NewExternalAgentThread", { "agent": "claude_code" }]
    }
  }
]
```

### 身份验证



如果你想覆盖适配器使用的可执行文件，可以在设置中将 `CLAUDE_CODE_EXECUTABLE` 环境变量设为你偏好的可执行文件路径。

```json
{
  "agent_servers": {
    "claude": {
      "env": {
        "CLAUDE_CODE_EXECUTABLE": "/path/to/alternate-claude-code-executable"
      }
    }
  }
}
```

### 使用方式

与 Zed 官方智能助手类似，你可以使用 Claude Code 完成任何所需任务。
为了提供上下文，你可以通过@符号提及文件、最近对话记录、符号定义，或直接获取网页内容。

除了通过 [ACP 协议](https://agentclientprotocol.com)进行交互外，Zed 还依赖 [Claude Code SDK](https://docs.anthropic.com/en/docs/claude-code/sdk/sdk-overview) 来实现某些特定功能。
不过，该 SDK 目前尚未完全开放支持所有功能所需的全部接口：

- 斜杠命令：支持部分[内置命令](https://docs.anthropic.com/en/docs/claude-code/slash-commands#built-in-slash-commands)，同时完全支持[自定义斜杠命令](https://docs.anthropic.com/en/docs/claude-code/slash-commands#custom-slash-commands)
- 支持[子代理](https://docs.anthropic.com/en/docs/claude-code/sub-agents)功能
- 目前暂不支持[钩子](https://docs.anthropic.com/en/docs/claude-code/hooks-guide)功能

> 另请注意，部分[官方代理](./agent-panel.md)功能尚未与Claude Code兼容：包括编辑历史消息、从历史记录恢复线程以及检查点功能。
> 我们计划在近期版本中增加这些功能。

#### CLAUDE.md配置文件

Zed编辑器中的Claude Code将自动识别以下位置的`CLAUDE.md`配置文件：项目根目录、项目子目录或`.claude`根目录。

若当前没有`CLAUDE.md`配置文件，您可以通过`init`斜杠命令要求Claude Code为您创建。

## Codex命令行工具

您也可以通过 Zed 的[智能助手面板](./agent-panel.md)直接运行 [Codex CLI](https://github.com/openai/codex)。
在底层实现中，Zed 通过[专用适配器](https://github.com/zed-industries/codex-acp)运行 Codex CLI，并基于 ACP 协议与其通信。

### 快速开始

从 Zed Stable v0.208 版本开始，您可以直接在 Zed 中使用 Codex。通过 {#kb agent::ToggleFocus} 打开智能助手面板，然后点击右上角的 `+` 按钮即可创建新的 Codex 会话。

若需为此功能设置键盘快捷键，可通过 `zed: open keymap` 命令编辑 `keymap.json` 文件，并添加以下配置：

```json
[
  {
    "bindings": {
      "cmd-alt-c": ["agent::NewExternalAgentThread", { "agent": "codex" }]
    }
  }
]
```

### 身份验证

Zed 的 Codex 安装认证与 Zed 智能助手完全分离。也就是说，通过 [Zed 助手设置](./llm-providers.md#openai) 添加的 OpenAI API 密钥将_不会_被 Codex 用于认证和计费。

为确保使用您选择的计费方式，请 [新建一个 Codex 会话](./agent-panel.md#new-thread)。首次使用时系统将提示您通过以下三种方式之一进行认证：

1. 通过 ChatGPT 登录 - 可使用您现有的付费版 ChatGPT 订阅账户  
   *注意：远程项目暂不支持此方式*
2. `CODEX_API_KEY` - 使用您在环境变量 `CODEX_API_KEY` 中设置的 API 密钥
3. `OPENAI_API_KEY` - 使用您在环境变量 `OPENAI_API_KEY` 中设置的 API 密钥

若已登录但需要更换认证方式，请在会话中输入 `/logout` 并重新认证。

#### 安装说明

首次创建 Codex 线程时，Zed 将自动安装 [codex-acp](https://github.com/zed-industries/codex-acp)。该安装包仅供 Zed 使用，并会在您使用智能助手时保持最新版本。

即使您已在全局环境安装 Codex，Zed 仍会优先使用其托管版本。

### 使用方式

与 Zed 原生智能助手类似，您可通过 Codex 实现各类需求。
如需提供上下文，您可以通过 @ 符号关联文件或代码符号，也可直接获取网络信息。

> 请注意，部分原生助手功能暂未适配 Codex：编辑历史消息、恢复历史线程以及创建检查点。
> 我们计划在近期版本中增加这些功能。

## 添加自定义助手 {#add-custom-agents}

通过以下设置调整，您可以在 Zed 中运行任何支持 ACP 协议的智能助手：

```json [settings]
{
  "agent_servers": {
    "Custom Agent": {
      "command": "node",
      "args": ["~/projects/agent/index.js", "--acp"],
      "env": {}
    }
  }
}
```

如果你正在开发一个遵循该协议的新智能体，并希望进行调试，这也会很有帮助。

你还可以通过使用 `claude` 和 `gemini` 名称，为内置集成指定自定义路径、参数或环境。

## 调试智能体

在 Zed 中使用外部智能体时，你可以通过命令面板中的 `dev: open acp logs` 访问调试视图。这让你能够查看 Zed 与智能体之间发送和接收的消息。

![ACP 日志的调试视图。](https://zed.dev/img/acp/acp-logs.webp)