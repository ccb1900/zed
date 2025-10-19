# Proto

Proto/proto3（Protocol Buffers 定义语言）支持通过 [Proto 扩展](https://github.com/zed-industries/zed/tree/main/extensions/proto)实现。

- Tree-sitter 解析器：[coder3101/tree-sitter-proto](https://github.com/coder3101/tree-sitter-proto)
- 语言服务器：[protobuf-language-server](https://github.com/lasorda/protobuf-language-server)

<!--
待定：明确应使用的语言服务器/功能支持情况。

## 环境配置

### 安装 protobuf-language-server

安装 protobuf-language-server 并确保其位于 PATH 环境变量中：

[[代码块_0]]

### 安装 ProtoLS

安装 protols 并确保其位于 PATH 环境变量中：

[[代码块_1]]

## 配置

[[代码块_2]]

## 代码格式化

若已安装 [[代码块_3]]，ProtoLS 支持代码格式化功能。
-->

要自定义您的格式化偏好设置，可以创建一个 `.clang-format` 文件，例如：

```clang-format
IndentWidth: 4
ColumnLimit: 120
```

或者，您也可以通过将 `clang-format` 指定为设置中的 [格式化工具](https://zed.dev/docs/configuring-zed#formatter)，让 zed 直接调用它：

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