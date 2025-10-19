# 模型上下文协议

Zed 使用[模型上下文协议](https://modelcontextprotocol.io/)与上下文服务器进行交互。

> 模型上下文协议（MCP）是一个开放协议，能够实现 LLM 应用程序与外部数据源及工具之间的无缝集成。无论您是在构建 AI 驱动的 IDE、增强聊天界面，还是创建自定义 AI 工作流，MCP 都提供了一种标准化方式，将 LLM 与其所需的上下文连接起来。

请查阅 [Anthropic 新闻公告](https://www.anthropic.com/news/model-context-protocol)和 [Zed 博客文章](https://zed.dev/blog/mcp)了解 MCP 的总体介绍。

## 安装 MCP 服务器

### 作为扩展使用

在 Zed 中使用 MCP 服务器的一种方式是通过扩展形式进行暴露。
要了解如何创建自己的扩展，请查看 [MCP 服务器扩展](../extensions/mcp-extensions.md)页面获取详细信息。

感谢我们优秀的社区，许多MCP服务器已作为扩展程序添加。
您可以通过以下任一途径查看可用的扩展：

1. [Zed官网](https://zed.dev/extensions?filter=context-servers)
2. 在应用中打开命令面板并执行`zed: extensions`操作
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

此外，您也可以通过访问智能助手面板的设置界面（亦可通过 `agent: open settings` 操作进入），以自定义方式添加服务器。
进入设置界面后，点击"添加自定义服务器"按钮，即可通过弹出的对话框完成添加。

## 使用 MCP 服务器

### 配置检查

无论您是通过扩展安装还是直接添加 MCP 服务器，目前大多数服务器在设置过程中仍需进行某些配置。

对于服务器扩展，安装完成后，Zed 会弹出一个模态窗口，显示正确配置所需的步骤。  
例如，GitHub MCP 扩展需要您添加一个[个人访问令牌](https://docs.github.com/zh/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)。  

对于自定义服务器，请务必查阅提供商的文档，以确定需要在 JSON 中添加的命令类型、参数和环境变量。  

要检查您的 MCP 服务器是否配置正确，请前往代理面板的设置视图，并观察其名称旁边的指示点。  
如果服务器运行正常，指示点将显示为绿色，且提示信息为“服务器已激活”。  
如果运行异常，其他颜色和提示信息将说明当前状态。  

### 在代理面板中使用  

安装完成后，您可以返回代理面板并开始输入提示。

在选择从MCP服务器获取工具时，某些模型的表现会优于其他模型。直接提及您的服务器名称总能帮助模型准确调用对应工具。

若需确保特定MCP服务器被调用，您可以创建[自定义配置](./agent-panel.md#custom-profiles)，在此配置中关闭所有内置工具（或可能与服务器工具产生冲突的功能），仅保留来自该MCP服务器的工具。

例如，[Dagger团队建议](https://container-use.com/agent-integrations#zed)在使用他们的[Container Use MCP服务器](https://zed.dev/extensions/mcp-server-container-use)时采用此方案：

```json [settings]
"agent": {
  "profiles": {
    "container-use": {
      "name": "Container Use",
      "tools": {
        "fetch": true,
        "thinking": true,
        "copy_path": false,
        "find_path": false,
        "delete_path": false,
        "create_directory": false,
        "list_directory": false,
        "diagnostics": false,
        "read_file": false,
        "open": false,
        "move_path": false,
        "grep": false,
        "edit_file": false,
        "terminal": false
      },
      "enable_all_context_servers": false,
      "context_servers": {
        "container-use": {
          "tools": {
            "environment_create": true,
            "environment_add_service": true,
            "environment_update": true,
            "environment_run_cmd": true,
            "environment_open": true,
            "environment_file_write": true,
            "environment_file_read": true,
            "environment_file_list": true,
            "environment_file_delete": true,
            "environment_checkpoint": true
          }
        }
      }
    }
  }
}
```

### 工具使用授权

Zed的智能体面板包含`agent.always_allow_tool_actions`设置项，若将其设为`false`，则任何编辑尝试及来自MCP服务器的工具调用都需要获得您的明确许可。

您可以通过以下两种方式修改此设置：在`settings.json`中将该键值设为`true`，或直接在智能体面板的设置界面中进行调整。