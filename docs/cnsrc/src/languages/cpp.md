# C++

Zed 原生支持 C++ 语言。

- 语法分析器：[tree-sitter/tree-sitter-cpp](https://github.com/tree-sitter/tree-sitter-cpp)
- 语言服务器：[clangd/clangd](https://github.com/clangd/clangd)

## 二进制文件

您可以配置 Zed 应使用哪个 `clangd` 二进制文件。

默认情况下，Zed 会尝试在您的 `$PATH` 中查找 `clangd` 二进制文件。如果该二进制文件成功执行，则会被采用。否则，Zed 将回退至安装自带的 `clangd` 版本并使用。

若希望安装预发布版本的 `clangd`，您可以通过在 `settings.json` 中将 `pre_release` 设置为 `true` 来指示 Zed：

```json [settings]
{
  "lsp": {
    "clangd": {
      "fetch": {
        "pre_release": true
      }
    }
  }
}
```

如需禁用 Zed 查找 `clangd` 二进制文件的功能，可在 `settings.json` 中将 `ignore_system_version` 设置为 `true`：

如果你想使用自定义位置的二进制文件，可以指定`path`和可选的`arguments`：

```json [settings]
{
  "lsp": {
    "clangd": {
      "binary": {
        "path": "/path/to/clangd",
        "arguments": []
      }
    }
  }
}
```

此`"path"`必须是绝对路径。

## 参数

你可以向clangd传递任意数量的参数。要查看完整的可用选项列表，请在命令行中运行`clangd --help`。例如，使用`--function-arg-placeholders=0`时，补全内容仅包含函数调用的括号，而默认设置（`--function-arg-placeholders=1`）的补全内容还会包含方法参数的占位符。

```json [settings]
{
  "lsp": {
    "clangd": {
      "binary": {
        "path": "/path/to/clangd",
        "arguments": ["--function-arg-placeholders=0"]
      }
    }
  }
}
```

## 格式化

默认情况下，Zed 将使用 `clangd` 语言服务器来格式化 C++ 代码。Clangd 与 `clang-format` 命令行工具功能相同。您可以通过创建 `.clang-format` 文件进行配置，例如：

```yaml
---
BasedOnStyle: LLVM
IndentWidth: 4
---
Language: Cpp
# Force pointers to the type for C++.
DerivePointerAlignment: false
PointerAlignment: Left
---
```

完整配置选项请参阅 [Clang-Format 样式选项](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)。

您可以通过以下方式触发代码格式化：
- 使用快捷键 {#kb editor::Format}
- 在命令面板中执行 `editor: format` 操作
- 在 Zed 设置中添加 `format_on_save`：

```json [settings]
  "languages": {
    "C++": {
      "format_on_save": "on",
      "tab_size": 2
    }
  }
```

## 更多服务器配置

通常建议在项目根目录创建 `.clangd` 文件来设置额外配置。

```text
CompileFlags:
  Add:
    - "--include-directory=/path/to/include"
Diagnostics:
  MissingIncludes: Strict
  UnusedIncludes: Strict
```

关于clangd配置文件的更高级用法，请查阅其[官方页面](https://clangd.llvm.org/config.html)。

## 编译命令

对于某些项目，Clangd需要`compile_commands.json`文件才能正确分析您的项目。该文件包含编译数据库，用于告知clangd项目的构建方式。

### CMake编译命令

使用CMake时，您可以通过在`CMakeLists.txt`中添加以下行来自动生成`compile_commands.json`：

```cmake
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```

构建项目后，CMake将在构建目录中生成`compile_commands.json`文件，clangd将自动识别该文件。

## 调试

你可以使用 CodeLLDB 或 GDB 调试原生二进制文件。（请确保在构建过程中向 C++ 编译器传递 `-g`，以便在生成的二进制文件中包含调试信息。）下方提供了可添加到 `.zed/debug.json` 的调试配置示例。

- [CodeLLDB 配置文档](https://github.com/vadimcn/codelldb/blob/master/MANUAL.md#starting-a-new-debug-session)
- [GDB 配置文档](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Debugger-Adapter-Protocol.html)
  - GDB 版本需不低于 v14.1

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