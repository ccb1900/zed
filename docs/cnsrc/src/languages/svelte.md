# Svelte

Svelte 支持可通过 [Svelte 扩展](https://github.com/zed-extensions/svelte)获得。

- Tree-sitter：[tree-sitter-grammars/tree-sitter-svelte](https://github.com/tree-sitter-grammars/tree-sitter-svelte)
- 语言服务器：[sveltejs/language-tools](https://github.com/sveltejs/language-tools)

## 额外主题样式配置

你可以修改某些样式在属性中的显示方式，例如指令和修饰符：

```json [settings]
"syntax": {
  // Styling for directives (e.g., `class:foo` or `on:click`) (the `on` or `class` part of the attribute).
  "attribute.function": {
    "color": "#ff0000"
  },
  // Styling for modifiers at the end of attributes, e.g. `on:<click|preventDefault|stopPropagation>`
  "attribute.special": {
    "color": "#00ff00"
  }
}
```

## 内联提示

当在 Zed 中启用内联提示时，为了让语言服务器返回这些提示，Zed 设置了以下初始化选项：

要覆盖这些设置，请使用以下内容：

```json [settings]
"lsp": {
  "svelte-language-server": {
    "initialization_options": {
      "configuration": {
        "typescript": {
          // ......
        },
        "javascript": {
          // ......
        }
      }
    }
  }
}
```

更多信息请参阅 [TypeScript 语言服务器 `package.json`](https://github.com/microsoft/vscode/blob/main/extensions/typescript-language-features/package.json)。