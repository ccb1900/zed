# Ruby

Ruby 语言支持可通过 [Ruby 扩展](https://github.com/zed-extensions/ruby)实现。

- 语法分析器：
  - [tree-sitter-ruby](https://github.com/tree-sitter/tree-sitter-ruby)
  - [tree-sitter-embedded-template](https://github.com/tree-sitter/tree-sitter-embedded-template)
- 语言服务器：
  - [ruby-lsp](https://github.com/Shopify/ruby-lsp)
  - [solargraph](https://github.com/castwide/solargraph)
  - [rubocop](https://github.com/rubocop/rubocop)
  - [Herb](https://herb-tools.dev)
- 调试适配器：[`rdbg`](https://github.com/ruby/debug)

该 Ruby 扩展还支持 ERB 文件。

## 语言服务器

目前有多款适用于 Ruby 的语言服务器。Zed 支持以下两种：

- [solargraph](https://github.com/castwide/solargraph)
- [ruby-lsp](https://github.com/Shopify/ruby-lsp)

它们都具备自动补全、诊断检查、代码操作等重叠功能，具体使用哪一个由您自行决定。请注意，两者无法同时启用。

除这两款语言服务器外，Zed 还支持以下工具：

- [rubocop](https://github.com/rubocop/rubocop)：Ruby 静态代码分析与语法检查工具。在 Zed 中它同样作为语言服务器运行，其功能与 solargraph 和 ruby-lsp 形成互补。
- [sorbet](https://sorbet.org/)：采用渐进式类型系统的 Ruby 静态类型检查工具。
- [steep](https://github.com/soutaro/steep)：基于 Ruby 类型签名（RBS）的静态类型检查器。
- [Herb](https://herb-tools.dev)：专为 ERB 文件设计的语言服务器。

配置语言服务器时，建议通过"dev: 打开语言服务器日志"命令开启LSP日志窗口，随后选择对应的语言实例即可查看记录的日志信息。

## 配置语言服务器

[Ruby扩展](https://github.com/zed-extensions/ruby)同时支持`solargraph`与`ruby-lsp`语言服务器。

### 语言服务器激活流程

对于所有支持的Ruby语言服务器（`solargraph`、`ruby-lsp`、`rubocop`、`sorbet`及`steep`），Ruby扩展按以下顺序激活：

1. 若在项目的`Gemfile`目录中发现语言服务器，将通过`bundle exec`调用
2. 若`Gemfile`中未找到，Ruby扩展会从系统`PATH`路径查找可执行文件
3. 若上述位置均未找到，Ruby扩展将自动将其作为全局gem安装（注意：不会安装至当前Ruby gemset环境）

你可以跳过步骤1，通过在设置中将 `use_bundler` 设为 `false` 来强制使用系统可执行文件：

```json [settings]
{
  "lsp": {
    "<SERVER_NAME>": {
      "settings": {
        "use_bundler": false
      }
    }
  }
}
```

### 使用 `solargraph`

`solargraph` 在 Ruby 扩展中默认启用。

### 使用 `ruby-lsp`

要切换到 `ruby-lsp`，请将以下内容添加到你的 `settings.json` 中：

```json [settings]
{
  "languages": {
    "Ruby": {
      "language_servers": ["ruby-lsp", "!solargraph", "!rubocop", "..."]
    }
  }
}
```

这将禁用 `solargraph` 和 `rubocop`，并启用 `ruby-lsp`。

### 使用 `rubocop`

Ruby 扩展还支持 `rubocop` 语言服务器，用于问题检测和自动修正。

要启用它，请将以下内容添加到你的 `settings.json` 中：

```json [settings]
{
  "languages": {
    "Ruby": {
      "language_servers": ["ruby-lsp", "rubocop", "!solargraph", "..."]
    }
  }
}
```

或者，你也可以通过在 `settings.json` 中添加以下内容来禁用 `ruby-lsp` 并启用 `solargraph` 和 `rubocop`：

```json [settings]
{
  "languages": {
    "Ruby": {
      "language_servers": ["solargraph", "rubocop", "!ruby-lsp", "..."]
    }
  }
}
```

## 设置 `solargraph`

Solargraph 默认禁用格式化和诊断功能。我们可以通过在 `settings.json` 中添加以下内容来让 Zed 启用这些功能：

```json [settings]
{
  "lsp": {
    "solargraph": {
      "initialization_options": {
        "diagnostics": true,
        "formatting": true
      }
    }
  }
}
```

### 配置

Solargraph 会从项目根目录中名为 `.solargraph.yml` 的配置文件读取配置。有关此文件的更多信息，请参阅 [Solargraph 配置文档](https://solargraph.org/guides/configuration)。

## 设置 `ruby-lsp`

你可以将 Ruby LSP 配置传递给 `initialization_options`，例如：

LSP 服务器和代码操作也可以针对特定项目进行配置。例如，若要在特定项目中使用 [standardrb/standard](https://github.com/standardrb/standard) 作为格式化工具和代码检查器，请将以下内容添加到项目仓库中的配置文件：

```json
{
  "lsp": {
    "standard-rb": {
      "command": "standardrb",
      "args": ["--lsp"]
    }
  }
}
```

## 配置 Rubocop LSP

Rubocop 默认禁用不安全自动修正功能。我们可以通过将以下配置添加到用户设置中，指示 Zed 启用该功能：

```json
{
  "lsp": {
    "rubocop": {
      "settings": {
        "unsafe_autocorrect": true
      }
    }
  }
}
```

## 配置 Sorbet

[Sorbet](https://sorbet.org/) 是一款流行的 Ruby 静态类型检查器，内置语言服务器功能。

要启用 Sorbet，请将 `\"sorbet\"` 添加到 `settings.json` 中 Ruby 的 `language_servers` 列表。若计划将 Sorbet 作为主要语言服务器使用，或准备将其与其他 LSP 搭配使用以实现类型检查等特定功能，建议停用其他语言服务器。

```json [settings]
{
  "languages": {
    "Ruby": {
      "language_servers": [
        "ruby-lsp",
        "sorbet",
        "!rubocop",
        "!solargraph",
        "..."
      ]
    }
  }
}
```

关于安装 Sorbet、在项目中配置及其行为设置的所有方面，请参阅 [Sorbet 官方文档](https://sorbet.org/docs/overview)。

## 配置 Steep

[Steep](https://github.com/soutaro/steep) 是一个 Ruby 静态类型检查器，它使用 RBS 文件来定义类型。

要启用 Steep，请将 `\"steep\"` 添加到 `settings.json` 中 Ruby 的 `language_servers` 列表。根据您的需求，可能需要调整顺序或禁用其他 LSP。

```json [settings]
{
  "languages": {
    "Ruby": {
      "language_servers": [
        "ruby-lsp",
        "steep",
        "!solargraph",
        "!rubocop",
        "..."
      ]
    }
  }
}
```

## 配置 Herb

`Herb` 默认已针对 `HTML+ERB` 语言启用。

## 在 Ruby 中使用 Tailwind CSS 语言服务器

可以在 Ruby 和 ERB 文件中使用 [Tailwind CSS 语言服务器](https://github.com/tailwindlabs/tailwindcss-intellisense/tree/HEAD/packages/tailwindcss-language-server#readme)。

为此，您需要配置语言服务器，使其知晓在 Ruby/ERB 文件中查找 CSS 类的位置，只需将以下内容添加到您的 `settings.json` 中：

```json [settings]
{
  "languages": {
    "Ruby": {
      "language_servers": ["tailwindcss-language-server", "..."]
    }
  },
  "lsp": {
    "tailwindcss-language-server": {
      "settings": {
        "experimental": {
          "classRegex": ["\\bclass:\\s*['\"]([^'\"]*)['\"]"]
        }
      }
    }
  }
}
```

通过此设置，您将在 ERB 文件内的 HTML 属性中以及位于 `class:` 键之后的 Ruby/ERB 字符串中获得 Tailwind CSS 类的补全建议。例如：

## 运行测试

要在 Ruby 项目中运行测试，您可以在本地的 `.zed/tasks.json` 配置文件中设置自定义任务。这些任务可以配置为与不同的测试框架协同工作，例如 Minitest、RSpec、quickdraw 和 tldr。以下是一些示例，展示如何在编辑器中设置这些任务来运行测试。

### 使用 Rails 的 Minitest

```json [tasks]
[
  {
    "label": "test $ZED_RELATIVE_FILE -n /$ZED_CUSTOM_RUBY_TEST_NAME/",
    "command": "bin/rails",
    "args": [
      "test",
      "$ZED_RELATIVE_FILE",
      "-n",
      "\"$ZED_CUSTOM_RUBY_TEST_NAME\""
    ],
    "cwd": "$ZED_WORKTREE_ROOT",
    "tags": ["ruby-test"]
  }
]
```

### Minitest

普通的 Minitest 不支持按行号运行测试，只能按名称运行，因此我们需要改用 `$ZED_CUSTOM_RUBY_TEST_NAME`：

```json [tasks]
[
  {
    "label": "-Itest $ZED_RELATIVE_FILE -n /$ZED_CUSTOM_RUBY_TEST_NAME/",
    "command": "bundle",
    "args": [
      "exec",
      "ruby",
      "-Itest",
      "$ZED_RELATIVE_FILE",
      "-n",
      "\"$ZED_CUSTOM_RUBY_TEST_NAME\""
    ],
    "cwd": "$ZED_WORKTREE_ROOT",
    "tags": ["ruby-test"]
  }
]
```

### RSpec

```json [tasks]
[
  {
    "label": "test $ZED_RELATIVE_FILE:$ZED_ROW",
    "command": "bundle",
    "args": ["exec", "rspec", "\"$ZED_RELATIVE_FILE:$ZED_ROW\""],
    "cwd": "$ZED_WORKTREE_ROOT",
    "tags": ["ruby-test"]
  }
]
```

类似的任务语法也可用于其他测试框架，例如 `quickdraw` 或 `tldr`。

## 调试

Ruby 扩展提供了用于调试 Ruby 代码的调试适配器。Zed 在界面和 `debug.json` 中为该适配器命名为 `rdbg`，其底层使用 [`debug`](https://github.com/ruby/debug) gem。该扩展采用与语言服务器相同的激活逻辑。

### 示例

#### 调试 Ruby 脚本

```json [debug]
[
  {
    "label": "Debug current file",
    "adapter": "rdbg",
    "request": "launch",
    "script": "$ZED_FILE",
    "cwd": "$ZED_WORKTREE_ROOT"
  }
]
```

#### 调试 Rails 服务器

```json [debug]
[
  {
    "label": "Debug Rails server",
    "adapter": "rdbg",
    "request": "launch",
    "command": "./bin/rails",
    "args": ["server"],
    "cwd": "$ZED_WORKTREE_ROOT",
    "env": {
      "RUBY_DEBUG_OPEN": "true"
    }
  }
]
```

## 格式化工具

### `erb-formatter`

要格式化 ERB 模板，您可以使用 `erb-formatter` 格式化工具。该工具通过 [`erb-formatter`](https://rubygems.org/gems/erb-formatter) gem 来实现 ERB 模板的格式化。

```json [settings]
{
  "HTML+ERB": {
    "formatter": {
      "external": {
        "command": "erb-formatter",
        "arguments": ["--stdin-filename", "{buffer_path}"]
      }
    }
  }
}
```