# MCP 服务器扩展

[模型上下文协议服务器](../ai/mcp.md)可作为扩展在智能体面板中使用。

## 定义 MCP 扩展

一个扩展可提供一个或多个 MCP 服务器。
每个 MCP 服务器必须在 `extension.toml` 中注册：

```toml
[context_servers.my-context-server]
```

然后，在扩展的 Rust 代码中，为扩展实现 `context_server_command` 方法：

```rust
impl zed::Extension for MyExtension {
    fn context_server_command(
        &mut self,
        context_server_id: &ContextServerId,
        project: &zed::Project,
    ) -> Result<zed::Command> {
        Ok(zed::Command {
            command: get_path_to_context_server_executable()?,
            args: get_args_for_context_server()?,
            env: get_env_for_context_server()?,
        })
    }
}
```

该方法应返回启动 MCP 服务器的命令，以及使其正常运行所需的任何参数或环境变量。

若需从外部来源（如 GitHub Releases 或 npm）下载 MCP 服务器，您也可以在此函数中完成操作。

## 可用扩展

请访问 [Zed 官网](https://zed.dev/extensions?filter=context-servers)查看所有已发布为扩展的 MCP 服务器。

我们建议查阅这些服务器的代码仓库，以便了解其常规创建方式与结构设计。

## 测试验证

如需测试新开发的 MCP 服务器扩展，您可以[将其作为开发扩展进行安装](./developing-extensions.md#developing-an-extension-locally)。