# Dart

Dart 语言支持可通过 [Dart 扩展](https://github.com/zed-extensions/dart)实现。

- 语法分析器：[UserNobody14/tree-sitter-dart](https://github.com/UserNobody14/tree-sitter-dart)
- 语言服务器：[dart language-server](https://github.com/dart-lang/sdk)

## 环境要求

需要安装 Dart SDK。

可通过 [dart.dev/get-dart](https://dart.dev/get-dart) 官网安装，或使用 [Flutter 版本管理工具 (fvm)](https://fvm.app/documentation/getting-started/installation) 进行安装。

## 配置说明

若系统路径中已配置 `dart`，Dart 扩展无需额外配置：

```sh
which dart
dart --version
```

如需指定 Dart 可执行文件路径或使用 FVM 管理的 Dart，可在 Zed 的 settings.json 配置文件中设置 `dart` 参数：

```json [settings]
{
  "lsp": {
    "dart": {
      "binary": {
        "path": "/opt/homebrew/bin/fvm",
        "arguments": ["dart", "language-server", "--protocol=lsp"]
      }
    }
  }
}
```

### 格式设置

Dart 默认采用非常保守的最大行长度限制（80个字符）。若希望 Dart LSP 在自动格式化时允许更长的行长度，请将以下内容添加到 Zed 的 settings.json 配置文件中：

```json [settings]
{
  "lsp": {
    "dart": {
      "settings": {
        "lineLength": 140
      }
    }
  }
}
```

关于 [Dart 语言服务器功能](https://github.com/dart-lang/sdk/blob/main/pkg/analysis_server/tool/lsp_spec/README.md) 的更多信息，请参阅 Dart 官方文档。