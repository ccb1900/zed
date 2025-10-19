# R

通过多个 Zed 的 R 扩展插件可获得 R 语言支持：

- [ocsmit/zed-r](https://github.com/ocsmit/zed-r)

  - 语法分析器：[r-lib/tree-sitter-r](https://github.com/r-lib/tree-sitter-r)
  - 语言服务器：[REditorSupport/languageserver](https://github.com/REditorSupport/languageserver)

- [posit-dev/air](https://github.com/posit-dev/air/tree/main/editors/zed)
  - 语言服务器：[posit-dev/air](https://github.com/posit-dev/air)

## 安装步骤

1. [下载并安装 R](https://cloud.r-project.org/)。
2. 安装 R 包 `languageserver` 和 `lintr`：

```R
install.packages("languageserver")
install.packages("lintr")
```

3. 通过 Zed 的扩展管理器安装 [ocsmit/zed-r](https://github.com/ocsmit/zed-r)。

例如在 macOS 系统上：

```sh
brew install --cask r
Rscript --version
Rscript -e 'options(repos = "https://cran.rstudio.com/"); install.packages("languageserver")'
Rscript -e 'options(repos = "https://cran.rstudio.com/"); install.packages("lintr")'
Rscript -e 'packageVersion("languageserver")'
Rscript -e 'packageVersion("lintr")'
```

## 配置

### 代码检查

`REditorSupport/languageserver` 集成了 [r-lib/lintr](https://github.com/r-lib/lintr) 作为代码检查工具。可以通过在项目中使用 `.lintr` 文件（或在主目录中设置全局默认值）进行配置。

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

或者将其从代码检查中排除：

```r
exclusions: list(".")
```

完整选项列表请参阅[使用 lintr](https://lintr.r-lib.org/articles/lintr.html)，

### 代码格式化

`REditorSupport/languageserver` 捆绑了对 [r-lib/styler](https://github.com/r-lib/styler) 格式化工具的支持。如需自定义其行为，请参阅[自定义 Styler](https://cran.r-project.org/web/packages/styler/vignettes/customizing_styler.html) 获取更多信息。

<!--
TBD: Get this working

### REditorSupport/languageserver Configuration

You can configure the [R languageserver settings](https://github.com/REditorSupport/languageserver#settings) via Zed Project Settings [[CODE_BLOCK_2]] or Zed User Settings [[CODE_BLOCK_3]]:

For example to disable Lintr linting and suppress code snippet suggestions (both enabled by default):

[[CODE_BLOCK_0]]

-->

<!--
待定：R 交互式环境文档

## 交互式环境

### Ark 安装

要使用 Zed REPL 配合 R 语言，您需要安装 [Ark](https://github.com/posit-dev/ark)——一个适用于 Jupyter 应用程序的 R 语言内核。  
您可以从 [Ark GitHub Releases](https://github.com/posit-dev/ark/releases) 下载最新版本，然后将 `ark` 可执行文件解压到您的 `PATH` 目录中。

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

```sh
# Linux X86_64
cd /tmp
curl -L -o ark-latest-linux.zip \
    $(curl -s "https://api.github.com/repos/posit-dev/ark/releases/latest" \
        | jq -r '.assets[] | select(.name | contains("linux-x64") and (contains("debug") | not)) | .browser_download_url'
    )
unzip ark-latest-linux.zip ark
sudo mv /tmp/ark /usr/local/bin/
```