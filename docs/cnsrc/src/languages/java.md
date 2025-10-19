# Java

Zed 中的 Java 语言支持由以下组件提供：

- Zed Java 扩展：[zed-extensions/java](https://github.com/zed-extensions/java)
- Tree-sitter 解析器：[tree-sitter/tree-sitter-java](https://github.com/tree-sitter/tree-sitter-java)
- 语言服务器：[eclipse-jdtls/eclipse.jdt.ls](https://github.com/eclipse-jdtls/eclipse.jdt.ls)

## 安装 OpenJDK

您需要安装 Java 运行时环境（OpenJDK）：

- macOS：`brew install openjdk`
- Ubuntu：`sudo add-apt-repository ppa:openjdk-23 && sudo apt-get install openjdk-23`
- Windows：`choco install openjdk`
- Arch Linux：`sudo pacman -S jre-openjdk-headless`

或手动下载并安装 [OpenJDK 23](https://jdk.java.net/23/)。

## 扩展安装

您可以通过打开 {#action zed::Extensions}({#kb zed::Extensions}) 并搜索 `java` 来安装扩展。

## 设置 / 初始化选项

该扩展将自动下载语言服务器，如果您希望自行管理，请参阅下方的[手动安装JDTLS](#manual-jdts-install)章节。

关于可用的`initialization_options`，请查阅[Eclipse.jdt.ls维基的初始化请求章节](https://github.com/eclipse-jdtls/eclipse.jdt.ls/wiki/Running-the-JAVA-LS-server-from-the-command-line#initialize-request)。

您可以通过启动{#action zed::OpenSettings}({#kb zed::OpenSettings})或使用项目内的`.zed/setting.json`，将这些自定义配置添加到Zed设置中。

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

默认情况下，zed会在您的`PATH`中查找`jdtls`二进制文件。如需指定特定二进制文件，可通过设置实现：

```json [settings]
  "lsp": {
    "jdtls": {
      "binary": {
        "path": "/path/to/java/bin/jdtls",
        // "arguments": [],
        // "env": {},
        "ignore_system_version": true
      }
    }
  }
```

### Zed Java 初始化选项

您还可以直接向语言服务器传递更多选项，例如：

```json [settings]
{
  "lsp": {
    "jdtls": {
      "initialization_options": {
        "bundles": [],
        "workspaceFolders": ["file:///home/snjeza/Project"],
        "settings": {
          "java": {
            "home": "/usr/local/jdk-9.0.1",
            "errors": {
              "incompleteClasspath": {
                "severity": "warning"
              }
            },
            "configuration": {
              "updateBuildConfiguration": "interactive",
              "maven": {
                "userSettings": null
              }
            },
            "trace": {
              "server": "verbose"
            },
            "import": {
              "gradle": {
                "enabled": true
              },
              "maven": {
                "enabled": true
              },
              "exclusions": [
                "**/node_modules/**",
                "**/.metadata/**",
                "**/archetype-resources/**",
                "**/META-INF/maven/**",
                "/**/test/**"
              ]
            },
            "jdt": {
              "ls": {
                "lombokSupport": {
                  "enabled": false // Set this to true to enable lombok support
                }
              }
            },
            "referencesCodeLens": {
              "enabled": false
            },
            "signatureHelp": {
              "enabled": false
            },
            "implementationsCodeLens": {
              "enabled": false
            },
            "format": {
              "enabled": true
            },
            "saveActions": {
              "organizeImports": false
            },
            "contentProvider": {
              "preferred": null
            },
            "autobuild": {
              "enabled": false
            },
            "completion": {
              "favoriteStaticMembers": [
                "org.junit.Assert.*",
                "org.junit.Assume.*",
                "org.junit.jupiter.api.Assertions.*",
                "org.junit.jupiter.api.Assumptions.*",
                "org.junit.jupiter.api.DynamicContainer.*",
                "org.junit.jupiter.api.DynamicTest.*"
              ],
              "importOrder": ["java", "javax", "com", "org"]
            }
          }
        }
      }
    }
  }
}
```

## 手动安装 JDTLS

如果您希望自行安装 JDTLS，可将本扩展配置为使用自定义安装路径。

- macOS 系统：`brew install jdtls`
- Arch 系统：通过 AUR 安装 `jdtls`（[下载地址](https://aur.archlinux.org/packages/jdtls)）

或手动下载安装：

- [JDTLS 里程碑版本](http://download.eclipse.org/jdtls/milestones/)（每两周更新）
- [JDTLS 快照版本](https://download.eclipse.org/jdtls/snapshots/)（高频更新）

## 相关资源

- [Zed Java 扩展仓库](https://github.com/zed-extensions/java)
- [Zed Java 问题反馈](https://github.com/zed-extensions/java/issues)