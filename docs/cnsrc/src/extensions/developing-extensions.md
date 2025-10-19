# 开发扩展

## 扩展功能

扩展可以为 Zed 添加以下功能：

- [语言支持](./languages.md)
- [调试器](./debugger-extensions.md)
- [主题](./themes.md)
- [图标主题](./icon-themes.md)
- [斜杠命令](./slash-commands.md)
- [MCP 服务器](./mcp-extensions.md)

## 本地开发扩展

在开始为 Zed 开发扩展之前，请确保[通过 rustup 安装 Rust](https://www.rust-lang.org/tools/install)。

> 必须通过 rustup 安装 Rust。如果您通过 homebrew 或其他方式安装了 Rust，开发扩展将无法正常工作。

在开发扩展时，您可以通过将其安装为_开发扩展_来在 Zed 中使用，而无需发布它。

在扩展页面，点击 `Install Dev Extension` 按钮（或使用 {#action zed::InstallDevExtension} 操作），然后选择包含您扩展的目录。

如需排查问题，你可查看 Zed.log（{#action zed::OpenLog}）获取更多输出信息。若需调试输出，请通过命令行使用 `zed --foreground` 参数重新启动 Zed，这将显示更详细的 INFO 级别日志记录。

若已安装同名已发布扩展，你的开发版扩展将覆盖原扩展。

安装后，`Extensions` 页面将显示上游扩展“已被开发版扩展覆盖”。

同名预装扩展需先卸载才能安装开发版扩展。详情参阅 [#31106](https://github.com/zed-industries/zed/issues/31106)。

## Zed 扩展目录结构

Zed 扩展是一个包含 `extension.toml` 的 Git 代码库。该文件必须包含以下扩展基础信息：

```toml
id = "my-extension"
name = "My extension"
version = "0.0.1"
schema_version = 1
authors = ["Your Name <you@example.com>"]
description = "My cool extension"
repository = "https://github.com/your-name/my-zed-extension"
```

除此之外，还有若干可选文件和目录可用于增强Zed扩展的功能。以下是一个提供全部功能的扩展目录结构示例：

```
my-extension/
  extension.toml
  Cargo.toml
  src/
    lib.rs
  languages/
    my-language/
      config.toml
      highlights.scm
  themes/
    my-theme.json
```

## WebAssembly

扩展的程序化部分使用Rust编写并编译为WebAssembly。要开发包含自定义代码的扩展，需包含如下所示的`Cargo.toml`：

```toml
[package]
name = "my-extension"
version = "0.0.1"
edition = "2021"

[lib]
crate-type = ["cdylib"]

[dependencies]
zed_extension_api = "0.1.0"
```

请使用 crates.io 上最新版本的 [[[代码块1]]](https://crates.io/crates/zed_extension_api)。确保它仍然与你希望支持的 [Zed 版本兼容](https://github.com/zed-industries/zed/blob/main/crates/extension_api#compatible-zed-versions)。

在你的 Rust 项目中的 [[代码块2]] 文件中，需要为你的扩展定义一个结构体并实现 [[代码块3]] 特性，同时使用 [[代码块4]] 宏来注册你的扩展：

[[代码块0]]

> [[代码块5]]/[[代码块6]] 会直接转发到 Zed 进程。为了查看来自你扩展的 [[代码块7]]/[[代码块8]] 输出，你可以在终端中使用 [[代码块9]] 标志启动 Zed。

## 复刻与克隆仓库

1. 复刻该仓库

> 注意：强烈建议将 `zed-industries/extensions` 代码库复刻到个人 GitHub 账户而非组织账户，这样 Zed 团队可以直接向你的 PR 推送必要修改，以加速发布流程。

2. 将代码库克隆到本地设备

```sh
# Substitute the url of your fork here:
# git clone https://github.com/zed-industries/extensions
cd extensions
git submodule init
git submodule update
```

## 扩展许可证要求

自 2025 年 10 月 1 日起，扩展代码库必须包含以下任一许可证：

- [MIT 许可证](https://opensource.org/license/mit)  
- [Apache 2.0 许可证](https://www.apache.org/licenses/LICENSE-2.0)

这将允许我们将基于你扩展代码生成的二进制文件分发给用户。  
若未包含有效许可证，后续添加或更新扩展的拉取请求将无法通过 CI 验证。

您的扩展程序许可证文件应位于扩展仓库的根目录。任何以 `LICENCE` 或 `LICENSE` 作为前缀（不区分大小写）的文件名都将被检查，以确保其符合已接受的许可证之一。具体可参阅[许可证验证源代码](https://github.com/zed-industries/extensions/blob/main/src/lib/license.js)。

> 此许可证要求仅适用于您的扩展代码本身（即编译成扩展二进制文件的代码）。
> 它不适用于您的扩展可能下载或与之交互的任何工具，例如语言服务器或其他外部依赖项。
> 如果您的仓库同时包含扩展代码和其他项目（如语言服务器），您无需重新许可这些其他项目——仅扩展代码需要采用上述已接受的许可证之一。

## 发布您的扩展

要发布扩展，请向 [ `zed-industries/extensions` 代码库](https://github.com/zed-industries/extensions) 提交拉取请求（PR）。

在你的拉取请求（PR）中，请执行以下操作：

1. 将你的扩展作为 Git 子模块添加到 `extensions/` 目录中：

```sh
git submodule add https://github.com/your-username/foobar-zed.git extensions/foobar
git add extensions/foobar
```

> 所有扩展子模块必须使用 HTTPS 网址，而非 SSH 网址（`git@github.com`）。

2. 在顶层的 `extensions.toml` 文件中为你的扩展添加一个新条目：

```toml
[my-extension]
submodule = "extensions/my-extension"
version = "0.0.1"
```

> 如果你的扩展位于子模块的子目录中，可以使用 `path` 字段指定扩展所在的位置。

3. 运行 `pnpm sort-extensions`，确保 `extensions.toml` 和 `.gitmodules` 已排序。

一旦你的 PR 被合并，该扩展将被打包并发布到 Zed 扩展注册表中。

> 扩展 ID 和名称不应包含 `zed` 或 `Zed`，因为它们都是 Zed 扩展。

## 更新扩展

要更新扩展，请向 [ `zed-industries/extensions` 代码库](https://github.com/zed-industries/extensions) 提交拉取请求。

在拉取请求中需完成以下操作：

1. 将扩展的子模块更新至新版本的提交节点
2. 在 `extensions.toml` 中更新该扩展的 `version` 字段
   - 确保 `version` 与 `extension.toml` 在对应提交节点设置的版本保持一致

如需自动化此流程，可使用[社区开发的 GitHub Action](https://github.com/huacnlee/zed-extension-action)。

> **注意：** 若您的扩展代码库使用其他许可证，在发布更新前需将其更改为[受认可的扩展许可证](#extension-license-requirements)之一。