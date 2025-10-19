# XML

XML 功能支持可通过 [XML 扩展](https://github.com/sweetppro/zed-xml/)实现。

- 语法分析器：[tree-sitter-grammars/tree-sitter-xml](https://github.com/tree-sitter-grammars/tree-sitter-xml)

## 配置说明

若存在未被自动识别为 XML 格式的附加文件扩展名，只需在 Zed 设置的 [文件类型](../configuring-zed.md#file-types) 选项中添加即可：

```json [settings]
  "file_types": {
    "XML": ["rdf", "gpx", "kml"]
  }
```