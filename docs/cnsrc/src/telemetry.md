# Zed 中的遥测数据

Zed 会收集匿名遥测数据，以帮助团队了解用户如何使用该应用程序，并发现他们遇到的各种问题。

## 配置遥测设置

您可以完全控制 Zed 发送的数据内容。  
如需启用或禁用部分或全部遥测类型，请通过命令面板中的 {#action zed::OpenSettings}({#kb zed::OpenSettings}) 打开 `settings.json` 文件。

插入并调整以下内容：

```json [settings]
"telemetry": {
    "diagnostics": false,
    "metrics": false
},
```

## 数据流

遥测数据会从应用程序发送到我们的服务器。数据通过我们的服务器进行代理，以便我们能够轻松切换分析服务。目前我们使用的服务包括：

- [Sentry](https://sentry.io)：崩溃监控服务——存储诊断事件
- [Snowflake](https://snowflake.com)：数据仓库——同时存储诊断事件与指标事件
- [Hex](https://www.hex.tech)：仪表板与数据探索平台——访问存储于Snowflake的数据
- [Amplitude](https://www.amplitude.com)：仪表板与数据探索平台——访问存储于Snowflake的数据

## 遥测数据类型

### 诊断信息

崩溃报告由[小型转储文件](https://learn.microsoft.com/zh-cn/windows/win32/debug/minidump-files)和额外调试信息构成。这些报告将在崩溃发生后的首次应用启动时发送。我们构建的仪表板能直观展示用户遇到问题的频率与严重程度。通过自动发送这些报告，我们可以在用户无需提交问题跟踪报告的情况下着手修复问题。仪表板中的图表还为我们提供了衡量Zed稳定性的直观参考。

你可以在 Zed 代码库的 [crates/telemetry_events/src/telemetry_events.rs](https://github.com/zed-industries/zed/blob/main/crates/telemetry_events/src/telemetry_events.rs) 文件中查看 `Panic` 结构体，了解与小型转储文件一同发送的额外数据。更多信息可查阅 [调试崩溃问题](./development/debugging-crashes.md) 文档。

### 客户端使用数据 {#client-metrics}

为改进 Zed 并了解其实际使用情况，Zed 会选择性收集以下使用数据：

- (a) 已打开文件的扩展名；
- (b) 您在编辑器中使用的功能与工具；
- (c) 项目统计信息（例如文件数量）；
- (d) 在您项目中检测到的开发框架

使用数据不包含您的软件代码或敏感项目细节。指标事件通过 HTTPS 协议上报，且请求会进行速率限制以避免占用过多网络带宽。

使用数据与一个安全的随机遥测ID相关联，该ID可能与您的电子邮件地址关联。目前这种关联有两个目的：（1）在保护您隐私的前提下，允许Zed分析长期使用模式；（2）使Zed能够联系特定用户群体获取反馈和改进建议。

您可以通过命令面板运行命令{#action zed::OpenTelemetryLog}，或点击应用菜单中的`Help > View Telemetry Log`来审核Zed上报的指标数据。

您可以在Zed代码库的[crates/telemetry_events/src/telemetry_events.rs](https://github.com/zed-industries/zed/blob/main/crates/telemetry_events/src/telemetry_events.rs)文件中查看完整的`Event`枚举类型及关联数据结构，从而了解所有事件类型及具体发送的数据内容。

### 服务端使用数据 {#metrics}

在使用Zed托管服务时，我们可能收集、生成和处理数据，以便为用户提供支持并优化托管服务。例如包括与速率限制相关的元数据以及计费指标/令牌使用情况。除非用户明确与Zed共享数据，并且我们与Anthropic签订了零数据保留协议，否则Zed不会持久存储用户内容，也不会使用用户内容来评估和/或改进我们的AI功能。

您可以通过[AI改进](./ai/ai-improvement.md)了解我们对数据收集的立场（所有与Zed共享的提示数据均需明确选择加入）。

## 疑虑与疑问

若对遥测数据存在疑虑，欢迎随时[提交问题](https://github.com/zed-industries/zed/issues/new/choose)。