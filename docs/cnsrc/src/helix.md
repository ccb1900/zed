# Helix 模式

_开发中！尚未实现所有 Helix 快捷键。_

Zed 的 Helix 模式是一个模拟层，为 Zed 带来 Helix 风格的快捷键和模态编辑体验。它基于 Zed 的 [Vim 模式](./vim.md)构建，因此核心功能大多相通。启用 `helix_mode` 时也会自动启用 `vim_mode`。

关于 Vim 相关功能（在 Helix 模式中同样可用）的指南，请参阅我们的 [Vim 模式文档](./vim.md)。

若要查看 Helix 模式的当前开发进度，或申请补充缺失的 Helix 功能，请访问 ["我们达到 Helix 水平了吗？" 讨论帖](https://github.com/zed-industries/zed/discussions/33580)。

如需查看 Helix 默认快捷键的详细列表，请访问 [官方 Helix 文档](https://docs.helix-editor.com/keymap.html)。

## 核心差异

所有适用于 `m i` 或 `m a` 的文本对象同样适用于 `]` 和 `[`。例如，`] (` 会选中光标后的下一对圆括号。