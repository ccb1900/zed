# Helm

Zed 对 Helm 的支持由社区维护的 [Helm 扩展](https://github.com/cabrinha/helm.zed)提供。

- 语法分析器：[tree-sitter-go-template](https://github.com/ngalaiko/tree-sitter-go-template/tree/master)
- 语言服务器：[mrjosh/helm-ls](https://github.com/mrjosh/helm-ls)

## 配置

通过编辑您的 `.zed/settings.json` 并添加以下内容，为 Helm 文件启用 Helm 语言支持：

```json [settings]
  "file_types": {
    "Helm": [
      "**/templates/**/*.tpl",
      "**/templates/**/*.yaml",
      "**/templates/**/*.yml",
      "**/helmfile.d/**/*.yaml",
      "**/helmfile.d/**/*.yml",
      "**/values*.yaml"
    ]
  }
```

由于 helm-ls 支持此功能，这也会将 values.yaml 文件标记为 helm 类型。