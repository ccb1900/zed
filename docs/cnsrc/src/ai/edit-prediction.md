# 编辑预测

编辑预测是 Zed 通过 AI 预测您想编写代码的原生机制。
每次按键都会向我们的[开源开放数据集 Zeta 模型](https://huggingface.co/zed-industries/zeta)发送新请求，该模型将返回单行或多行建议，您可以通过按下 `tab` 快速接受。

## 配置 Zeta

Zed 的编辑预测最初通过标题栏上的横幅引入。
点击该横幅将弹出一个对话框，其中的按钮（"启用编辑预测"）会将 `zed` 设置为您的 `edit_prediction_provider`。

![入门横幅与对话框](https://zed.dev/img/edit-prediction/docs.webp)

但如果您未看到该横幅，Zed 的编辑预测是默认的编辑预测提供程序，您应能在状态栏中直接看到它。

### 切换模式 {#switching-modes}

Zed 的编辑预测提供两种不同的显示模式：

1. `eager`（默认模式）：预测内容以内联方式显示，只要不与语言服务器补全功能冲突
2. `subtle`：仅当按住修饰键时才会显示内联预测（默认为`alt`）

可通过`mode`键切换模式：

```json [settings]
"edit_predictions": {
  "mode": "eager" // or "subtle"
},
```

或通过状态栏菜单直接切换：

![编辑预测状态栏菜单，包含模式切换功能](https://zed.dev/img/edit-prediction/status-bar-menu.webp)

### 与其他`tab`操作的冲突 {#edit-predictions-conflict}

默认情况下，当`tab`通常执行其他操作时，Zed要求使用修饰键来接受预测：

1. 当语言服务器补全菜单可见时
2. 当光标未处于正确缩进级别时

在这些情况下，会使用 `alt-tab` 来接受预测。当语言服务器补全菜单打开时，先按住 `alt` 会使其暂时消失，以便在缓冲区中预览预测内容。

在 Linux 系统上，`alt-tab` 通常被窗口管理器用于切换窗口，因此默认将 `alt-l` 设为接受预测的快捷键绑定。`tab` 和 `alt-tab` 同样有效，但默认不显示。

{#action editor::AcceptPartialEditPrediction} ({#kb editor::AcceptPartialEditPrediction}) 可用于接受当前编辑预测直至下一个单词边界。

其他供应商的配置请参阅下方的 [配置 GitHub Copilot](#github-copilot) 和 [配置 Supermaven](#supermaven) 章节。这些供应商仅支持在当前光标位置插入文本，而 Zeta 模型提供包括删除在内的多种预测操作。

## 配置编辑预测快捷键 {#edit-predictions-keybinding}

默认情况下，使用 `tab` 来接受编辑预测。您可以通过在键位映射中插入以下代码来使用其他键绑定：

```json [settings]
{
  "context": "Editor && edit_prediction",
  "bindings": {
    // Here we also allow `alt-enter` to accept the prediction
    "alt-enter": "editor::AcceptEditPrediction"
  }
}
```

当与 `tab` 键[发生冲突](#edit-predictions-conflict)时，Zed 会使用不同的上下文来接受键绑定（`edit_prediction_conflict`）。如果您想使用其他键，可以在键位映射中插入以下代码：

```json [settings]
{
  "context": "Editor && edit_prediction_conflict",
  "bindings": {
    "ctrl-enter": "editor::AcceptEditPrediction" // Example of a modified keybinding
  }
}
```

如果您的键绑定包含修饰键（如上例中的 `ctrl`），该修饰键也将用于预览编辑预测并临时隐藏语言服务器补全菜单。

您也可以将此操作绑定到不带修饰键的快捷键。在这种情况下，Zed 将使用默认修饰键（`alt`）来预览编辑预测。

```json [settings]
{
  "context": "Editor && edit_prediction_conflict",
  "bindings": {
    // Here we bind tab to accept even when there's a language server completion
    // or the cursor isn't at the correct indentation level
    "tab": "editor::AcceptEditPrediction"
  }
}
```

若要在存在语言服务器补全菜单时保留使用修饰键接受预测的功能，同时允许 `tab` 无论光标位置如何都能接受预测，您可以通过 `showing_completions` 进一步指定上下文：

```json [settings]
{
  "context": "Editor && edit_prediction_conflict && !showing_completions",
  "bindings": {
    // Here we don't require a modifier unless there's a language server completion
    "tab": "editor::AcceptEditPrediction"
  }
}
```

### 快捷键设置示例：始终使用 Alt-Tab

下面的键绑定示例导致始终使用`alt-tab`，而非有时使用`tab`。您可能希望如此，以便仅使用一个键绑定来接受编辑预测，因为`tab`的行为会随上下文变化。

```json [keymap]
  {
    "context": "Editor && edit_prediction",
    "bindings": {
      "alt-tab": "editor::AcceptEditPrediction"
    }
  },
  // Bind `tab` back to its original behavior.
  {
    "context": "Editor",
    "bindings": {
      "tab": "editor::Tab"
    }
  },
  {
    "context": "Editor && showing_completions",
    "bindings": {
      "tab": "editor::ComposeCompletion"
    }
  },
```

如果在`settings.json`中设置了`"vim_mode": true`，则需在上述绑定后添加额外绑定，以将`tab`恢复至其原始行为：

### 键位绑定示例：在 Linux 上显示 Tab 与 Alt-Tab

虽然 `tab` 和 `alt-tab` 在 Linux 系统中受支持，但实际显示的是 `alt-l`。若您的窗口管理器未占用 `alt-tab`，且您希望使用 `tab` 和 `alt-tab`，请将以下绑定配置添加至 `keymap.json`：

### 缺少快捷键绑定 {#edit-predictions-missing-keybinding}

Zed 要求在 `Editor && edit_prediction` 和 `Editor && edit_prediction_conflict` 上下文中，至少为 {#action editor::AcceptEditPrediction} 操作设置一个快捷键绑定（[了解更多](#edit-predictions-keybinding)）。

如果你之前在全局上下文中将默认快捷键绑定到了其他操作，你将无法预览或接受编辑预测。例如：

要解决这个问题，你可以为接受编辑预测指定自己的快捷键：

```json [keymap]
[
  // ...
  {
    "context": "Editor && edit_prediction_conflict",
    "bindings": {
      "alt-l": "editor::AcceptEditPrediction"
    }
  }
]
```

如果你想使用默认快捷键，可以通过将你的快捷键移至更具体的上下文或更改为其他内容来释放它。

## 禁用自动编辑预测

你可以通过不同级别禁用编辑预测的显示，包括完全关闭该功能。

或者，如果你将 Zed 设置为提供程序，请考虑[使用静默模式](#切换模式)。

### 在缓冲区上

若要在输入时不自动显示预测，请在 `settings.json` 中设置：

这会隐藏所有显示有预测可用的指示，无论您处于[何种显示模式](#切换模式)（仅在您使用 Zed 作为服务提供商时有效）。
不过，您仍可以通过执行 {#action editor::ShowEditPrediction} 或按下 {#kb editor::ShowEditPrediction} 来手动触发编辑预测。

### 针对特定语言

若要在处理特定语言时不自动显示输入预测，请在 `settings.json` 中设置：

```json [settings]
{
  "language": {
    "python": {
      "show_edit_predictions": false
    }
  }
}
```

### 在特定目录中

要禁用特定目录或文件的编辑预测，请在 `settings.json` 中设置：

```json [settings]
{
  "edit_predictions": {
    "disabled_globs": ["~/.config/zed/settings.json"]
  }
}
```

### 完全关闭

要完全关闭所有提供商的编辑预测功能，请将设置明确指定为 `none`，如下所示：

```json [settings]
"features": {
  "edit_prediction_provider": "none"
},
```

## 配置 GitHub Copilot {#github-copilot}

若要将 GitHub Copilot 设为您的提供商，请在 `settings.json` 中进行如下设置：

```json [settings]
{
  "features": {
    "edit_prediction_provider": "copilot"
  }
}
```

点击状态栏中的 Copilot 图标并按照设置说明操作，即可登录 GitHub Copilot。

### 使用 GitHub Copilot Enterprise {#github-copilot-enterprise}

如果您的组织使用 GitHub Copilot Enterprise，可以在 `settings.json` 中指定企业实例的 URI 来配置 Zed：

```json [settings]
{
  "edit_predictions": {
    "copilot": {
      "enterprise_uri": "https://your.enterprise.domain"
    }
  }
}
```

将 `"https://your.enterprise.domain"` 替换为您的 GitHub Enterprise 管理员提供的 URL（例如 `https://foo.ghe.com`）。

设置完成后，Zed 将通过您的企业端点路由 Copilot 请求。当您点击状态栏中的 Copilot 图标登录时，系统将重定向至您配置的企业 URL 以完成身份验证。其他所有 Copilot 功能和使用方式保持不变。

Copilot 可提供多个补全备选方案，您可通过以下操作进行导航：

- {#action editor::NextEditPrediction} ({#kb editor::NextEditPrediction})：切换到下一个编辑预测
- {#action editor::PreviousEditPrediction} ({#kb editor::PreviousEditPrediction})：切换到上一个编辑预测

## 配置 Supermaven {#supermaven}

如需使用 Supermaven 作为服务提供商，请在 `settings.json` 中设置：

```json [settings]
{
  "features": {
    "edit_prediction_provider": "supermaven"
  }
}
```

您可以通过点击状态栏中的 Supermaven 图标并按照设置说明登录 Supermaven。

## 相关功能

您也可以使用[智能助手面板](./agent-panel.md)或[行内助手](./inline-assistant.md)与语言模型交互。关于 Zed 中其他 AI 功能的更多信息，请参阅 [AI 功能文档](./overview.md)。