# 模型上下文协议

Zed 使用[模型上下文协议](https://modelcontextprotocol.io/)与上下文服务器进行交互。

> 模型上下文协议（MCP）是一个开放协议，能够实现大型语言模型应用与外部数据源及工具之间的无缝集成。无论您是在构建AI驱动的集成开发环境、增强聊天界面，还是创建定制化AI工作流，MCP都提供了标准化方式，将大型语言模型与其所需上下文连接起来。

请查阅[Anthropic新闻公告](https://www.anthropic.com/news/model-context-protocol)和[Zed博客文章](https://zed.dev/blog/mcp)了解MCP的总体介绍。

## 安装MCP服务器

### 作为扩展使用

在Zed中使用MCP服务器的方式之一是通过扩展形式进行部署。
若想了解如何创建自己的MCP服务器，请查看[MCP服务器扩展](../extensions/mcp-extensions.md)页面获取详细信息。

多亏了我们优秀的社区，许多MCP服务器已作为扩展程序添加。
您可以通过以下任一途径查看可用的扩展：

1. [Zed官网](https://zed.dev/extensions?filter=context-servers)
2. 在应用中打开命令面板，执行`zed: extensions`操作
3. 在应用中进入智能助手面板的右上角菜单，查找“查看服务器扩展”菜单项

以下是部分可用扩展示例：

- [Context7](https://zed.dev/extensions/context7-mcp-server)
- [GitHub](https://zed.dev/extensions/github-mcp-server)
- [Puppeteer](https://zed.dev/extensions/puppeteer-mcp-server)
- [Gem](https://zed.dev/extensions/gem)
- [Brave搜索](https://zed.dev/extensions/brave-search-mcp-server)
- [Prisma](https://github.com/aqrln/prisma-mcp-zed)
- [Framelink Figma](https://zed.dev/extensions/framelink-figma-mcp-server)
- [Linear](https://zed.dev/extensions/linear-mcp-server)
- [Resend](https://zed.dev/extensions/resend-mcp-server)

### 作为自定义服务器

在 Zed 中使用 MCP 服务器并非只有创建扩展这一种方式。
您可以直接将服务器命令添加到 `settings.json` 来建立连接，具体操作如下：

```json [settings]
{
  "context_servers": {
    "your-mcp-server": {
      "source": "custom",
      "command": "some-command",
      "args": ["arg-1", "arg-2"],
      "env": {}
    }
  }
}
```

此外，您也可以通过访问智能助手面板的设置界面（亦可通过 `agent: open settings` 操作进入），点击“添加自定义服务器”按钮，在出现的弹窗中完成添加。

## 使用 MCP 服务器

### 配置检查

无论您是通过扩展安装还是直接添加 MCP 服务器，目前大多数服务器在设置过程中仍需进行某些配置。

对于服务器扩展，安装后 Zed 会弹出模态窗口，显示正确配置所需的内容。
例如 GitHub MCP 扩展需要您添加[个人访问令牌](https://docs.github.com/zh/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)。

对于自定义服务器，请务必查阅供应商文档，确定需要在 JSON 中添加的命令类型、参数和环境变量。

要检查 MCP 服务器是否配置正确，请前往智能体面板的设置视图，观察服务器名称旁的指示灯。
若运行正常，指示灯将显示绿色，悬停提示会显示"服务器已激活"。
若出现异常，不同颜色和提示信息将说明当前状态。

### 在智能体面板中使用

安装完成后，您可以返回智能体面板开始输入指令。

在选择从MCP服务器获取工具时，某些模型的表现会优于其他模型。直接提及您的服务器名称通常有助于模型准确调用。

但若需确保特定MCP服务器被调用，您可以通过[创建自定义配置](./agent-panel.md#custom-profiles)来实现：关闭所有内置工具（或可能与服务器工具冲突的功能），仅启用来自目标MCP服务器的工具。

例如，[Dagger团队建议](https://container-use.com/agent-integrations#zed)在使用其[Container Use MCP服务器](https://zed.dev/extensions/mcp-server-container-use)时采用此方案：

[[代码块_0]]

### 工具使用授权

Zed 智能体面板包含 `agent.always_allow_tool_actions` 设置项，若将其设为 `false`，则任何编辑尝试及来自 MCP 服务器的工具调用都需要获得您的授权许可。

您可以通过以下两种方式修改此设置：在 `settings.json` 中将该键值设为 `true`，或直接在智能体面板的设置界面中调整。