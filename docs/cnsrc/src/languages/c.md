# C

Zed 原生支持 C 语言。

- 语法分析器：[tree-sitter/tree-sitter-c](https://github.com/tree-sitter/tree-sitter-c)
- 语言服务器：[clangd/clangd](https://github.com/clangd/clangd)
- 调试适配器：[CodeLLDB](https://github.com/vadimcn)（主选），[GDB](https://sourceware.org/gdb/)（备选，不适用于苹果芯片）

## Clangd：强制识别为 C 语言

Clangd 默认会识别 C++/C 混合项目。如果你的项目仅包含 C 语言代码，可以通过 `-xc` 参数指示 clangd 将所有文件视为 C 语言文件。具体操作方式是在项目根目录创建 `.clangd` 文件并写入以下内容：

```yaml
CompileFlags:
  Add: [-xc]
```

默认情况下 clang 和 gcc 会将 `*.C` 与 `*.H`（大写扩展名）识别为 C++ 而非 C 语言，Zed 也遵循这一惯例。若你正在处理纯 C 语言项目（例如包含传统大写路径格式 `FILENAME.C` 的项目），可通过在设置中添加以下配置来覆盖此行为：

## 格式化

默认情况下，Zed 将使用 `clangd` 语言服务器来格式化 C 代码。Clangd 与 `clang-format` 命令行工具相同。要配置此功能，您可以添加一个 `.clang-format` 文件。例如：

```yaml
---
BasedOnStyle: GNU
IndentWidth: 2
---
```

有关完整选项列表，请参阅 [Clang-Format 样式选项](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)。

您可以通过 {#kb editor::Format} 或命令面板中的 `editor: format` 操作来触发格式化，或者将 `format_on_save` 添加到您的 Zed 设置中：

```json [settings]
  "languages": {
    "C": {
      "format_on_save": "on",
      "tab_size": 2
    }
  }
```

## 编译命令

对于某些项目，Clangd 需要一个 `compile_commands.json` 文件来正确分析您的项目。该文件包含编译数据库，告诉 clangd 如何构建您的项目。

### CMake 编译命令

使用 CMake 时，您可以通过在 [[代码块_2]] 中添加以下行来自动生成 [[代码块_1]]：

[[代码块_0]]

项目构建完成后，CMake 将在构建目录中生成 [[代码块_3]] 文件，clangd 会自动识别该文件。

## 调试

您可以使用 CodeLLDB 或 GDB 调试原生二进制文件。（请确保构建过程向 C 编译器传递 [[代码块_4]] 参数，以便在生成的二进制文件中包含调试信息。）以下示例展示了可添加到 [[代码块_5]] 中的调试配置：

- [CodeLLDB 配置文档](https://github.com/vadimcn/codelldb/blob/master/MANUAL.md#starting-a-new-debug-session)
- [GDB 配置文档](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Debugger-Adapter-Protocol.html)

### 构建与调试二进制文件

[[代码块_0]]