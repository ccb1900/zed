# Tailwind CSS

Zed 内置支持 Tailwind CSS 自动补全、代码检查与悬停预览功能。

- 语言服务器：[tailwindlabs/tailwindcss-intellisense](https://github.com/tailwindlabs/tailwindcss-intellisense)

## 配置说明

如需配置 Tailwind CSS 语言服务器，请参考[扩展设置文档](https://github.com/tailwindlabs/tailwindcss-intellisense?tab=readme-ov-file#extension-settings)，并将配置添加至您的 `settings.json` 文件的 `lsp` 配置段：

```json [settings]
{
  "lsp": {
    "tailwindcss-language-server": {
      "settings": {
        "classFunctions": ["cva", "cx"],
        "experimental": {
          "classRegex": ["[cls|className]\\s\\:\\=\\s\"([^\"]*)"]
        }
      }
    }
  }
}
```

Zed 中可搭配使用 Tailwind CSS 的语言：

- [Astro](./astro.md)
- [CSS](./css.md)
- [ERB](./ruby.md)
- [HEEx](./elixir.md#heex)
- [HTML](./html.md)
- [TypeScript](./typescript.md)
- [JavaScript](./javascript.md)
- [PHP](./php.md)
- [Svelte](./svelte.md)
- [Vue](./vue.md)

### Prettier 插件

Zed 原生支持 Prettier，这意味着如果您已安装 [Tailwind CSS Prettier 插件](https://github.com/tailwindlabs/prettier-plugin-tailwindcss)，只需将其添加到 Prettier 配置中即可自动生效：

```json [settings]
// .prettierrc
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```