# OCaml

OCaml 支持可通过 [OCaml 扩展](https://github.com/zed-extensions/ocaml)实现。

- 语法分析器：[tree-sitter/tree-sitter-ocaml](https://github.com/tree-sitter/tree-sitter-ocaml)
- 语言服务器：[ocaml/ocaml-lsp](https://github.com/ocaml/ocaml-lsp)

## 配置说明

若您已配置好开发环境，可直接跳转至[启动 Zed](#启动-zed) 部分。

### 使用 Opam

Opam 是 OCaml 官方推荐的包管理器，非常适合 OCaml 入门使用。请按照[此指南](https://ocaml.org/install)完成 Opam 的安装。

根据指南安装 opam 并设置好开发环境切换后，即可继续后续操作。

### 启动 Zed

此时您应已安装 `ocamllsp`，可通过以下命令验证：

```sh
ocamllsp --help
```

在你的终端中。如果看到帮助信息，说明一切就绪。如果没有，请重新查阅 `ocamllsp` 的安装指南，并确保已正确安装。

言归正传，我们现在可以启动 Zed 了。鉴于 OCaml 包管理器的工作方式，需要你通过终端运行 Zed，因此请确保已安装 [Zed 命令行工具](https://zed.dev/features#cli)（若尚未安装）。

安装命令行工具后，只需在终端中进入项目目录并执行：

```sh
zed .
```

大功告成！现在你应该能直接运行 Zed 并享受 OCaml 支持，无需任何额外配置。