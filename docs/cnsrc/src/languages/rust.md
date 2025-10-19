# Rust

Zed 原生支持 Rust 语言。

- 语法分析器：[tree-sitter/tree-sitter-rust](https://github.com/tree-sitter/tree-sitter-rust)
- 语言服务器：[rust-lang/rust-analyzer](https://github.com/rust-lang/rust-analyzer)
- 调试适配器：[CodeLLDB](https://github.com/vadimcn/codelldb)（主要）、[GDB](https://sourceware.org/gdb/)（次要，不适用于苹果芯片）

<!--
TBD: Polish Rust Docs. Zed is a good rust editor, good Rust docs make it look like we care about Rust (we do!)
TBD: Users may not know what inlayHints, don't start there.
TBD: Provide explicit examples not just [[CODE_BLOCK_0]]
-->

## 内联提示

可通过以下配置调整 Rust 中 `rust-analyzer` 的内联提示设置：

```json [settings]
{
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        "inlayHints": {
          "maxLength": null,
          "lifetimeElisionHints": {
            "enable": "skip_trivial",
            "useParameterNames": true
          },
          "closureReturnTypeHints": {
            "enable": "always"
          }
        }
      }
    }
  }
}
```

更多信息请参阅 Rust Analyzer 手册中的[内联提示](https://rust-analyzer.github.io/book/features.html#inlay-hints)章节。

## 目标目录

可以通过`initialization_options`设置`rust-analyzer`目标目录：

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

将`true`设置为`target/rust-analyzer`会将目标目录指定为`target/rust-analyzer`。您也可以使用类似`"target/analyzer"`的字符串来自定义目录路径，而非使用`true`。

## 二进制文件

您可以配置 Zed 应使用哪个 `rust-analyzer` 二进制文件。

默认情况下，Zed 会尝试在您的 `$PATH` 中查找 `rust-analyzer` 并使用它。如果该二进制文件成功执行 `rust-analyzer --help`，就会被采用。否则，Zed 将回退到安装自带的稳定版 `rust-analyzer` 并使用该版本。

若希望安装预发布版 `rust-analyzer`，您可以通过在 `settings.json` 中将 `pre_release` 设为 `true` 来指示 Zed：

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

若要禁用 Zed 查找 `rust-analyzer` 二进制文件的功能，可在 `settings.json` 中将 `ignore_system_version` 设为 `true`：

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

如需使用自定义路径中的二进制文件，您可以指定 `path` 及可选的 `arguments`：

```json [settings]
{
  "lsp": {
    "rust-analyzer": {
      "binary": {
        "path": "/Users/example/bin/rust-analyzer",
        "arguments": []
      }
    }
  }
}
```

此`"path"`必须是绝对路径。

## 备用目标平台

若希望rust-analyzer为当前平台之外的目标平台提供诊断信息（例如在macOS系统上为Windows平台进行诊断），可使用以下Zed语言服务器配置：

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

若您正在使用`rustup`，可通过运行以下命令查看可用的目标平台三元组列表（`aarch64-apple-darwin`、`x86_64-unknown-linux-gnu`等）：

```sh
rustup target list --installed
```

## 语言服务器任务

Zed默认使用tree-sitter提供任务功能，但rust-analyzer额外提供了通过语言服务器协议查询文件相关任务的扩展方法。
该功能默认启用，可通过以下方式配置：

```json [settings]
"lsp": {
  "rust-analyzer": {
    "enable_lsp_tasks": true,
  }
}
```

## 手动获取 Cargo 诊断信息

默认情况下，rust-analyzer 启用了 `checkOnSave: true` 功能，这会导致每次缓冲区保存都会触发 `cargo check --workspace --all-targets` 命令。
如果通过 `checkOnSave: false` 禁用此功能（参见上方服务器配置 JSON 示例），仍然可以通过在 Rust 文件中使用 `editor: run/clear/cancel flycheck` 命令来手动获取 cargo 诊断信息；当启用相关设置时，项目诊断编辑器也会通过 `editor: run flycheck` 命令刷新 cargo 诊断信息。

## 更多服务器配置

<!--
TBD: Is it possible to specify RUSTFLAGS? https://github.com/zed-industries/zed/issues/14334
-->

Rust-analyzer [手册](https://rust-analyzer.github.io/book/) 详细介绍了 rust-analyzer 语言服务器的各种功能和配置选项。
Zed 中的 Rust-analyzer 使用默认参数运行。

### 大型项目与性能表现

在大型项目中可能导致资源过度消耗的一个主要注意事项，是以下功能特性的组合使用：

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

这意味着每次 Zed 执行保存操作时，都会运行 `cargo check --workspace --all-targets` 命令，对完整项目（工作区）、lib、doc、test、bin、bench 及[其他构建目标](https://doc.rust-lang.org/cargo/reference/cargo-targets.html)进行全面检查。

虽然这在小型项目中运行良好，但该方案不具备良好的可扩展性。

替代方案是使用[任务](../tasks.md)，因为Zed已经提供了`cargo check --workspace --all-targets`任务功能，并且支持通过cmd/ctrl点击终端输出来定位错误，同时可以限制或完全关闭保存时检查功能。

保存时检查功能负责基于cargo check输出来返回部分诊断信息，关闭该功能将限制rust-analyzer仅使用其自带的[诊断功能](https://rust-analyzer.github.io/book/diagnostics.html)。

建议参考手册中更多`rust-analyzer.cargo.`、`rust-analyzer.check.`和`rust-analyzer.diagnostics.`设置进行更精细的配置。
以下是Zed settings.json的配置片段（编辑并保存`lsp.rust-analyzer`部分后，语言服务器将自动重启）：

```json [settings]
{
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        // get more cargo-less diagnostics from rust-analyzer,
        // which might include false-positives (those can be turned off by their names)
        "diagnostics": {
          "experimental": {
            "enable": true
          }
        },
        // To disable the checking entirely
        // (ignores all cargo and check settings below)
        "checkOnSave": false,
        // To check the `lib` target only.
        "cargo": {
          "allTargets": false
        },
        // Use `-p` instead of `--workspace` for cargo check
        "check": {
          "workspace": false
        }
      }
    }
  }
}
```

### 多项目工作区

如果你希望 rust-analyzer 在同一文件夹中分析多个未在 Cargo 工作区的 `[members]` 中列出的 Rust 项目，
可以在本地项目设置的 `linkedProjects` 中列出它们：

```json [settings]
{
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        "linkedProjects": ["./path/to/a/Cargo.toml", "./path/to/b/Cargo.toml"]
      }
    }
  }
}
```

### 代码片段

有一种方法可以从 rust-analyzer 获取自定义补全项，这些补全项会根据代码片段内容来转换代码：

```json [settings]
{
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        "completion": {
          "snippets": {
            "custom": {
              "Arc::new": {
                "postfix": "arc",
                "body": ["Arc::new(${receiver})"],
                "requires": "std::sync::Arc",
                "scope": "expr"
              },
              "Some": {
                "postfix": "some",
                "body": ["Some(${receiver})"],
                "scope": "expr"
              },
              "Ok": {
                "postfix": "ok",
                "body": ["Ok(${receiver})"],
                "scope": "expr"
              },
              "Rc::new": {
                "postfix": "rc",
                "body": ["Rc::new(${receiver})"],
                "requires": "std::rc::Rc",
                "scope": "expr"
              },
              "Box::pin": {
                "postfix": "boxpin",
                "body": ["Box::pin(${receiver})"],
                "requires": "std::boxed::Box",
                "scope": "expr"
              },
              "vec!": {
                "postfix": "vec",
                "body": ["vec![${receiver}]"],
                "description": "vec![]",
                "scope": "expr"
              }
            }
          }
        }
      }
    }
  }
}
```

## 调试

Zed 默认支持通过 `CodeLLDB` 和 `GDB` 调试 Rust 二进制文件和测试。运行 {#action debugger::Start} ({#kb debugger::Start}) 即可启动这些预配置的调试任务。

如需更精细的控制，您可以在 `.zed/debug.json` 中添加调试配置。请参考以下示例：

- [CodeLLDB 配置文档](https://github.com/vadimcn/codelldb/blob/master/MANUAL.md#starting-a-new-debug-session)
- [GDB 配置文档](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Debugger-Adapter-Protocol.html)

### 构建二进制文件后调试

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

当您使用`cargo build`或`cargo test`作为构建命令时，Zed 能够自动推断输出二进制文件的路径。

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