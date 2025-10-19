# Kotlin

Zed 中的 Kotlin 语言支持由社区维护的 [Kotlin 扩展](https://github.com/zed-extensions/kotlin)提供。
问题反馈请至：[https://github.com/zed-extensions/kotlin/issues](https://github.com/zed-extensions/kotlin/issues)

- 语法分析器：[fwcd/tree-sitter-kotlin](https://github.com/fwcd/tree-sitter-kotlin)
- 语言服务器：[fwcd/kotlin-language-server](https://github.com/fwcd/kotlin-language-server)

## 配置

工作区配置选项可通过 `settings.json` 中的 lsp 设置传递给语言服务器。

完整的 lsp `settings` 列表可在[此处](https://github.com/fwcd/kotlin-language-server/blob/main/server/src/main/kotlin/org/javacs/kt/Configuration.kt)
查看，其中 `class Configuration` 对应常规配置项，`class InitializationOptions` 对应初始化选项。

### JVM 目标平台

以下示例将 JVM 目标平台从 `default`（即 1.8 版本）更改为 `17`：

### JAVA_HOME

要使用特定的 Java 安装，只需通过以下方式指定 `JAVA_HOME` 环境变量：

```json [settings]
{
  "lsp": {
    "kotlin-language-server": {
      "binary": {
        "env": {
          "JAVA_HOME": "/Users/whatever/Applications/Work/Android Studio.app/Contents/jbr/Contents/Home"
        }
      }
    }
  }
}
```