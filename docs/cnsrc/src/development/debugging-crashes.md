# 调试崩溃问题

当 Zed 出现异常或崩溃时，系统会向一个辅助进程发送消息，该进程会检查崩溃编辑器的内存状态，并在 `~/Library/Logs/Zed` 或 `$XDG_DATA_HOME/zed/logs` 目录下生成 [minidump](https://chromium.googlesource.com/breakpad/breakpad/+/master/docs/getting_started_with_breakpad.md#the-minidump-file-format) 文件。该小型转储文件可用于生成所有线程堆栈的回溯信息。

若您已启用 Zed 的遥测功能，这些文件将在重启应用时自动上传至我们的服务器。它们最终会出现在 [Slack 频道](https://zed-industries.slack.com/archives/C0977J9MA1Y)和 [Sentry 平台](https://zed-dev.sentry.io/issues)中（这两个平台仅限 Zed 内部员工访问）。

这些崩溃报告包含丰富的信息，但由于缺少代码范围和符号信息，阅读起来较为困难。您仍可通过以下方式在本地进行分析：下载对应 Zed 版本的源代码和未剥离符号的二进制文件（或独立的符号文件），然后运行：

在你的日志目录中，除了小型转储文件外，还应该有一个 `<uuid>.json` 文件，其中包含额外的元数据，如恐慌消息、代码范围和系统规格。

## 使用调试器

如果你能稳定复现崩溃，可以使用调试器来检查程序在崩溃时的状态，这通常能提供有关崩溃原因的有用信息。

你可以在此处阅读更多关于为 Zed 设置和使用调试器，特别是用于调试崩溃的信息：[此处](./debuggers.md#debugging-panics-and-crashes)