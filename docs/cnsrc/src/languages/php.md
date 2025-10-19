# PHP

PHP 支持通过 [PHP 扩展](https://github.com/zed-extensions/php)实现。

- Tree-sitter：https://github.com/tree-sitter/tree-sitter-php
- 语言服务器：
  - [phpactor](https://github.com/phpactor/phpactor)
  - [intelephense](https://github.com/bmewburn/vscode-intelephense/)

## 选择语言服务器

PHP 扩展同时提供 `phpactor` 和 `intelephense` 语言服务器支持。

默认启用 `phpactor`。

## Phpactor

Zed PHP 扩展可自动安装 `phpactor`，但需要提前安装 `php` 并确保其在系统路径中可用：

```sh
# brew install php            # macOS
# sudo apt-get install php    # Debian/Ubuntu
# yum install php             # CentOS/RHEL
# pacman -S php               # Arch Linux
which php
```

## Intelephense

[Intelephense](https://intelephense.com/) 是一款基于免费增值模式的[专有](https://github.com/bmewburn/vscode-intelephense/blob/master/LICENSE.txt#L29) PHP 语言服务器，部分功能需购买[高级许可证](https://intelephense.com/)方可使用。

若需切换至 [[代码块1]]，请将以下内容添加至 [[代码块2]]：

[[代码块0]]

要使用高级功能，您可以将[许可证文件 licence.txt](https://intelephense.com/faq.html) 放置在用户主目录的 [[代码块3]] 路径下。或者，您也可以将许可证密钥或包含许可证密钥的文件路径，作为初始化参数传递给 [[代码块4]] 语言服务器。具体操作是在 [[代码块5]] 中添加以下配置：

```json [settings]
{
  "lsp": {
    "intelephense": {
      "initialization_options": {
        "licenceKey": "/path/to/licence.txt"
      }
    }
  }
}
```

## PHPDoc

Zed 支持 PHPDoc 注释的语法高亮功能。

- Tree-sitter 解析器：[claytonrcarter/tree-sitter-phpdoc](https://github.com/claytonrcarter/tree-sitter-phpdoc)