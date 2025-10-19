---
description: Zed 是一款支持多种 Git 功能的文本编辑器
title: Zed 编辑器 Git 集成文档
---

# Git

Zed 目前提供了一套基础的 Git 功能，未来将支持更多高级功能，例如冲突解决工具、逐行暂存等。

以下是当前支持的所有功能概览：

- 提交
- 暂存、推送、拉取和获取
- 项目差异：所有更改的多缓冲区视图
- 缓冲区和编辑器滚动条中的差异指示器
- 编辑器中未暂存更改的内联差异切换和还原
- 项目面板中的 Git 状态显示
- 分支创建与切换
- Git 追溯查看

## Git 面板

Git 面板可让您全面了解工作树和 Git 暂存区的状态。

您可以使用 {#action git_panel::ToggleFocus} 或点击状态栏中的 Git 图标来打开 Git 面板。

在面板中，您可以一目了然地查看项目状态——当前激活的代码库与分支、已修改的文件列表，以及每个文件的暂存状态。

Zed会持续监控您的代码库，确保您在命令行中做出的更改能实时同步显示。

## 项目差异对比

通过命令面板的 {#action git::Diff} 操作或Git面板，打开项目差异视图({#kb git::Diff})即可查看Git捕获的所有变更。

项目差异视图中显示的所有变更行为与多缓冲区完全一致：它们都是文件的可编辑片段。

您可以通过标签栏的按钮或对应快捷键，对每个代码块或整个文件进行暂存/取消暂存操作。

<!-- Add media -->

## 获取、推送与拉取

通过Zed中的Git面板按钮或命令面板中的相应操作，从您的Git仓库进行拉取、推送或获取：{#action git::Fetch}、{#action git::Push}和{#action git::Pull}。

## 暂存工作流程

Zed提供两种主要的暂存工作流程，可以使用项目差异视图或直接通过面板操作。

### 使用项目差异视图

在项目差异视图中，您可以专注于每个代码块，并通过点击标签栏按钮或使用快捷键{#action git::StageAndNext}（{#kb git::StageAndNext}）逐个暂存它们。

同样地，使用{#action git::StageAll}（{#kb git::StageAll}）快捷键可以一次性暂存所有代码块，然后立即通过{#action git::Commit}（{#kb git::Commit}）提交。

### 使用Git面板

在面板中，您只需输入提交消息并点击提交按钮或使用{#action git::Commit}。这将自动暂存所有已跟踪的文件（在条目的复选框中以`[·]`表示）并提交它们。

<!-- Show a set of changes with default staged -->

可以通过每个条目对应的复选框来暂存修改。使用面板顶部的按钮或{#action git::StageAll}可一次性暂存所有变更。

<!-- Add media -->

## 提交操作

Zed 提供两种提交文本框：

1. 第一种位于 Git 面板底部。按下 {#kb git::Commit} 将立即提交所有已暂存的更改。
2. 第二种可通过操作 {#action git::ExpandCommitEditor} 触发，或在 Git 面板提交文本框处于焦点状态时按 {#kb git::ExpandCommitEditor} 启动。

### 撤销提交

在 Zed 中完成提交后，Git 面板的提交文本框下方会立即显示一个状态栏，其中包含最近提交的记录。
您可以使用其中的"取消提交"按钮执行 `git reset HEADˆ--soft` 命令。

## Git 中的 AI 支持

Zed 目前支持基于大语言模型的提交信息生成功能。
当您在 Git 面板中聚焦于提交信息编辑器时，可通过点击左下角的铅笔图标，或使用快捷键 {#action git::GenerateCommitMessage} ({#kb git::GenerateCommitMessage}) 来调用 AI 生成提交信息。

> 请注意：您需要配置 LLM 服务商以完成计费设置，可通过使用自己的 API 密钥或试用/购买 Zed 托管的 AI 模型实现。请访问 [AI 配置页面](./ai/configuration.md) 了解具体操作。

您可以通过配置 `commit_message_model` 智能体设置来指定首选模型。更多信息请参阅 [功能特定模型](./ai/agent-settings.md#feature-specific-models)。

```json [settings]
{
  "agent": {
    "version": "2",
    "commit_message_model": {
      "provider": "anthropic",
      "model": "claude-3-5-haiku"
    }
  }
}
```

<!-- Add media -->

未来可能会实现更高级的人工智能与 Git 功能的集成。

## Git 集成功能

Zed 与主流 Git 托管服务平台深度集成，可将 Git 提交哈希值及问题追踪、拉取请求与合并请求的引用编号自动转换为可点击的快捷链接。

当前支持的托管平台包括：
[GitHub](https://github.com)
[GitLab](https://gitlab.com)
[Bitbucket](https://bitbucket.org)
[SourceHut](https://sr.ht)
[Codeberg](https://codeberg.org)

Zed还提供"复制永久链接"功能，可生成指向Git托管服务中代码片段的永久链接。
这类链接适用于分享特定提交中某文件的指定行或行范围。
您可以通过以下方式触发此操作：在[命令面板](./getting-started.md#command-palette)中搜索`permalink`；
创建[custom key bindings](key-bindings.md#custom-key-bindings)绑定至`editor::CopyPermalinkToLine`或`editor::OpenPermalinkToLine`操作；
或直接在编辑器中选中行后右键点击选择`Copy Permalink`。

## 差异区块快捷键

当查看含变更的文件时，Zed会显示可展开/折叠的差异区块以便详细审阅：

- **展开所有差异块**：{#action editor::ExpandAllDiffHunks}（快捷键 {#kb editor::ExpandAllDiffHunks}）
- **折叠所有差异块**：按下 `Escape`（绑定为 {#action editor::Cancel}）
- **切换选中差异块**：{#action editor::ToggleSelectedDiffHunks}（快捷键 {#kb editor::ToggleSelectedDiffHunks}）
- **在差异块间导航**：{#action editor::GoToHunk} 与 {#action editor::GoToPreviousHunk}

> **提示：** `Escape` 键是折叠所有已展开差异块并返回更改概览的最快捷方式。

## 操作参考

| 操作                                      | 快捷键绑定                            |
| ----------------------------------------- | ------------------------------------- |
| {#action git::Add}                        | {#kb git::Add}                        |
| {#action git::StageAll}                   | {#kb git::StageAll}                   |
| {#action git::UnstageAll}                 | {#kb git::UnstageAll}                 |
| {#action git::ToggleStaged}               | {#kb git::ToggleStaged}               |
| {#action git::StageAndNext}               | {#kb git::StageAndNext}               |
| {#action git::UnstageAndNext}             | {#kb git::UnstageAndNext}             |
| {#action git::Commit}                     | {#kb git::Commit}                     |
| {#action git::ExpandCommitEditor}         | {#kb git::ExpandCommitEditor}         |
| {#action git::Push}                       | {#kb git::Push}                       |
| {#action git::ForcePush}                  | {#kb git::ForcePush}                  |
| {#action git::Pull}                       | {#kb git::Pull}                       |
| {#action git::Fetch}                      | {#kb git::Fetch}                      |
| {#action git::Diff}                       | {#kb git::Diff}                       |
| {#action git::Restore}                    | {#kb git::Restore}                    |
| {#action git::RestoreFile}                | {#kb git::RestoreFile}                |
| {#action git::Branch}                     | {#kb git::Branch}                     |
| {#action git::Switch}                     | {#kb git::Switch}                     |
| {#action git::CheckoutBranch}             | {#kb git::CheckoutBranch}             |
| {#action git::Blame}                      | {#kb git::Blame}                      |
| {#action editor::ToggleGitBlameInline}    | {#kb editor::ToggleGitBlameInline}    |
| {#action editor::ExpandAllDiffHunks}      | {#kb editor::ExpandAllDiffHunks}      |
| {#action editor::ToggleSelectedDiffHunks} | {#kb editor::ToggleSelectedDiffHunks} |

> 并非所有操作都预设了快捷键，但可通过[自定义键位映射](./key-bindings.md#user-keymaps)进行绑定。

## Git 命令行配置

若希望在命令行执行提交时，同时将 Zed 设置为[Git 提交信息编辑器](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration#_core_editor)，可使用以下命令：

```shell
git config --global core.editor "zed --wait"
```

或将以下配置添加到您的 shell 环境文件中（如 `~/.zshrc`、`~/.bashrc` 等）：

```shell
export GIT_EDITOR="zed --wait"
```
