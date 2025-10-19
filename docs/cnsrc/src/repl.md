# 交互式编程环境

## 快速入门

将 [Jupyter 内核](https://docs.jupyter.org/en/latest/projects/kernels.html)的强大功能引入您的编辑器！Zed 内置的交互式编程环境让您能够在编辑器中以类似笔记本的方式，使用自己的文本文件进行交互式代码运行。

## 安装

Zed 支持运行多种语言的代码。要开始使用，您需要为您想要使用的语言安装一个内核。

**当前支持的语言：**

- [Python (ipykernel)](#python)
- [TypeScript (Deno)](#typescript-deno)
- [R (Ark)](#r-ark)
- [R (Xeus)](#r-xeus)
- [Julia](#julia)
- [Scala (Almond)](#scala)

安装完成后，您可以在相应语言文件或其他支持这些语言的环境（如Markdown）中使用REPL。若您最近添加了内核，请执行`repl: refresh kernelspecs`命令使其在编辑器中可用。

## 使用REPL

要启动REPL，请打开目标语言文件并使用`repl: run`命令（macOS系统默认为`ctrl-shift-enter`）来运行代码块、选中内容或单行代码。您也可以点击工具栏中的REPL图标。

`repl: run`命令将在您选中的内容上执行，执行结果会显示在选中内容下方。

可通过`repl: clear outputs`命令或工具栏的REPL菜单清除输出内容。

### 单元格模式

Zed支持将[笔记本作为脚本](https://jupytext.readthedocs.io/en/latest/formats-scripts.html)使用，在Python中采用`# %%`作为代码块分隔符，在TypeScript中则使用`// %%`。这让你能够将代码写入单一文件，并像操作笔记本那样逐单元运行。

通过`repl: run`命令，位于`# %%`标记之间的每个代码块都将作为独立单元运行。

```python
# %% Cell 1
import time
import numpy as np

# %% Cell 2
import matplotlib.pyplot as plt
import matplotlib.pyplot as plt
from matplotlib import style
style.use('ggplot')
```

## 语言特定说明

### Python {#python}

#### 全局环境

<div class="warning">

在macOS系统中，系统自带的Python_无法直接使用_。你需要通过[pyenv](https://github.com/pyenv/pyenv?tab=readme-ov-file#installation)进行配置，或使用虚拟环境。

</div>

若要使当前Python环境具备可用内核，请运行：

```sh
pip install ipykernel
python -m ipykernel install --user
```

#### Conda环境

#### 使用 pip 创建虚拟环境

```sh
source activate myenv
pip install ipykernel
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

### R（Ark 内核）{#r-ark}

通过下载适用于您操作系统的发行版来安装 [Ark](https://github.com/posit-dev/ark/releases)。例如，对于 macOS，只需解压 `ark` 二进制文件并将其放入 `/usr/local/bin` 目录中。然后运行：

```sh
ark --install
```

### R（Xeus 内核）{#r-xeus}

- 安装 [Xeus-R](https://github.com/jupyter-xeus/xeus-r)
- 安装 Zed 的 R 扩展（在 Zed 扩展中搜索 `R`）

<!--
TBD: Improve R REPL (Ark Kernel) instructions
-->

### TypeScript：Deno {#typescript-deno}

- [安装 Deno](https://docs.deno.com/runtime/manual/getting_started/installation/)，然后安装 Deno 的 Jupyter 内核：

```sh
deno jupyter --install
```

### Julia

- 从[官方网站](https://julialang.org/downloads/)下载并安装Julia。
- 为Zed安装Julia扩展（在Zed扩展中搜索`julia`）。

### Scala

- 使用`scoop`（Coursier）[安装Scala](https://www.scala-lang.org/download/)：
  - `scoop install coursier`
- REPL（Almond）[设置说明](https://almond.sh/docs/quick-start-install)：
  - 安装OpenJDK（Eclipse官方OpenJDK二进制文件）
  - `cs setup`
  - `cs install almond`

## 更改每种语言使用的内核 {#changing-kernels}

Zed会自动检测系统中可用的内核。如需为语言配置不同的默认内核，可以在`settings.json`中为任何支持的语言指定内核。

## 调试内核规格

可用内核可通过 `repl: sessions` 命令查看。要刷新可运行的内核，请使用 `repl: refresh kernelspecs` 命令。

若已安装 `jupyter`，可运行 `jupyter kernelspec list` 查看可用内核。

```sh
$ jupyter kernelspec list
Available kernels:
  ark                   /Users/z/Library/Jupyter/kernels/ark
  conda-base            /Users/z/Library/Jupyter/kernels/conda-base
  deno                  /Users/z/Library/Jupyter/kernels/deno
  python-chatlab-dev    /Users/z/Library/Jupyter/kernels/python-chatlab-dev
  python3               /Users/z/Library/Jupyter/kernels/python3
  ruby                  /Users/z/Library/Jupyter/kernels/ruby
  rust                  /Users/z/Library/Jupyter/kernels/rust
```

> 注意：Zed 会尽力利用 `sys.prefix` 和 `CONDA_PREFIX` 在 Python 环境中查找内核。如需明确控制，请在对应环境中直接运行 `python -m ipykernel install --user --name myenv --display-name "Python (myenv)"` 来安装内核。