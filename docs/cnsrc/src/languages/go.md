# Go

Zed 原生支持 Go 语言。

- 语法分析器：[tree-sitter/tree-sitter-go](https://github.com/tree-sitter/tree-sitter-go)
- 语言服务器：[golang/tools/tree/master/gopls](https://github.com/golang/tools/tree/master/gopls)
- 调试适配器：[delve](https://github.com/go-delve/delve)

## 环境配置

建议通过 Go 的包管理器安装 gopls，而非使用 Homebrew 或 Linux 发行版的包管理器。

1. 请确保已卸载通过包管理器安装的所有 gopls 版本：

```sh
# MacOS homebrew
brew remove gopls
# Ubuntu
sudo apt-get remove gopls
sudo snap remove gopls
# Arch
sudo pacman -R gopls
```

2. 使用 Go 模块工具安装/更新 `gopls` 至最新版本：

```sh
go install golang.org/x/tools/gopls@latest
```

3. 确保 `gopls` 位于您的环境路径中：

```sh
which gopls
gopls version
```

若系统提示找不到 `gopls`，您可能需要将 `export PATH="$PATH:$HOME/go/bin"` 添加到 `.zshrc` / `.bash_profile` 中

## 嵌入提示功能

Zed 为嵌入提示设置了以下初始化选项：

```json [settings]
"hints": {
    "assignVariableTypes": true,
    "compositeLiteralFields": true,
    "compositeLiteralTypes": true,
    "constantValues": true,
    "functionTypeParameters": true,
    "parameterNames": true,
    "rangeVariableTypes": true
}
```

当用户在设置中启用 Zed 的嵌入提示功能时，该配置将使语言服务器返回对应的嵌入提示信息。

如需自定义这些设置，可使用：

```json [settings]
"lsp": {
    "gopls": {
        "initialization_options": {
            "hints": {
                // ....
            }
        }
    }
}
```

更多详细信息请参阅 [gopls 嵌入提示文档](https://github.com/golang/tools/blob/master/gopls/doc/inlayHints.md)。

## 调试功能

Zed 支持通过 Delve 实现零配置调试 Go 测试及入口点（`func main`）。执行 {#action debugger::Start}（快捷键 {#kb debugger::Start}）即可查看根据上下文自动生成的预配置调试任务列表。

如需更多控制，你可以向 `.zed/debug.json` 添加调试配置。示例如下：

- [Delve 配置文档](https://github.com/go-delve/delve/blob/master/Documentation/api/dap/README.md#launch-and-attach-configurations)

### 调试 Go 包

要调试特定包，可将 Delve 模式设为 "debug" 实现。此时应将 "program" 设置为对应包名。

```json [debug]
[
  {
    "label": "Go (Delve)",
    "adapter": "Delve",
    "program": "$ZED_FILE",
    "request": "launch",
    "mode": "debug"
  },
  {
    "label": "Run server",
    "adapter": "Delve",
    "request": "launch",
    "mode": "debug",
    // For Delve, the program can be a package name
    "program": "./cmd/server"
    // "args": [],
    // "buildFlags": [],
  }
]
```

### 调试 Go 测试

要调试包的测试，请将 Delve 模式设为 "test"。
此时 "program" 仍为包名，您可以使用 "buildFlags" 设置构建标签，并通过 "args" 为测试二进制文件设置参数（具体操作方式详见 `go help testflags`）。

```json [debug]
[
  {
    "label": "Run integration tests",
    "adapter": "Delve",
    "request": "launch",
    "mode": "test",
    "program": ".",
    "buildFlags": ["-tags", "integration"]
    // To filter down to just the test your cursor is in:
    // "args": ["-test.run", "$ZED_SYMBOL"]
  }
]
```

### 分别构建与调试

若需使用特定命令构建应用程序，可采用 Delve 的 "exec" 模式。此时 "program" 应指向可执行文件，且 "build" 命令需用于构建该可执行文件。

### 连接到现有的 Delve 实例

您可能会遇到需要连接到并非运行在本地机器上的现有 Delve 实例的情况；此时，可以使用 `tcp_arguments` 来配置 Zed 与 Delve 的连接。

在这种情况下，Zed 不会启动新的 Delve 实例，而是选择使用现有的一个。这样做的结果是，Zed 中_将没有终端_；你必须直接与 Delve 实例交互，因为它负责处理被调试程序的标准输入/输出。

## Go 模块

- Tree-sitter：[camdencheek/tree-sitter-go-mod](https://github.com/camdencheek/tree-sitter-go-mod)
- 语言服务器：无

## Go 依赖校验和

- Tree-sitter：[amaanq/tree-sitter-go-sum](https://github.com/amaanq/tree-sitter-go-sum)
- 语言服务器：无

## Go 工作区

- Tree-sitter:
  [tree-sitter-go-work](https://github.com/d1y/tree-sitter-go-work)
- 语言服务器: 暂无