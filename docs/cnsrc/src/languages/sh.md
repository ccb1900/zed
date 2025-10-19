# Shell 脚本

Zed 原生支持 Shell 脚本（bash、zsh、dash、sh）。

- 语法分析器：[tree-sitter/tree-sitter-bash](https://github.com/tree-sitter/tree-sitter-bash)

## 设置

您可以在 Zed 用户设置（`~/.config/zed/settings.json`）或 Zed 项目设置（`.zed/settings.json`）中配置 Shell 脚本的各项设置：

```json [settings]
  "languages": {
    "Shell Script": {
      "tab_size": 2,
      "hard_tabs": false
    }
  }
```

### 格式化

Zed 支持使用 [`shfmt`](https://github.com/mvdan/sh) 等外部工具对 Shell 脚本进行自动格式化。

1. 安装 `shfmt`：

```sh
brew install shfmt            # macos (homebrew)
sudo apt-get install shfmt    # debian/ubuntu
dnf install shfmt             # fedora
yum install shfmt             # redhat
pacman -Sy shfmt              # archlinux
choco install shfmt           # windows (chocolatey)
```

2. 确保 `shfmt` 在您的环境路径中可用，并检查版本：

```sh
which shfmt
shfmt --version
```

3. 配置 Zed 在保存时使用 `shfmt` 自动格式化 Shell 脚本：

```json [settings]
  "languages": {
    "Shell Script": {
      "format_on_save": "on",
      "formatter": {
        "external": {
          "command": "shfmt",
          // Change `--indent 2` to match your preferred tab_size
          "arguments": ["--filename", "{buffer_path}", "--indent", "2"]
        }
      }
    }
  }
```

## 另请参阅：

- [Zed 文档：语言支持：Bash](./bash.md)
- [Zed 文档：语言支持：Fish](./fish.md)