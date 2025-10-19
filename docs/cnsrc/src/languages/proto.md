# Proto

Proto/proto3（Protocol Buffers 定义语言）支持可通过 [Proto 扩展](https://github.com/zed-industries/zed/tree/main/extensions/proto)实现。

- 语法分析器：[coder3101/tree-sitter-proto](https://github.com/coder3101/tree-sitter-proto)
- 语言服务器：[protobuf-language-server](https://github.com/lasorda/protobuf-language-server)

<!--
待定：需明确应使用的语言服务器/功能支持情况。

## 环境配置

### 安装 protobuf-language-server

安装 protobuf-language-server 并确保其位于 PATH 环境变量中：

```
go install github.com/lasorda/protobuf-language-server@latest
which protobuf-language-server
```

### 安装 ProtoLS

安装 protols 并确保其位于 PATH 环境变量中：

```
cargo install protols
which protols
```

## 配置

```json [settings]
"lsp": {
  "protobuf-language-server": {
    "binary": {
      "path": "protols"
    }
  }
}
```

## 代码格式化

若已安装 `clang-format`，ProtoLS 支持代码格式化功能。

```sh
# MacOS:
brew install clang-format
# Ubuntu
sudo apt-get install clang-format
# Fedora
sudo dnf install clang-tools-extra
```

要自定义格式化偏好设置，请创建 `.clang-format` 文件，例如：

```clang-format
IndentWidth: 4
ColumnLimit: 120
```

或者，您可以在设置中将 `clang-format` 指定为[格式化工具](https://zed.dev/docs/configuring-zed#formatter)，让 zed 直接调用它：

```json [settings]
  "languages": {
    "Proto": {
      "format_on_save": "on",
      "tab_size": 4,
      "formatter": {
        "external": {
          "command": "clang-format",
          "arguments": ["-style={IndentWidth: 4, ColumnLimit: 0}"]
        }
      }
    },
  }
```
-->