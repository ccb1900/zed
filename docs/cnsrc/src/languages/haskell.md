# Haskell

Haskell 支持可通过 [Haskell 扩展](https://github.com/zed-extensions/haskell)实现。

- 语法分析器：[tree-sitter-haskell](https://github.com/tree-sitter/tree-sitter-haskell)
- 语言服务器：[haskell-language-server](https://github.com/haskell/haskell-language-server)

## 安装 HLS

推荐通过 [ghcup](https://www.haskell.org/ghcup/install/) 工具安装 [haskell-language-server](https://haskell-language-server.readthedocs.io/en/latest/installation.html)（HLS）（`curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
`）：

```sh
ghcup install hls
which haskell-language-server-wrapper
```

## 配置 HLS

如需配置 haskell-language-server (hls)，可将配置选项添加到 Zed 的 settings.json 文件中：

```json [settings]
{
  "lsp": {
    "hls": {
      "initialization_options": {
        "haskell": {
          "formattingProvider": "fourmolu"
        }
      }
    }
  }
}
```

更多选项请参阅官方的 [配置 haskell-language-server](https://haskell-language-server.readthedocs.io/en/latest/configuration.html) 文档。

若您希望使用特定的 hls 二进制文件，或想用 [static-ls](https://github.com/josephsumabat/static-ls) 作为即插即用的替代方案，可通过以下方式指定二进制路径与参数：

```json [settings]
{
  "lsp": {
    "hls": {
      "binary": {
        "path": "static-ls",
        "arguments": ["--experimentalFeatures"]
      }
    }
  }
}
```