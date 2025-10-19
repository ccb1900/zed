# YAML

Zed 原生支持 YAML 格式。

- 语法分析器：[zed-industries/tree-sitter-yaml](https://github.com/zed-industries/tree-sitter-yaml)
- 语言服务器：[redhat-developer/yaml-language-server](https://github.com/redhat-developer/yaml-language-server)

## 配置

您可以通过在 Zed 的 settings.json 文件中添加 `yaml-language-server` 代码块（位于 `lsp` 键下），来配置各种 [yaml-language-server 设置](https://github.com/redhat-developer/yaml-language-server?tab=readme-ov-file#language-server-settings)。例如：

```json [settings]
  "lsp": {
    "yaml-language-server": {
      "settings": {
        "yaml": {
          "keyOrdering": true,
          "format": {
            "singleQuote": true
          },
          "schemas": {
              "http://json.schemastore.org/composer": ["/*"],
              "../relative/path/schema.json": ["/config*.yaml"]
          }
        }
      }
    }
  }
```

注意，设置键必须嵌套，因此`yaml.keyOrdering`需改为`{"yaml": { "keyOrdering": true }}`。

## 格式化

Zed 默认使用 Prettier 格式化 YAML 文件。

### Prettier 格式化

您可以自定义 Prettier 的格式化行为。例如，若要在 YAML 文件中使用单引号，请将以下内容添加到您的`.prettierrc`配置文件中：

```json [settings]
{
  "overrides": [
    {
      "files": ["*.yaml", "*.yml"],
      "options": {
        "singleQuote": false
      }
    }
  ]
}
```

### yaml-language-server 格式化

若要在 YAML 格式化中使用`yaml-language-server`替代 Prettier，请将以下内容添加到 Zed 的`settings.json`中：

```json [settings]
  "languages": {
    "YAML": {
      "formatter": "language_server"
    }
  }
```

## 模式

默认情况下，yaml-language-server 会尝试确定给定 YAML 文件的正确模式，并从 [Json Schema Store](https://schemastore.org/) 获取相应的 JSON 模式。

您可以通过 `schemas` 设置键（如上所示）覆盖任何自动检测的架构，或者通过在 YAML 文件顶部添加模式行注释来提供[内联架构](https://github.com/redhat-developer/yaml-language-server#using-inlined-schema)引用：

```yaml
# yaml-language-server: $schema=https://json.schemastore.org/github-action.json
name: Issue Assignment
on:
  issues:
    types: [oppened]
```

如果需要，您可以禁用从 JSON 架构自动检测和获取架构的功能：

```json [settings]
  "lsp": {
    "yaml-language-server": {
      "settings": {
        "yaml": {
          "schemaStore": {
            "enable": false
          }
        }
      }
    }
  }
```

## 自定义标签

Yaml-language-server 支持[自定义标签](https://github.com/redhat-developer/yaml-language-server#adding-custom-tags)，这些标签可用于在运行时将自定义应用程序功能注入到您的 YAML 文件中。

例如，Amazon CloudFormation YAML 使用了许多自定义标签，为了支持这些标签，您可以将以下内容添加到 settings.json 文件中：

```json [settings]
  "lsp": {
    "yaml-language-server": {
      "settings": {
        "yaml": {
          "customTags": [
            "!And scalar",
            "!And mapping",
            "!And sequence",
            "!If scalar",
            "!If mapping",
            "!If sequence",
            "!Not scalar",
            "!Not mapping",
            "!Not sequence",
            "!Equals scalar",
            "!Equals mapping",
            "!Equals sequence",
            "!Or scalar",
            "!Or mapping",
            "!Or sequence",
            "!FindInMap scalar",
            "!FindInMap mapping",
            "!FindInMap sequence",
            "!Base64 scalar",
            "!Base64 mapping",
            "!Base64 sequence",
            "!Cidr scalar",
            "!Cidr mapping",
            "!Cidr sequence",
            "!Ref scalar",
            "!Ref mapping",
            "!Ref sequence",
            "!Sub scalar",
            "!Sub mapping",
            "!Sub sequence",
            "!GetAtt scalar",
            "!GetAtt mapping",
            "!GetAtt sequence",
            "!GetAZs scalar",
            "!GetAZs mapping",
            "!GetAZs sequence",
            "!ImportValue scalar",
            "!ImportValue mapping",
            "!ImportValue sequence",
            "!Select scalar",
            "!Select mapping",
            "!Select sequence",
            "!Split scalar",
            "!Split mapping",
            "!Split sequence",
            "!Join scalar",
            "!Join mapping",
            "!Join sequence",
            "!Condition scalar",
            "!Condition mapping",
            "!Condition sequence"
          ]
        }
      }
    }
  }
```