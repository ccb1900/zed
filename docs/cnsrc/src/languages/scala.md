# Scala

Zed 中的 Scala 语言支持由社区维护的 [Scala 扩展](https://github.com/scalameta/metals-zed)提供。
问题反馈请至：[https://github.com/scalameta/metals-zed/issues](https://github.com/scalameta/metals-zed/issues)

- 语法分析器：[tree-sitter/tree-sitter-scala](https://github.com/tree-sitter/tree-sitter-scala)
- 语言服务器：[scalameta/metals](https://github.com/scalameta/metals)

## 环境配置

- 使用 `cs setup` (Coursier) 安装 Scala：https://www.scala-lang.org/download/
  - `brew install coursier/formulas/coursier && cs setup`
- REPL (Almond) 安装指南 https://almond.sh/docs/quick-start-install
  - `brew install --cask temurin` (Eclipse 基金会官方 OpenJDK 二进制文件)
  - `brew install coursier/formulas/coursier && cs setup`
  - `coursier launch --use-bootstrap almond -- --install`

## 配置说明

可通过以下方式配置 Metals 语言服务器的行为：

- `.scalafix.conf` 文件 - 参见 [Scalafix 配置](https://scalacenter.github.io/scalafix/docs/users/configuration.html)
- `.scalafmt.conf` 文件 - 参见 [Scalafmt 配置](https://scalameta.org/scalafmt/docs/configuration.html)

您可以将这些文件放置在项目根目录中，或在 Metals 配置中指定其位置。更多信息请参阅 [Metals 用户配置](https://scalameta.org/metals/docs/editors/user-configuration)。

<!--
TBD: Provide LSP configuration example for metals in Zed settings.json. metals.{javaHome,excludedPackages,customProjectRoot} etc.
-->