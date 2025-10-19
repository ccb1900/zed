# C

Zed 原生支持 C 语言。

- 语法分析器：[tree-sitter/tree-sitter-c](https://github.com/tree-sitter/tree-sitter-c)
- 语言服务器：[clangd/clangd](https://github.com/clangd/clangd)
- 调试适配器：[CodeLLDB](https://github.com/vadimcn)（主要）、[GDB](https://sourceware.org/gdb/)（次要，不适用于 Apple 芯片）

## Clangd：强制检测为 C 语言

Clangd 默认会假设项目混合使用 C++ 和 C。如果你的项目仅使用 C 语言，可以通过 `-xc` 标志指示 clangd 将所有文件视为 C 语言文件。具体操作是在项目根目录创建 `.clangd` 文件，并填入以下内容：

```yaml
CompileFlags:
  Add: [-xc]
```

默认情况下，clang 和 gcc 会将 `*.C` 和 `*.H`（大写扩展名）识别为 C++ 而非 C 语言，因此 Zed 也遵循这一惯例。如果你正在处理纯 C 语言项目（例如包含传统大写路径如 `FILENAME.C` 的项目），可以通过在设置中添加以下配置来覆盖此行为：

```json [settings]
{
  "file_types": {
    "C": ["C", "H"]
  }
}
```

## 代码格式化

默认情况下，Zed 会使用 `clangd` 语言服务器来格式化 C 代码。Clangd 与 `clang-format` 命令行工具相同。要配置此功能，您可以添加 `.clang-format` 文件。例如：

```yaml
---
BasedOnStyle: GNU
IndentWidth: 2
---
```

完整选项列表请参阅 [Clang-Format 样式选项](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)。

您可以通过快捷键 {#kb editor::Format}、命令面板中的 `editor: format` 操作，或在 Zed 设置中添加 `format_on_save` 来触发格式化：

```json [settings]
  "languages": {
    "C": {
      "format_on_save": "on",
      "tab_size": 2
    }
  }
```

## 编译命令

对于某些项目，Clangd 需要 `compile_commands.json` 文件才能正确分析您的项目。该文件包含编译数据库，用于告知 clangd 项目的构建方式。

### CMake 编译命令

使用CMake时，只需在`CMakeLists.txt`中添加以下行即可自动生成`compile_commands.json`：

```cmake
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```

项目构建完成后，CMake会在构建目录中生成`compile_commands.json`文件，clangd将自动识别该文件。

## 调试

您可以使用CodeLLDB或GDB调试原生二进制文件。（请确保构建过程向C编译器传递`-g`参数，以便在生成的二进制文件中包含调试信息。）下方提供了可添加到`.zed/debug.json`的调试配置示例：

- [CodeLLDB配置文档](https://github.com/vadimcn/codelldb/blob/master/MANUAL.md#starting-a-new-debug-session)
- [GDB配置文档](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Debugger-Adapter-Protocol.html)

### 构建与调试二进制文件

```json [debug]
[
  {
    "label": "Debug native binary",
    "build": {
      "command": "make",
      "args": ["-j8"],
      "cwd": "$ZED_WORKTREE_ROOT"
    },
    "program": "$ZED_WORKTREE_ROOT/build/prog",
    "request": "launch",
    "adapter": "CodeLLDB"
  }
]
```