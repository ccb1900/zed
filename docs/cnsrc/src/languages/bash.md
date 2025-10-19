# Bash

Zed 中的 Bash 语言支持由社区维护的 [Basher 扩展](https://github.com/d1y/bash.zed)提供。
问题反馈请至：[https://github.com/d1y/bash.zed/issues](https://github.com/d1y/bash.zed/issues)

- 语法分析器：[tree-sitter/tree-sitter-bash](https://github.com/tree-sitter/tree-sitter-bash)
- 语言服务器：[bash-lsp/bash-language-server](https://github.com/bash-lsp/bash-language-server)

## 配置

当 `shellcheck` 可用时，`bash-language-server` 将在内部使用它来提供诊断信息。

### 安装 `shellcheck`：

```sh
brew install shellcheck             # macOS (HomeBrew)
apt-get install shellcheck          # Ubuntu/Debian
pacman -S shellcheck                # ArchLinux
dnf install shellcheck              # Fedora
yum install shellcheck              # CentOS/RHEL
zypper install shellcheck           # openSUSE
choco install shellcheck            # Windows (Chocolatey)
```

并验证其是否可在您的路径中使用：

```sh
which shellcheck
shellcheck --version
```

如需自定义报告的警告/错误信息，只需创建 `.shellcheckrc` 文件即可。您可以在项目根目录或主目录（`~/.shellcheckrc`）中进行此操作。更多信息请参阅：[shellcheck 文档](https://github.com/koalaman/shellcheck/wiki/Ignore#ignoring-one-or-more-types-of-errors-forever)。

### 另请参阅：

- [Zed 文档：语言支持：Shell 脚本](./sh.md)