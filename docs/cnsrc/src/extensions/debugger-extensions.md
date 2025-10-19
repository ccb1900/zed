# 调试器扩展

[调试适配器协议](https://microsoft.github.io/debug-adapter-protocol) 服务器可作为扩展在[调试器](../debugger.md)中使用。

## 定义调试器扩展

一个扩展可提供一个或多个 DAP 服务器。
每个 DAP 服务器必须在 `extension.toml` 中注册：

```toml
[debug_adapters.my-debug-adapter]
# Optional relative path to the JSON schema for the debug adapter configuration schema. Defaults to `debug_adapter_schemas/$DEBUG_ADAPTER_NAME_ID.json`.
# Note that while this field is optional, a schema is mandatory.
schema_path = "relative/path/to/schema.json"
```

接着，在扩展的 Rust 代码中，实现 `get_dap_binary` 方法：

此方法应返回启动调试适配器协议服务器的命令，以及使其正常运行所需的任何参数或环境变量。

若需从外部源（如GitHub Releases或npm）下载DAP服务器，也可在此函数中完成。请确保仅定期检查更新，因为每当用户使用您的调试适配器启动新调试会话时，都会调用此函数。

您还必须实现`dap_request_kind`。该函数用于判断特定调试场景是需要_启动_新的被调试程序，还是需要_附加_到现有进程。
我们还通过它来确认某个调试场景是否需要运行_定位器_。

这两个函数足以在基于`debug.json`的用户工作流中公开您的调试适配器，但您应强烈考虑同时实现`dap_config_to_scenario`。

当用户通过新建进程模态界面启动会话时，会使用`dap_config_to_scenario`。其核心功能是接收通用调试配置（不特定于任何调试适配器），并将其转化为适用于当前适配器的具体调试方案。
换言之，它旨在解决这个问题：“给定程序路径、参数列表、工作目录和环境变量，生成对应调试适配器的配置应该是怎样的？”

## 调试定位器定义

Zed通过_调试定位器_提供自动化创建调试方案的机制。
定位器会识别调试目标，并确定如何启动对应的调试会话。借助定位器，我们可以自动转换现有用户任务（例如`cargo run`），将其转化为调试方案（例如先执行`cargo build`，再以`target/debug/my_program`作为待调试程序启动调试器）。

> 即使您的扩展程序未提供调试适配器，也可以定义专属的调试定位器。若您的扩展已支持语言任务，我们强烈建议您实现此功能——这能让用户无需手动配置调试适配器即可启动调试会话。

定位器可与调试适配器解耦（但非强制），其核心职责是定位调试目标并确定如何启动调试会话。这种设计能带来更灵活、可扩展的调试体验。

您的扩展可定义一个或多个调试定位器。每个定位器必须在 `extension.toml` 中注册：

```toml
[debug_locators.my-debug-locator]
```

定位器包含两个组成部分：
首先，系统会在所有可用任务上运行每个定位器，以判断是否有定位器能为指定任务提供调试方案。该过程通过调用 `dap_locator_create_scenario` 实现。

此函数应在调试场景定义了给定用户任务的调试对应项时，返回该调试场景。
请注意，调试场景可包含[构建任务](../debugger.md#build-tasks)。若存在构建任务，我们将在构建任务成功完成后执行该任务。

`run_dap_locator` 在无法确定构建目标时非常有用。某些构建系统可能会生成名称无法预先确定的产物。
但请注意，您**无需**进行两阶段解析——如果仅通过 `dap_locator_create_scenario` 即可确定完整调试配置，则可以省略返回的 `DebugScenario` 中的 `build` 属性。同时请注意，您的定位器**将会**接收到其不太可能接受的任务调用；因此您应在执行任何高成本操作前，优先返回 `None`。

## 可用扩展

请访问 [Zed 官网](https://zed.dev/extensions?filter=debug-adapters) 查看所有已发布为扩展的 DAP 服务器。

我们建议查阅这些扩展的代码仓库，以了解其常规创建方式与结构设计。

## 测试

要测试新的调试适配器协议服务器扩展，你可以[将其作为开发扩展进行安装](./developing-extensions.md#developing-an-extension-locally)。