# 智能助手设置

了解可在 Zed 智能助手面板中自定义的所有设置选项。

## 模型设置 {#model-settings}

### 默认模型 {#default-model}

若您使用 [Zed 托管大语言模型服务](./subscription.md)，系统将自动设置 `claude-sonnet-4` 作为智能任务（助手面板、行内助手）的默认模型，同时指定 `gpt-5-nano` 作为“快速”任务的默认模型（话题总结、Git 提交信息生成）。若未订阅服务或需要修改默认设置，您可手动编辑设置中的 `default_model` 对象：

```json [settings]
{
  "agent": {
    "default_model": {
      "provider": "openai",
      "model": "gpt-4o"
    }
  }
}
```

### 功能专属模型 {#feature-specific-models}

您可为以下智能功能分别指定独立模型：

- 话题总结模型：用于生成话题摘要
- 行内助手模型：用于行内助手功能
- 提交信息模型：用于生成 Git 提交信息

> 如果这些功能未设置自定义模型，它们会自动回退使用默认模型。

### 内联辅助的备选模型 {#alternative-assists}

内联辅助功能尤其具备使用不同模型并行执行多次生成的能力。
通过为其分配多个模型即可实现这一功能，这使上述配置更进一步。

完成配置后，内联辅助界面将提供控件，供用户在不同模型生成的输出结果之间切换查看。

您在此处指定的模型始终会与您的[默认模型](#default-model)一起使用。

例如，以下配置将为每次辅助生成两个输出：一个使用Claude Sonnet 4（默认模型），另一个使用GPT-5-mini。

```json [settings]
{
  "agent": {
    "default_model": {
      "provider": "zed.dev",
      "model": "claude-sonnet-4"
    },
    "inline_alternatives": [
      {
        "provider": "zed.dev",
        "model": "gpt-4-mini"
      }
    ]
  }
}
```

### 模型温度设置

为特定提供商和/或模型指定自定义温度参数：

```json [settings]
"model_parameters": [
  // To set parameters for all requests to OpenAI models:
  {
    "provider": "openai",
    "temperature": 0.5
  },
  // To set parameters for all requests in general:
  {
    "temperature": 0
  },
  // To set parameters for a specific provider and model:
  {
    "provider": "zed.dev",
    "model": "claude-sonnet-4",
    "temperature": 1.0
  }
],
```

## 代理面板设置 {#agent-panel-settings}

请注意，部分设置同时会显示在代理面板的设置界面中。您可通过`agent: open settings`操作，或点击面板右上角的下拉菜单进入该界面。

### 默认视图

使用`default_view`设置可调整代理面板的默认显示模式。
可选模式包括`thread`（默认）与`text_thread`：

```json [settings]
{
  "agent": {
    "default_view": "text_thread"
  }
}
```

### 字体大小

通过`agent_font_size`设置可调整面板中代理响应内容的字体尺寸。

```json [settings]
{
  "agent": {
    "agent_font_size": 18
  }
}
```

> 代理面板中的所有编辑器——无论是主消息输入框还是历史消息——均采用等宽字体，因此受`buffer_font_size`设置控制，该设置需在您的`settings.json`中进行全局配置。

### 自动运行命令

控制是否允许代理在未经您许可的情况下运行命令。
默认值为`false`。

```json [settings]
{
  "agent": {
    "always_allow_tool_actions": true
  }
}
```

### 单文件审阅

控制是否在代理完成编辑后，在单个缓冲区中显示审阅操作（接受与拒绝）。
默认值为`false`。

```json [settings]
{
  "agent": {
    "single_file_review": true
  }
}
```

当设置为false时，这些控件仅在多缓冲区审阅标签页中可用。

### 声音通知

控制当代理完成生成更改或需要您输入时，是否播放通知声音。
默认值为`false`。

```json [settings]
{
  "agent": {
    "play_sound_when_agent_done": true
  }
}
```

### 消息编辑器尺寸

使用 `message_editor_min_lines` 设置来控制代理消息编辑器的最小行高。
默认设置为 `4`，最大行数始终为最小值的两倍。

```json [settings]
{
  "agent": {
    "message_editor_min_lines": 4
  }
}
```

### 发送消息的修饰键

设置发送消息需要按下修饰键（macOS 为 `cmd`，Linux 为 `ctrl`）。
这有助于更审慎地构建提示内容。
默认值为 `false`。

```json [settings]
{
  "agent": {
    "use_modifier_to_send": true
  }
}
```

### 编辑卡片

通过 `expand_edit_card` 设置控制编辑卡片是否在代理面板中显示完整差异信息。
默认设置为 `true`，若设为 false，卡片高度将限制在固定行数内，需点击展开查看完整内容。

```json [settings]
{
  "agent": {
    "expand_edit_card": false
  }
}
```

### 终端卡片

使用 `expand_terminal_card` 设置来控制终端卡片是否在代理面板中显示命令输出。
该设置默认为 `true`，但如果设为 false，卡片将完全折叠（即使命令正在运行），需要点击才能展开。

```json [settings]
{
  "agent": {
    "expand_terminal_card": false
  }
}
```

### 反馈控制

控制是否在每个代理响应底部显示点赞/点踩按钮，允许用户就代理表现向 Zed 提供反馈。
默认值为 `true`。

```json [settings]
{
  "agent": {
    "enable_feedback": false
  }
}
```