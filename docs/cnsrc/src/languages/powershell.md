# PowerShell

Zed 编辑器中的 PowerShell 语言支持由社区维护的 [Zed PowerShell 扩展](https://github.com/wingyplus/zed-powershell)提供。问题反馈请提交至：[github.com/wingyplus/zed-powershell/issues](https://github.com/wingyplus/zed-powershell/issues)

- 语法分析器：[airbus-cert/tree-sitter-powershell](https://github.com/airbus-cert/tree-sitter-powershell)
- 语言服务器：[PowerShell/PowerShellEditorServices](https://github.com/PowerShell/PowerShellEditorServices)

## 环境配置

### 安装 PowerShell 7+ 版本 {#powershell-install}

- macOS：`brew install powershell/tap/powershell`
- Alpine：[在Alpine Linux上安装PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/install-alpine)
- Debian：[在Debian Linux上安装PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/install-debian)
- RedHat：[在RHEL上安装PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/install-rhel)
- Ubuntu：[在Ubuntu上安装PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/install-ubuntu)
- Windows：[在Windows上安装PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows)

Zed的PowerShell扩展将默认使用路径中找到的`pwsh`可执行文件。

### 安装PowerShell编辑器服务（可选）{#powershell-editor-services}

Zed PowerShell 扩展将尝试自动下载 [PowerShell Editor Services](https://github.com/PowerShell/PowerShellEditorServices)。

如需使用特定二进制文件，可在 Zed 的 settings.json 文件中进行配置：

```json [settings]
  "lsp": {
    "powershell-es": {
      "binary": {
        "path": "/path/to/PowerShellEditorServices"
      }
    }
  }
```