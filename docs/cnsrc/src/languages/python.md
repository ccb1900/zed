# 如何在 Zed 中配置 Python

Zed 原生支持 Python。

- 语法分析器：[tree-sitter-python](https://github.com/zed-industries/tree-sitter-python)
- 语言服务器：
  - [DetachHead/basedpyright](https://github.com/DetachHead/basedpyright)
  - [astral-sh/ruff](https://github.com/astral-sh/ruff)
  - [astral-sh/ty](https://github.com/astral-sh/ty)
  - [microsoft/pyright](https://github.com/microsoft/pyright)
  - [python-lsp/python-lsp-server](https://github.com/python-lsp/python-lsp-server) (PyLSP)
- 调试适配器：[debugpy](https://github.com/microsoft/debugpy)

## 安装 Python

开始前需确保已安装 Zed 和 Python。

### 步骤一：安装 Python

Zed 不内置 Python 运行环境，需自行安装。
请选择以下任一方式：

- uv（推荐）

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

如需了解更多信息，请访问 [Astral 安装指南](https://docs.astral.sh/uv/getting-started/installation/)。

- Homebrew 安装方式：

```bash
brew install python
```

- Python.org 安装程序：从 [python.org/downloads](https://python.org/downloads) 下载最新版本。

### 步骤 2：验证 Python 安装

确认 Python 已安装并在终端中可用：

```bash
python3 --version
```

您应该会看到类似 `Python 3.x.x` 的输出。

## 在 Zed 中打开首个 Python 项目

安装 Zed 和 Python 后，打开包含 Python 代码的文件夹即可开始工作。

### 步骤 1：启动包含 Python 项目的 Zed

打开 Zed。  
从菜单栏选择 文件 > 打开文件夹，或通过终端启动：

```bash
zed path/to/your/project
```

Zed 将自动识别 `.py` 文件，这得益于其原生 tree-sitter-python 解析器，无需插件或手动配置。

### 步骤 2：使用集成终端（可选）

Zed 内置集成终端，可通过底部面板访问。当检测到项目使用[虚拟环境](#virtual-environments)时，新建终端将自动激活该环境。您可通过 [`detect_venv`](../configuring-zed.md#terminal-detect_venv) 设置配置此行为。

## 在 Zed 中配置 Python 语言服务器

Zed 预置了多种 Python 语言服务器。默认配置下：
- 主语言服务器为 [basedpyright](https://github.com/DetachHead/basedpyright)
- 代码格式化和语法检查由 [Ruff](https://github.com/astral-sh/ruff) 实现

其他内置语言服务器包括：

- [Ty](https://docs.astral.sh/ty/)——来自Astral的新兴语言服务器，专为速度打造。
- [Pyright](https://github.com/microsoft/pyright)——basedpyright的基础版本。
- [PyLSP](https://github.com/python-lsp/python-lsp-server)——一款基于插件的语言服务器，可与`pycodestyle`、`autopep8`和`yapf`等工具集成。

这些功能默认处于关闭状态，但可在设置中启用。例如：

```json [settings]
{
  "languages": {
    "Python": {
      "language_servers": [
        // Disable basedpyright and enable Ty, and otherwise
        // use the default configuration.
        "ty",
        "!basedpyright",
        "..."
      ]
    }
  }
}
```

更多关于如何启用和禁用语言服务器的信息，请参阅：[使用语言服务器](https://zed.dev/docs/configuring-languages#working-with-language-servers)。

### Basedpyright

自 Zed v0.204.0 起，[basedpyright](https://docs.basedpyright.com/latest/) 成为 Zed 的主要 Python 语言服务器。它提供核心语言服务器功能，如代码导航（跳转到定义/查找所有引用）和类型检查。与 Pyright 相比，它增加了对额外语言服务器功能（如内联提示）和检查规则的支持。

请注意，虽然独立运行的 basedpyright 默认使用 `recommended` [类型检查模式](https://docs.basedpyright.com/latest/benefits-over-pyright/better-defaults/#typecheckingmode)，但 Zed 将其默认配置为限制较少的 `standard` 模式，以匹配 Pyright 的行为。您可以通过 `pyrightconfig.json` 或 `pyproject.toml` 中的 `typeCheckingMode` 设置来为项目配置类型检查模式，这将覆盖 Zed 的默认设置。继续阅读以了解有关配置 basedpyright 的更多详细信息。

#### Basedpyright 配置

basedpyright 从两种不同类型的源读取配置选项：

- 语言服务器设置（"工作区配置"），这类配置需按编辑器进行设置（在 Zed 中需使用 `settings.json`），但会应用于该编辑器打开的所有项目
- 配置文件（`pyrightconfig.json`、`pyproject.toml`），这类配置与编辑器无关，但仅对放置配置文件的特定项目生效

根据经验法则：仅在使用基于编辑器的 basedpyright 时才相关的选项必须在语言服务器设置中配置；而即使通过[命令行工具](https://docs.basedpyright.com/latest/configuration/command-line/)运行时也相关的选项则必须在配置文件中设置。内联提示相关的设置属于第一类，而[诊断类别](https://docs.basedpyright.com/latest/configuration/config-files/#diagnostic-categories)设置则属于第二类。

以下是两种配置类型的示例。完整可用选项请参阅 basedpyright 文档中的[语言服务器设置](https://docs.basedpyright.com/latest/configuration/language-server-settings/)与[配置文件](https://docs.basedpyright.com/latest/configuration/config-files/)章节。

##### 语言服务器设置

在 Zed 中配置 basedpyright 语言服务器时，需在 `settings.json` 的 `lsp` 章节进行设置。

例如，若需实现以下功能：
- 诊断工作区所有文件（而非默认仅诊断已打开文件）
- 禁用函数参数的内联提示

可使用如下配置：

```json [settings]
{
  "lsp": {
    "basedpyright": {
      "settings": {
        "analysis": {
          "diagnosticMode": "workspace",
          "inlayHints.callArgumentNames": false
        }
      }
    }
  }
}
```

##### 配置文件

basedpyright 从 `pyrightconfig.json` 配置文件中读取项目特定配置，并从 `pyproject.toml` 清单的 `[tool.basedpyright]` 和 `[tool.pyright]` 部分获取配置。若两处均存在配置，则 `pyrightconfig.json` 的配置会覆盖 `pyproject.toml`。

以下是一个 `pyrightconfig.json` 配置文件示例，该配置使 basedpyright 使用 `strict` 类型检查模式，且不对 `__pycache__` 目录中的任何文件发出诊断信息：

```json [settings]
{
  "typeCheckingMode": "strict",
  "ignore": ["**/__pycache__"]
}
```

### PyLSP

[python-lsp-server](https://github.com/python-lsp/python-lsp-server/)（通常称为 PyLSP）默认集成了多个外部工具（autopep8、mccabe、pycodestyle、yapf），其他工具（如 flake8、pylint）为可选功能，需显式启用并配置。

更多信息请参阅 [Python 语言服务器配置](https://github.com/python-lsp/python-lsp-server/blob/develop/CONFIGURATION.md)。

## 虚拟环境

[虚拟环境](https://docs.python.org/3/library/venv.html)是一个实用工具，它能够以隔离于同一台机器上其他项目的方式，为特定项目固定Python版本和依赖包集合。基于与语言无关的[工具链](../toolchains.md)概念，Zed内置了对虚拟环境的发现、配置和激活功能。

请注意，如果您安装了全局Python环境，在Zed中也会被视为一个工具链。

### 创建虚拟环境

如果您的项目尚未设置虚拟环境，可以通过以下方式创建：

```bash
python3 -m venv .venv
```

或者，如果您使用`uv`，首次运行`uv sync`时将会自动创建虚拟环境。

### Zed如何使用Python工具链

Zed会通过以下方式为您的项目使用选定的Python工具链：

- 内置语言服务器将自动配置工具链的Python解释器路径，若适用则包括虚拟环境。这一点至关重要，以便它们能够解析依赖关系。（请注意，目前无法像这样自动配置由扩展提供的语言服务器。）
- Python任务（例如pytest测试）将使用工具链的Python解释器运行。
- 如果工具链是虚拟环境，当您在Zed集成终端中启动新shell时，将自动运行该环境的激活脚本，让您便捷地访问所选Python解释器及依赖项集合。
- 如果内置语言服务器安装在当前活动的虚拟环境中，则将使用该二进制文件，而非Zed自动安装的私有二进制文件。这一点同样适用于debugpy。

### 选择工具链

对于大多数项目，Zed 会自动选择合适的 Python 工具链。在具有多个虚拟环境的复杂项目中，可能需要手动覆盖此选择。您可以使用[工具链选择器](../toolchains.md#selecting-toolchains)从 Zed 发现的列表中选择工具链，若列表中没有所需工具链，也可[手动指定工具链路径](../toolchains.md#adding-toolchains-manually)。

## 代码格式化与静态检查

Zed 为 Python 代码提供 [Ruff](https://docs.astral.sh/ruff/) 格式化及静态检查功能。（具体而言，Zed 通过 `ruff server` 子命令将 Ruff 作为 LSP 服务器运行。）格式化和静态检查功能均默认开启，包含保存时自动格式化。

### 配置格式化功能

您可以在 `settings.json` 中为 Python 文件关闭保存时自动格式化：

```json [settings]
{
  "languages": {
    "Python": {
      "format_on_save": "off"
    }
  }
}
```

或者，你可以使用 `black` 命令行工具进行 Python 格式化，同时保持 Ruff 用于代码检查：

```json [settings]
{
  "languages": {
    "Python": {
      "formatter": {
        "external": {
          "command": "black",
          "arguments": ["--stdin-filename", "{buffer_path}", "-"]
        }
      }
      // Or use `"formatter": null` to disable formatting entirely.
    }
  }
}
```

### 配置 Ruff

与 basedpyright 类似，Ruff 在 Zed 中使用时会同时从 Zed 的语言服务器设置和配置文件（`ruff.toml`）中读取选项。但与 basedpyright 不同的是，_所有_选项都可以在这两个位置进行配置，因此选择将 Ruff 配置放在何处取决于你是希望它在项目之间共享但特定于 Zed（此时应使用语言服务器设置），还是希望它特定于一个项目但适用于所有 Ruff 调用（此时应使用 `ruff.toml`）。

以下是在 Zed 的 `settings.json` 中使用语言服务器设置来禁用所有 Ruff 检查（同时仍使用 Ruff 作为格式化工具）的示例：

```json [settings]
{
  "lsp": {
    "ruff": {
      "initialization_options": {
        "settings": {
          "exclude": ["*"]
        }
      }
    }
  }
}
```

以下是一个来自 Ruff 文档的 `ruff.toml` 示例，其中包含代码检查和格式化选项：

```toml
[lint]
# Avoid enforcing line-length violations (`E501`)
ignore = ["E501"]

[format]
# Use single quotes when formatting.
quote-style = "single"
```

更多详细信息，请参阅 Ruff 文档中关于[配置文件](https://docs.astral.sh/ruff/configuration/)和[语言服务器设置](https://docs.astral.sh/ruff/editors/settings/)的部分，以及[选项列表](https://docs.astral.sh/ruff/settings/)。

## 调试

Zed 通过 `debugpy` 适配器支持 Python 调试。您可以直接无配置启动调试，或在 `.zed/debug.json` 中定义自定义启动配置。

### 无配置启动调试

Zed 能自动检测可调试的 Python 入口点。按 F4（或通过命令面板运行 debugger: start）即可查看当前项目的可用选项。
支持以下类型：

- Python 脚本
- 模块
- pytest 测试

Zed 底层使用 `debugpy`，无需手动配置适配器。

### 定义自定义调试配置

如需可复用的调试设置，请在项目根目录创建 `.zed/debug.json` 文件。这将让您更灵活地控制代码运行与调试方式。

- [debugpy 配置文档](https://github.com/microsoft/debugpy/wiki/Debug-configuration-settings#launchattach-settings)

#### 调试当前文件

```json [debug]
[
  {
    "label": "Python Active File",
    "adapter": "Debugpy",
    "program": "$ZED_FILE",
    "request": "launch"
  }
]
```

这可以运行编辑器中当前打开的文件。

#### 调试 Flask 应用

对于使用 Flask 的项目，您可以定义完整的启动配置：

[[代码块_0]]

...可以使用以下配置：

[[代码块_1]]

这些配置可以组合使用，以便为 Web 服务器、测试运行器或自定义脚本定制体验。

## 排查问题并维护高效的 Python 环境

Zed 的设计初衷是尽量减少配置负担，但偶尔仍可能出现问题——尤其是在环境、语言服务器或工具链方面。以下方法可确保你的 Python 环境保持稳定运行。

### 解决语言服务器启动问题

若语言服务器无响应，或出现诊断提示、自动补全等功能失效的情况：

- 检查你的 Zed 日志（使用 {#action zed::OpenLog} 操作），查找与你要使用的语言服务器相关的错误。如果语言服务器完全未能启动，这里最有可能找到有用信息。
- 使用语言服务器日志视图来了解受影响语言服务器的生命周期。你可以通过 {#action dev::OpenLanguageServerLogs} 操作访问该视图，或点击状态栏中的闪电图标并选择你的语言服务器。此视图中最有用的数据包括：
  - “服务器日志”，显示语言服务器输出的任何错误
  - “服务器信息”，显示语言服务器启动方式的详细信息
- 验证你的 `settings.json` 或 `pyrightconfig.json` 语法是否正确。
- 重启 Zed 以重新初始化语言服务器连接，或使用 {#action editor::RestartLanguageServer} 尝试重启语言服务器

如果语言服务器无法解析导入，并且你正在使用虚拟环境，请确保在环境选择器中选择了正确的环境。你可以通过“服务器信息”视图确认 Zed 发送给语言服务器的虚拟环境——留意末尾的 `* Configuration` 部分。