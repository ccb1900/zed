# Ansible

Zed 对 Ansible 的支持由社区维护的 [Ansible 扩展](https://github.com/kartikvashistha/zed-ansible)提供。

- 语法分析器：[zed-industries/tree-sitter-yaml](https://github.com/zed-industries/tree-sitter-yaml)
- 语言服务器：[ansible/vscode-ansible](https://github.com/ansible/vscode-ansible/tree/main/packages/ansible-language-server)

## 配置

### 文件识别

为避免误处理非 Ansible 的 YAML 文件，Ansible 语言默认不与任何文件扩展名关联。如需修改此行为，您可以在项目的 Zed 设置（`"file_types"`）或用户全局设置（`.zed/settings.json`）中添加 `~/.config/zed/settings.json` 配置段，以匹配您的目录/命名规范。例如：

请根据您的需求随意修改此列表。

#### 库存

如果您的库存文件为 YAML 格式，您可以通过以下任一方式操作：

- 在库存文件顶部添加以下注释，将 `ansible-lint` 库存 JSON 架构附加到文件中：

```yml
# yaml-language-server: $schema=https://raw.githubusercontent.com/ansible/ansible-lint/main/src/ansiblelint/schemas/inventory.json
```

- 或者，在 Zed 设置中配置 YAML 语言服务器，为所有符合清单模式的文件设置此架构（[参考](https://zed.dev/docs/languages/yaml)）：

```json [settings]
"lsp": {
    "yaml-language-server": {
      "settings": {
        "yaml": {
          "schemas": {
            "https://raw.githubusercontent.com/ansible/ansible-lint/main/src/ansiblelint/schemas/inventory.json": [
              "./inventory/*.yaml",
              "hosts.yml",
            ]
          }
        }
      }
    }
},
```

### LSP 配置

默认情况下，Ansible 语言服务器会接收以下预设配置。该配置与 [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig/blob/03bc581e05e81d33808b42b2d7e76d70adb3b595/lua/lspconfig/configs/ansiblels.lua) 为 Ansible 语言服务器设置的默认值保持一致：

```json [settings]
{
  "ansible": {
    "ansible": {
      "path": "ansible"
    },
    "executionEnvironment": {
      "enabled": false
    },
    "python": {
      "interpreterPath": "python3"
    },
    "validation": {
      "enabled": true,
      "lint": {
        "enabled": true,
        "path": "ansible-lint"
      }
    }
  }
}
```

> [!注意]
> 为确保代码检查功能正常工作，请确保 `ansible-lint` 已安装并可在 PATH 环境变量中找到

如有需要，以上任何默认设置都可以在 Zed 设置文件的 `"lsp"` 部分进行覆盖。例如：

可在项目页面[此处](https://github.com/ansible/vscode-ansible/blob/5a89836d66d470fb9d20e7ea8aa2af96f12f61fb/docs/als/settings.md)查看可传递给服务器的完整选项/设置列表。
请根据实际需求自由调整选项数值。