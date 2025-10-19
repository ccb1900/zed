# Terraform

Terraform 支持可通过 [Terraform 扩展](https://github.com/zed-extensions/terraform) 获得。

- 语法分析器：[MichaHoffmann/tree-sitter-hcl](https://github.com/MichaHoffmann/tree-sitter-hcl)
- 语言服务器：[hashicorp/terraform-ls](https://github.com/hashicorp/terraform-ls)

## 配置

Terraform 语言服务器可在您的 `settings.json` 中进行配置，例如：

```json [settings]
{
  "lsp": {
    "terraform-ls": {
      "initialization_options": {
        "experimentalFeatures": {
          "prefillRequiredFields": true
        }
      }
    }
  }
}
```

查看[完整的服务器设置列表请点击此处](https://github.com/hashicorp/terraform-ls/blob/main/docs/SETTINGS.md)。