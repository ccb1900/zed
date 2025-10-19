# Swift

Zed 编辑器中的 Swift 语言支持由社区维护的 [Swift 扩展](https://github.com/zed-extensions/swift)提供。
问题反馈请至：[https://github.com/zed-extensions/swift/issues](https://github.com/zed-extensions/swift/issues)

- 语法分析器：[alex-pinkus/tree-sitter-swift](https://github.com/alex-pinkus/tree-sitter-swift)
- 语言服务器：[swiftlang/sourcekit-lsp](https://github.com/swiftlang/sourcekit-lsp)
- 调试适配器：[`lldb-dap`](https://github.com/swiftlang/llvm-project/blob/next/lldb/tools/lldb-dap/README.md)

## 语言服务器配置

您可以通过在用户主目录或项目根目录下创建 `.sourcekit-lsp/config.json` 文件来调整 SourceKit LSP 的行为。完整配置说明请参阅 [SourceKit-LSP 配置文件文档](https://github.com/swiftlang/sourcekit-lsp/blob/main/Documentation/Configuration%20File.md)。

## 调试功能

Swift扩展提供了一个用于调试Swift代码的调试适配器。
Zed界面及[[代码块1]]中显示的适配器名称为[[代码块2]]，其底层使用的是Swift工具链提供的[[代码块3]](https://github.com/swiftlang/llvm-project/blob/next/lldb/tools/lldb-dap/README.md)。
该扩展会按以下优先级顺序查找[[代码块4]]二进制文件：通过[[代码块5]]、使用[[代码块6]]，以及搜索[[代码块7]]。
如果未找到[[代码块8]]，该扩展不会尝试自动下载。

- [lldb-dap配置文档](https://github.com/llvm/llvm-project/blob/main/lldb/tools/lldb-dap/README.md#configuration-settings-reference)

### 示例

#### 构建并调试Swift二进制文件

[[代码块0]]