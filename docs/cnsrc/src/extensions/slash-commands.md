# 斜杠命令

扩展可以为助手提供斜杠命令功能。

## 示例扩展

如需查看提供斜杠命令功能的实际扩展示例，请参阅 [`slash-commands-example` 扩展](https://github.com/zed-industries/zed/tree/main/extensions/slash-commands-example)。

若想亲自试用，您可以将此扩展[作为开发扩展安装](./developing-extensions.md#developing-an-extension-locally)。

## 定义斜杠命令

单个扩展可以提供一个或多个斜杠命令。每个斜杠命令都必须在 `extension.toml` 中注册。

例如，以下是一个提供两个斜杠命令的扩展：`/echo` 和 `/pick-one`：

```toml
[slash_commands.echo]
description = "echoes the provided input"
requires_argument = true

[slash_commands.pick-one]
description = "pick one of three options"
requires_argument = true
```

每个斜杠命令可以定义以下属性：

- `description`：描述斜杠命令的文本，将在显示可用命令时展示。
- `requires_argument`：指示斜杠命令是否需要至少一个参数才能运行。

## 实现斜杠命令行为

要为斜杠命令实现具体行为，请为您的扩展程序实现`run_slash_command`方法。

该方法接收待执行的斜杠命令、传递给命令的参数列表，以及一个可选的`Worktree`参数。

该方法返回`SlashCommandOutput`对象，其中`text`字段包含命令的文本输出。输出还可以定义包含输出文本范围的`SlashCommandOutputSection`区块，这些区块将在助手上下文编辑器中呈现为可折叠区域。

您的扩展程序应当根据命令名称（不包含前导`/`符号）执行`match`操作，并据此执行相应行为：

```rs
为 MyExtension 实现 zed::Extension {
    运行斜杠命令(
        &自身,
        命令: 斜杠命令,
        参数: 向量<String>,
        _工作树: 可选<&Worktree>,
    ) -> 结果<SlashCommandOutput, String> {
        匹配 命令.名称.作为字符串() {
            "echo" => {
                如果 参数.是空的() {
                    返回 错误("没有内容可回显".转换为字符串());
                }

                让 文本 = 参数.连接(" ");

                确定(斜杠命令输出 {
                    部分: 向量![斜杠命令输出部分 {
                        范围: (0..文本.长度()).转换(),
                        标签: "回显".转换为字符串(),
                    }],
                    文本,
                })
            }
            "pick-one" => {
                让 某(选择项) = 参数.首个() 否则 {
                    返回 错误("未选择任何选项".转换为字符串());
                };

匹配选择项字符串时：
    "选项1" | "选项2" | "选项3" => {}
    无效选项 => {
        返回错误(格式化!("{无效选项} 不是有效选项"));
    }

让文本 = 格式化!("您选择了 {选择项}。");

确定(斜杠命令输出 {
    部分: 向量![斜杠命令输出部分 {
        范围: (0..文本.长度()).转换(),
        标签: 格式化!("选择一项: {选择项}"),
    }],
    文本,
})
}
命令 => 错误(格式化!("未知斜杠命令: \"{命令}\"")),
}
}
}
```

## Auto-completing slash command arguments

For slash commands that have arguments, you may also choose to implement `完成斜杠命令参数` 来为您的斜杠命令提供补全建议。

该方法接收将要运行的斜杠命令及其参数列表，并返回将在自动补全菜单中显示的`SlashCommandArgumentCompletion`列表。

每个`SlashCommandArgumentCompletion`包含以下属性：

- `label`：显示在补全菜单中的标签文本
- `new_text`：接受补全时将被插入的文本内容
- `run_command`：布尔值，表示接受补全时是否执行该斜杠命令

需要特别注意的是，您的扩展程序应当基于命令名称（不包含前导`/`）进行`match`处理，并返回所需的参数补全结果：

```rs
impl zed::Extension for MyExtension {
    fn complete_slash_command_argument(
        &self,
        command: SlashCommand,
        _args: Vec<String>,
    ) -> Result<Vec<SlashCommandArgumentCompletion>, String> {
        match command.name.as_str() {
            "echo" => Ok(vec![]),
            "pick-one" => Ok(vec![
                SlashCommandArgumentCompletion {
                    label: "Option One".to_string(),
                    new_text: "option-1".to_string(),
                    run_command: true,
                },
                SlashCommandArgumentCompletion {
                    label: "Option Two".to_string(),
                    new_text: "option-2".to_string(),
                    run_command: true,
                },
                SlashCommandArgumentCompletion {
                    label: "Option Three".to_string(),
                    new_text: "option-3".to_string(),
                    run_command: true,
                },
            ]),
            command => Err(format!("unknown slash command: \"{command}\"")),
        }
    }
}
```