# Java

Zed 中的 Java 语言支持由以下组件提供：

- Zed Java 扩展：[zed-extensions/java](https://github.com/zed-extensions/java)
- 语法分析器：[tree-sitter/tree-sitter-java](https://github.com/tree-sitter/tree-sitter-java)
- 语言服务器：[eclipse-jdtls/eclipse.jdt.ls](https://github.com/eclipse-jdtls/eclipse.jdt.ls)

## 安装 OpenJDK

您需要安装 Java 运行时环境（OpenJDK）。

- macOS：`brew install openjdk`
- Ubuntu：`sudo add-apt-repository ppa:openjdk-23 && sudo apt-get install openjdk-23`
- Windows：`choco install openjdk`
- Arch Linux：`sudo pacman -S jre-openjdk-headless`

或手动下载并安装 [OpenJDK 23](https://jdk.java.net/23/)。

## 扩展安装

您可以通过打开 {#action zed::Extensions}({#kb zed::Extensions}) 并搜索 `java` 来安装扩展。

## 设置 / 初始化选项

该扩展将自动下载语言服务器，若您希望自行管理，请参阅下方的[手动安装 JDTLS](#manual-jdts-install)部分。

关于可用的`initialization_options`配置项，请查阅[Eclipse.jdt.ls Wiki 的初始化请求章节](https://github.com/eclipse-jdtls/eclipse.jdt.ls/wiki/Running-the-JAVA-LS-server-from-the-command-line#initialize-request)。

您可以通过启动{#action zed::OpenSettings}({#kb zed::OpenSettings})或使用项目内的`.zed/setting.json`配置块，将这些自定义配置添加到 Zed 设置中。

### Zed Java 设置

```json [settings]
{
  "lsp": {
    "jdtls": {
      "initialization_options": {}
    }
  }
}
```

## 配置示例

### JDTLS 二进制文件

默认情况下，Zed 会在您的`PATH`路径中查找`jdtls`二进制文件。如需指定特定二进制文件，可通过设置进行配置：

### Zed Java 初始化选项

您还可以直接向语言服务器传递更多选项，例如：

[[代码块_0]]

## 手动安装 JDTLS

若您有需要，可自行安装 JDTLS，并将扩展配置为使用该版本。

- macOS：`brew install jdtls`
- Arch：通过 AUR 安装 [`jdtls`](https://aur.archlinux.org/packages/jdtls)

或手动下载安装：

- [JDTLS 里程碑版本](http://download.eclipse.org/jdtls/milestones/)（每两周更新）
- [JDTLS 快照版本](https://download.eclipse.org/jdtls/snapshots/)（高频更新）

## 相关链接

- [Zed Java 项目仓库](https://github.com/zed-extensions/java)
- [Zed Java 问题反馈](https://github.com/zed-extensions/java/issues)