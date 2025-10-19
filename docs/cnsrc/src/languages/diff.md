# 差异对比

Zed 原生支持差异对比功能。

- 语法分析器：[zed-industries/the-mikedavis/tree-sitter-diff](https://github.com/the-mikedavis/tree-sitter-diff)

## 配置说明

Zed 不会对差异文件进行格式化操作，且默认将[[[保存时移除行尾空格]]](https://zed.dev/docs/configuring-zed#remove-trailing-whitespace-on-save)与[[[保存时确保文件末尾换行]]](https://zed.dev/docs/configuring-zed#ensure-final-newline-on-save)功能设为关闭状态。

系统会自动识别扩展名为[[.diff]]和[[.patch]]的文件作为差异对比文件。如需添加其他扩展名，请在 Zed 的 settings.json 配置文件中将其添加至[[差异对比文件扩展名]]设置项：

```json
{
  "file_types": {
    "Diff": ["your_custom_extension"]
  }
}
```