# 如何在 Zed 中配置 Python

Zed 原生支持 Python。

- 语法解析器：[tree-sitter-python](https://github.com/zed-industries/tree-sitter-python)
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

要了解更多信息，请访问 [Astral 安装指南](https://docs.astral.sh/uv/getting-started/installation/)。

- Homebrew 安装：

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

Zed 将自动识别 `.py` 文件，这得益于其原生 tree-sitter-python 解析器，无需任何插件或手动配置。

### 步骤 2：使用集成终端（可选）

Zed 内置了一个集成终端，可通过底部面板访问。如果 Zed 检测到您的项目正在使用[虚拟环境](#virtual-environments)，新创建的终端将自动激活该环境。您可以通过 [`detect_venv`](../configuring-zed.md#terminal-detect_venv) 设置来配置此行为。

## 在 Zed 中配置 Python 语言服务器

Zed 预置了多种 Python 语言服务器。默认情况下，[basedpyright](https://github.com/DetachHead/basedpyright) 作为主语言服务器，而 [Ruff](https://github.com/astral-sh/ruff) 用于代码格式化和静态检查。

其他内置语言服务器包括：

- [Ty](https://docs.astral.sh/ty/)——由Astral开发的新兴语言服务器，专为高速运行而构建。
- [Pyright](https://github.com/microsoft/pyright)——基于basedpyright的核心基础。
- [PyLSP](https://github.com/python-lsp/python-lsp-server)——基于插件的语言服务器，可与`pycodestyle`、`autopep8`和`yapf`等工具集成。

这些功能默认处于禁用状态，但可以在设置中启用。例如：

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

详见：[语言服务器使用指南](https://zed.dev/docs/configuring-languages#working-with-language-servers)获取关于启用和禁用语言服务器的更多信息。

### Basedpyright

自Zed v0.204.0版本起，[basedpyright](https://docs.basedpyright.com/latest/)已成为Zed的主要Python语言服务器。它提供核心语言服务器功能，包括代码导航（跳转到定义/查找所有引用）和类型检查。与Pyright相比，它额外支持更多语言服务器特性（如内联提示）和检查规则。

需要注意的是，虽然独立运行的basedpyright默认采用`recommended` [类型检查模式](https://docs.basedpyright.com/latest/benefits-over-pyright/better-defaults/#typecheckingmode)，但Zed会将其配置为默认使用限制较少的`standard`模式，这与Pyright的行为保持一致。您可以通过在`pyrightconfig.json`或`pyproject.toml`中设置`typeCheckingMode`参数来覆盖Zed的默认配置，为项目指定类型检查模式。下文将详细介绍如何配置basedpyright。

#### Basedpyright配置说明

basedpyright 从两种不同的配置源读取配置选项：

- 语言服务器设置（"工作区配置"），需在每个编辑器中单独配置（在 Zed 中需使用 `settings.json`），但会应用于该编辑器打开的所有项目
- 配置文件（`pyrightconfig.json`、`pyproject.toml`），这些配置与编辑器无关，但仅对所在项目生效

基本原则是：仅在使用基于编辑器的 basedpyright 时相关的选项必须在语言服务器设置中配置，而即使通过[命令行工具](https://docs.basedpyright.com/latest/configuration/command-line/)运行时也相关的选项则必须在配置文件中设置。行内提示相关的设置属于第一类，而[诊断类别](https://docs.basedpyright.com/latest/configuration/config-files/#diagnostic-categories)设置则属于第二类。

以下是两种配置方式的示例。关于完整可用选项列表，请参阅basedpyright文档中的[语言服务器设置](https://docs.basedpyright.com/latest/configuration/language-server-settings/)和[配置文件](https://docs.basedpyright.com/latest/configuration/config-files/)说明。

##### 语言服务器设置

在Zed中配置basedpyright语言服务器设置时，需在`settings.json`的`lsp`段落中进行设置。

例如，若需实现以下功能：
- 诊断工作区中的所有文件（而非默认仅诊断已打开文件）
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

basedpyright 从 `pyrightconfig.json` 配置文件以及 `pyproject.toml` 清单的 `[tool.basedpyright]` 和 `[tool.pyright]` 部分读取项目特定配置。若两处均存在配置，`pyrightconfig.json` 的配置将覆盖 `pyproject.toml`。

以下示例 `pyrightconfig.json` 文件配置 basedpyright 使用 `strict` 类型检查模式，且不对 `__pycache__` 目录中的任何文件发出诊断信息：

```json [settings]
{
  "typeCheckingMode": "strict",
  "ignore": ["**/__pycache__"]
}
```

### PyLSP

[python-lsp-server](https://github.com/python-lsp/python-lsp-server/)（通常称为 PyLSP）默认集成了多个外部工具（autopep8、mccabe、pycodestyle、yapf），而其他工具（如 flake8、pylint）为可选功能，需显式启用并配置。

更多信息请参阅 [Python 语言服务器配置](https://github.com/python-lsp/python-lsp-server/blob/develop/CONFIGURATION.md)。

## 虚拟环境

[虚拟环境](https://docs.python.org/3/library/venv.html)是一个实用工具，它能够以与同一台机器上其他项目隔离的方式，为特定项目固定Python版本和依赖包集合。基于语言无关的[工具链](../toolchains.md)概念，Zed内置了对发现、配置和激活虚拟环境的支持。

请注意，如果您安装了全局Python环境，在Zed中也会被视为一个工具链。

### 创建虚拟环境

如果您的项目尚未设置虚拟环境，可以通过以下方式创建：

```bash
python3 -m venv .venv
```

或者，如果您正在使用`uv`，首次运行`uv sync`时将会自动创建虚拟环境。

### Zed如何使用Python工具链

Zed会通过以下方式为您的项目使用选定的Python工具链：

- 内置语言服务器将自动配置工具链的 Python 解释器路径及虚拟环境（若适用）。这对依赖项解析至关重要。（请注意，目前扩展提供的语言服务器尚不支持此类自动配置。）
- Python 任务（如 pytest 测试）将使用工具链的 Python 解释器运行。
- 若工具链为虚拟环境，在 Zed 集成终端中启动新 shell 时，将自动运行该环境的激活脚本，方便您使用选定的 Python 解释器和依赖项集合。
- 若内置语言服务器已安装在当前虚拟环境中，将优先使用该环境中的二进制文件，而非 Zed 自动安装的私有二进制文件。此规则同样适用于 debugpy。

### 选择工具链

对于大多数项目，Zed 会自动选择合适的 Python 工具链。在包含多个虚拟环境的复杂项目中，可能需要手动覆盖此选择。您可以使用[工具链选择器](../toolchains.md#selecting-toolchains)从 Zed 发现的列表中选择工具链，如果所需工具链不在列表中，也可以[手动指定工具链路径](../toolchains.md#adding-toolchains-manually)。

## 代码格式化与静态检查

Zed 为 Python 代码提供 [Ruff](https://docs.astral.sh/ruff/) 格式化器和静态检查工具。（具体而言，Zed 通过 `ruff server` 子命令将 Ruff 作为 LSP 服务器运行。）格式化和静态检查功能均默认启用，包括保存时自动格式化。

### 配置格式化功能

您可以在 `settings.json` 中针对 Python 文件禁用保存时自动格式化：

```json [settings]
{
  "languages": {
    "Python": {
      "format_on_save": "off"
    }
  }
}
```

或者，你可以使用 `black` 命令行工具进行 Python 代码格式化，同时保持 Ruff 的代码检查功能：

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

与 basedpyright 类似，Ruff 在 Zed 中运行时既会读取语言服务器设置，也会读取配置文件（`ruff.toml`）中的选项。但与 basedpyright 不同的是，_所有_选项都可以在这两个位置进行配置。因此，配置位置的选择取决于你是希望配置在项目间共享但仅限 Zed 使用（此时应使用语言服务器设置），还是希望配置仅针对特定项目但对所有 Ruff 调用生效（此时应使用 `ruff.toml`）。

以下是在Zed的`settings.json`中使用语言服务器设置来禁用所有Ruff检查（同时仍使用Ruff作为格式化工具）的示例：

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

这里还有一个包含代码检查和格式化选项的`ruff.toml`示例，改编自Ruff官方文档：

```toml
[lint]
# Avoid enforcing line-length violations (`E501`)
ignore = ["E501"]

[format]
# Use single quotes when formatting.
quote-style = "single"
```

更多详细信息，请参阅Ruff文档中关于[配置文件](https://docs.astral.sh/ruff/configuration/)、[语言服务器设置](https://docs.astral.sh/ruff/editors/settings/)以及[选项列表](https://docs.astral.sh/ruff/settings/)的说明。

## 调试

Zed通过`debugpy`适配器支持Python调试。您可以从零配置开始，或在`.zed/debug.json`中定义自定义启动配置。

### 零配置启动调试

Zed能自动检测可调试的Python入口点。按下F4（或通过命令面板运行debugger: start）即可查看当前项目的可用选项。
支持以下类型：

- Python脚本
- 模块
- pytest测试

Zed底层使用`debugpy`，无需手动配置适配器。

### 定义自定义调试配置

如需可复用的配置方案，请在项目根目录创建`.zed/debug.json`文件。这将让您更精细地控制Zed运行和调试代码的方式。

- [debugpy配置文档](https://github.com/microsoft/debugpy/wiki/Debug-configuration-settings#launchattach-settings)

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

这运行当前在编辑器中打开的文件。

#### 调试 Flask 应用

对于使用 Flask 的项目，您可以定义完整的启动配置：

```
.venv/
app/
  init.py
  main.py
  routes.py
templates/
  index.html
static/
  style.css
requirements.txt
```

...可以使用以下配置：

```json [debug]
[
  {
    "label": "Python: Flask",
    "adapter": "Debugpy",
    "request": "launch",
    "module": "app",
    "cwd": "$ZED_WORKTREE_ROOT",
    "env": {
      "FLASK_APP": "app",
      "FLASK_DEBUG": "1"
    },
    "args": [
      "run",
      "--reload", // Enables Flask reloader that watches for file changes
      "--debugger" // Enables Flask debugger
    ],
    "autoReload": {
      "enable": true
    },
    "jinja": true,
    "justMyCode": true
  }
]
```

这些可以组合使用，为 Web 服务器、测试运行器或自定义脚本定制体验。

## 故障排除与维护高效的 Python 环境

Zed 的设计初衷是尽量减少配置负担，但偶尔仍会出现问题——尤其是在环境、语言服务器或工具链方面。以下方法可确保您的 Python 环境持续稳定运行。

### 解决语言服务器启动问题

若语言服务器无响应，或出现诊断提示、自动补全等功能失效时：

- 检查 Zed 日志（使用 {#action zed::OpenLog} 操作）中与您尝试使用的语言服务器相关的错误。如果语言服务器完全未能启动，这里最可能找到有用信息。
- 使用语言服务器日志视图来了解受影响语言服务器的生命周期。可通过 {#action dev::OpenLanguageServerLogs} 操作访问此视图，或点击状态栏中的闪电图标并选择对应语言服务器。该视图中最有价值的数据包括：
  - "服务器日志"：显示语言服务器输出的任何错误信息
  - "服务器信息"：显示语言服务器启动方式的详细信息
- 确认您的 `settings.json` 或 `pyrightconfig.json` 语法正确。
- 重启 Zed 以重新初始化语言服务器连接，或使用 {#action editor::RestartLanguageServer} 操作尝试重启语言服务器。

如果语言服务器无法解析导入，并且你正在使用虚拟环境，请确保在环境选择器中选择了正确的环境。你可以使用“服务器信息”视图来确认 Zed 发送给语言服务器的是哪个虚拟环境——留意末尾的`* Configuration`部分。