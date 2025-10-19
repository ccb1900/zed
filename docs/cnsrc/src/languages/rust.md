# Rust

Zed 原生支持 Rust 语言。

- 语法分析器：[tree-sitter/tree-sitter-rust](https://github.com/tree-sitter/tree-sitter-rust)
- 语言服务器：[rust-lang/rust-analyzer](https://github.com/rust-lang/rust-analyzer)
- 调试适配器：[CodeLLDB](https://github.com/vadimcn/codelldb)（主选），[GDB](https://sourceware.org/gdb/)（备选，不适用于 Apple 芯片）

<!--
TBD: Polish Rust Docs. Zed is a good rust editor, good Rust docs make it look like we care about Rust (we do!)
TBD: Users may not know what inlayHints, don't start there.
TBD: Provide explicit examples not just [[CODE_BLOCK_0]]
-->

## 内联提示

以下配置可用于更改 Rust 中 `rust-analyzer` 的内联提示设置：

请参阅 [嵌入提示](https://rust-analyzer.github.io/book/features.html#inlay-hints) 以获取更多信息。

## 目标目录

可以在 `initialization_options` 中设置 `rust-analyzer` 目标目录：

```json [settings]
{
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        "rust": {
          "analyzerTargetDir": true
        }
      }
    }
  }
}
```

将 `true` 设置为 `true` 会将目标目录指定为 `target/rust-analyzer`。您也可以通过字符串（例如 `"target/analyzer"`）来自定义目录路径，而非使用 `true`。

## 二进制文件

你可以配置 Zed 应使用哪个 `rust-analyzer` 二进制文件。

默认情况下，Zed 会尝试在你的 `$PATH` 中查找 `rust-analyzer` 并使用它。如果该二进制文件成功执行 `rust-analyzer --help`，则会被采用。否则，Zed 将回退到安装其自带的稳定 `rust-analyzer` 版本并使用。

如果你想安装预发布的 `rust-analyzer` 版本，可以在 `settings.json` 中将 `pre_release` 设置为 `true` 来指示 Zed 这样做：

```json [settings]
{
  "lsp": {
    "rust-analyzer": {
      "fetch": {
        "pre_release": true
      }
    }
  }
}
```

如果你想禁用 Zed 查找 `rust-analyzer` 二进制文件，可以在 `settings.json` 中将 `ignore_system_version` 设置为 `true`：

```json [settings]
{
  "lsp": {
    "rust-analyzer": {
      "binary": {
        "ignore_system_version": true
      }
    }
  }
}
```

如果你想使用自定义位置的二进制文件，可以指定 `path` 和可选的 `arguments`：

这个 `"path"` 必须是绝对路径。

## 备用目标平台

若希望 rust-analyzer 为当前平台之外的目标平台提供诊断（例如在 macOS 上运行时诊断 Windows 目标），可使用以下 Zed LSP 设置：

```json [settings]
{
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        "cargo": {
          "target": "x86_64-pc-windows-msvc"
        }
      }
    }
  }
}
```

若您正在使用 `rustup`，可通过运行以下命令获取可用目标三元组列表（`aarch64-apple-darwin`、`x86_64-unknown-linux-gnu` 等）：

```sh
rustup target list --installed
```

## LSP 任务

Zed 使用 tree-sitter 提供任务功能，但 rust-analyzer 通过 LSP 扩展方法支持查询文件相关任务。
此功能默认启用，可通过以下方式配置：

## 手动获取 Cargo 诊断信息

默认情况下，rust-analyzer 启用了 `checkOnSave: true` 功能，这会导致每次保存缓冲区时都会触发 `cargo check --workspace --all-targets` 命令。
如果通过 `checkOnSave: false` 禁用此功能（参见上方服务器配置 JSON 示例），仍然可以通过在 Rust 文件中使用 `editor: run/clear/cancel flycheck` 命令来手动获取诊断信息；当启用该设置时，项目诊断编辑器也会通过 `editor: run flycheck` 命令刷新 cargo 诊断信息。

## 更多服务器配置

<!--
TBD: Is it possible to specify RUSTFLAGS? https://github.com/zed-industries/zed/issues/14334
-->

Rust-analyzer [手册](https://rust-analyzer.github.io/book/) 详细介绍了 rust-analyzer 语言服务器的各种功能和配置选项。
Zed 中的 Rust-analyzer 使用默认参数运行。

### 大型项目与性能考量

在大型项目中可能导致资源过度消耗的一个主要注意事项，是以下功能的组合使用：

```
rust-analyzer.checkOnSave (default: true)
    Run the check command for diagnostics on save.
```

```
rust-analyzer.check.workspace (default: true)
    Whether --workspace should be passed to cargo check. If false, -p <package> will be passed instead.
```

```
rust-analyzer.cargo.allTargets (default: true)
    Pass --all-targets to cargo invocation
```

这意味着每次 Zed 保存时，都会执行 `cargo check --workspace --all-targets` 命令，对整个项目（工作区）、lib、doc、test、bin、bench 及[其他构建目标](https://doc.rust-lang.org/cargo/reference/cargo-targets.html)进行全面检查。

虽然这在小型项目中运行良好，但随着项目规模扩大，这种机制将难以保持高效。

替代方案是使用[任务](../tasks.md)，因为Zed已提供`cargo check --workspace --all-targets`任务功能，支持通过cmd/ctrl点击终端输出跳转至错误位置，同时可限制或完全关闭保存时检查功能。

保存时检查功能负责基于cargo check输出返回部分诊断信息，关闭该功能将限制rust-analyzer仅使用其自带的[诊断功能](https://rust-analyzer.github.io/book/diagnostics.html)。

建议参考手册中更多`rust-analyzer.cargo.`、`rust-analyzer.check.`和`rust-analyzer.diagnostics.`配置选项进行精细化设置。
以下为Zed配置文件settings.json的代码片段（编辑并保存`lsp.rust-analyzer`配置节后，语言服务器将自动重启）：

### 多项目工作空间

若希望 rust-analyzer 在同一目录下分析多个未在 Cargo 工作空间 `[members]` 中列出的 Rust 项目，
可在本地项目设置的 `linkedProjects` 中配置项目列表：

### 代码片段

有一种方法可以从 rust-analyzer 获取自定义补全项，这些项会根据片段内容来转换代码：

[[代码块_0]]

## 调试功能

Zed 支持通过 `CodeLLDB` 和 `GDB` 直接调试 Rust 二进制文件及测试程序。执行 {#action debugger::Start}（快捷键 {#kb debugger::Start}）即可启动这些预配置的调试任务。

若需更精细的控制，您可以在 `.zed/debug.json` 中添加调试配置。参考以下示例：

- [CodeLLDB 配置文档](https://github.com/vadimcn/codelldb/blob/master/MANUAL.md#starting-a-new-debug-session)
- [GDB 配置文档](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Debugger-Adapter-Protocol.html)

### 编译后调试

```json [debug]
[
  {
    "label": "Build & Debug native binary",
    "build": {
      "command": "cargo",
      "args": ["build"]
    },
    "program": "$ZED_WORKTREE_ROOT/target/debug/binary",
    // sourceLanguages is required for CodeLLDB (not GDB) when using Rust
    "sourceLanguages": ["rust"],
    "request": "launch",
    "adapter": "CodeLLDB"
  }
]
```

### 根据构建命令自动定位调试目标

当您使用 `cargo build` 或 `cargo test` 作为构建命令时，Zed 能够自动推断输出二进制文件的路径。

```json [debug]
[
  {
    "label": "Build & Debug native binary",
    "adapter": "CodeLLDB",
    "build": {
      "command": "cargo",
      "args": ["build"]
    },
    // sourceLanguages is required for CodeLLDB (not GDB) when using Rust
    "sourceLanguages": ["rust"]
  }
]
```