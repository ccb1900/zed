# R

通过多个 Zed 扩展可实现对 R 语言的支持：

- [ocsmit/zed-r](https://github.com/ocsmit/zed-r)

  - 语法分析器：[r-lib/tree-sitter-r](https://github.com/r-lib/tree-sitter-r)
  - 语言服务器：[REditorSupport/languageserver](https://github.com/REditorSupport/languageserver)

- [posit-dev/air](https://github.com/posit-dev/air/tree/main/editors/zed)
  - 语言服务器：[posit-dev/air](https://github.com/posit-dev/air)

## 安装步骤

1. [下载并安装 R 语言环境](https://cloud.r-project.org/)
2. 安装 R 软件包 [[代码块_1]] 和 [[代码块_2]]：

[[代码块_0]]

3. 通过 Zed 的扩展管理器安装 [ocsmit/zed-r](https://github.com/ocsmit/zed-r)

以 macOS 系统为例：

## 配置

### 代码检查

`REditorSupport/languageserver` 内置了对 [r-lib/lintr](https://github.com/r-lib/lintr) 检查工具的支持。可通过在项目内（或在主目录中设置全局默认值）创建 `.lintr` 文件进行配置。

```r
linters: linters_with_defaults(
    line_length_linter(120),
    commented_code_linter = NULL
  )
exclusions: list(
    "inst/doc/creating_linters.R" = 1,
    "inst/example/bad.R",
    "tests/testthat/exclusions-test"
  )
```

或将其从检查范围中排除：

```r
exclusions: list(".")
```

完整配置选项请参阅 [使用 lintr](https://lintr.r-lib.org/articles/lintr.html)：

### 代码格式化

`REditorSupport/languageserver` 集成了对 [r-lib/styler](https://github.com/r-lib/styler) 格式化工具的支持。如需自定义其行为，请参阅 [自定义 Styler](https://cran.r-project.org/web/packages/styler/vignettes/customizing_styler.html) 获取更多信息。

<!--
TBD: Get this working

### REditorSupport/languageserver Configuration

You can configure the [R languageserver settings](https://github.com/REditorSupport/languageserver#settings) via Zed Project Settings [[CODE_BLOCK_2]] or Zed User Settings [[CODE_BLOCK_3]]:

For example to disable Lintr linting and suppress code snippet suggestions (both enabled by default):

[[CODE_BLOCK_0]]

-->

<!--
待办：R 语言交互式环境文档

## 交互式环境

### Ark 安装指南

要使用 Zed REPL 与 R 语言配合，您需要安装 [Ark](https://github.com/posit-dev/ark)——这是一个专为 Jupyter 应用设计的 R 语言内核。
您可以从 [Ark GitHub Releases](https://github.com/posit-dev/ark/releases) 下载最新版本，然后将 `ark` 二进制文件解压到您的 `PATH` 目录中。

例如，安装最新的非调试版本：

```sh
# macOS
cd /tmp
curl -L -o ark-latest-darwin.zip \
    $(curl -s "https://api.github.com/repos/posit-dev/ark/releases/latest" | \
    jq -r '.assets[] | select(.name | contains("darwin-universal") and (contains("debug") | not)) | .browser_download_url')
unzip ark-latest-darwin.zip ark
sudo mv /tmp/ark /usr/local/bin/
```

-->