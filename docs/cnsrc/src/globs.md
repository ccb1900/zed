# 通配符

Zed 支持使用 [通配符](https://en.wikipedia.org/wiki/Glob_(programming)) 模式，这是类Unix shell风格路径匹配通配符（如 `*.rs` 或 `src/**/*.ts`）的正式名称，这些通配符被 sh、bash、zsh 等 shell 所支持。通配符与 [正则表达式](https://en.wikipedia.org/wiki/Regular_expression) 类似但有区别。在 Zed 中，这些通常用于匹配文件名。

## 通配符风格

Zed 使用两个不同的 Rust 库来匹配通配符模式：

- [ignore 库](https://docs.rs/ignore/latest/ignore/) 用于匹配存储在 `.gitignore` 文件中的通配符模式
- [glob 库](https://docs.rs/glob/latest/glob/) 用于在 Zed 中匹配文件路径

虽然简单表达式在不同环境中具有可移植性（例如在 gitignore 中运行 `ls *.py` 或 `*.tmp`），但高级功能（字符类、排除规则、`**` 等）的支持程度和语法在不同实现中存在显著差异。本文档后续内容将基于 Zed 通过 `glob` 包实现的功能来描述通配符规则。关于 `.gitignore`、Shell 及其他编程语言的通配模式语法文档，请参阅下文[参考资料](#references)章节。

`glob` 包完全基于 Rust 实现，不依赖平台 libc 提供的 `glob` / `fnmatch` 接口。这意味着 Zed 中的通配符规则在不同平台上应具有一致的表现。

## 引言

全局"模式"用于匹配文件名或完整文件路径。例如，在使用"搜索所有文件"功能时，您可以点击漏斗形状的"切换筛选器"按钮，这将显示额外的"包含"和"排除"搜索字段，支持通过全局模式指定文件路径和文件名的匹配规则。

创建全局模式时，您可以使用一个或多个特殊字符：

| 特殊字符 | 含义                                                           |
| ----------------- | ----------------------------------------------------------------- |
| ?               | 匹配任意单个字符                                      |
| *               | 匹配任意（可能为空）字符序列               |
| **              | 匹配当前目录及任意子目录        |
| [abc]           | 匹配括号内的任意一个字符                         |
| [a-z]           | 匹配指定范围内的任意字符（按Unicode顺序）         |
| [^abc]          | 对[abc]的否定（匹配不在括号内的字符） |

注：

1. 不支持类似 `{a,b,c}` 的 Shell 风格花括号扩展语法。
2. 方括号内若要匹配字面连字符 `-`，必须将其置于首位 `[-abc]` 或末位 `[abc-]`。
3. 匹配字面左方括号 `[` 时需使用 `[[]`，或将其置于字符组首位 `[[abc]`。
4. 匹配字面右方括号 `]` 时需使用 `[]]`，或将其置于字符组末位 `[abc]]`。

## 示例

### 匹配文件扩展名

若需仅搜索 Markdown 文件，请在“包含”搜索字段中添加 `*.md`。

### 不区分大小写匹配

Zed 中的通配符区分大小写，因此 `*.c` 不会匹配 `main.C`（即使在 macOS 的 HFS+/APFS 等不区分大小写的文件系统中）。建议使用方括号进行字符匹配：将 `*.c` 替换为 `*.[cC]`。

### 匹配目录

如果你想在 [zed 代码库](https://github.com/zed-industries/zed) 中搜索 [配置语言服务器](https://zed.dev/docs/configuring-languages#configuring-language-servers) 的示例（位于 Zed 的 settings.json 中的 `"lsp"` 下），你可以搜索 `"lsp"`，并在“包含”筛选器中指定 `docs/**/*.md`。这将仅匹配路径位于 `docs` 目录下或该文件夹的任何嵌套子目录 `**/` 中，且文件名以 `.md` 结尾的文件。

若您希望仅限定在[Zed语言特定文档](https://zed.dev/docs/languages)页面内操作，可以定义一个更精确的匹配模式：`docs/src/languages/*.md`。该模式将匹配[`docs/src/languages/rust.md`](https://github.com/zed-industries/zed/blob/main/docs/src/languages/rust.md)与[`docs/src/languages/cpp.md`](https://github.com/zed-industries/zed/blob/main/docs/src/languages/cpp.md)，但不会匹配[`docs/src/configuring-languages.md`](https://github.com/zed-industries/zed/blob/main/docs/src/configuring-languages.md)。

### 隐式通配符

在使用项目搜索的“包含”/“排除”过滤器时，每个通配符模式都会自动被隐式通配符包裹。例如，若要从搜索结果中排除路径或文件名包含"license"的文件，只需在排除框中输入`license`。系统后台会将`license`转换为`**license**`。这意味着名为`license.*`、`*.license`的文件或位于`license`子目录中的文件都将被过滤。这样用户就能轻松筛选`*.ts`文件，而无需每次都手动输入`**/*.ts`。

另一种情况是，如果您想在Zed设置中创建仅适用于特定目录的[`file_types`](./configuring-zed.md#file-types)覆盖规则，则必须显式包含通配符。例如，若您有一个包含`html`扩展名的模板文件目录，并希望将其识别为Jinja2模板，可使用以下配置：

```json [settings]
{
  "file_types": {
    "C++": ["[cC]"],
    "Jinja2": ["**/templates/*.html"]
  }
}
```

## 参考文档

虽然Zed中的通配符实现如上所述，但在其他语言中使用通配符编写代码时，请参考您所用平台的通配符文档：

- [macOS fnmatch](https://developer.apple.com/library/archive/documentation/System/Conceptual/ManPages_iPhoneOS/man3/fnmatch.3.html)（BSD C 标准库）
- [Linux fnmatch](https://www.gnu.org/software/libc/manual/html_node/Wildcard-Matching.html)（GNU C 标准库）
- [POSIX fnmatch](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fnmatch.html)（POSIX 规范）
- [node-glob](https://github.com/isaacs/node-glob)（Node.js `glob` 包）
- [Python glob](https://docs.python.org/3/library/glob.html)（Python 标准库）
- [Golang glob](https://pkg.go.dev/path/filepath#Match)（Go 标准库）
- [gitignore 模式](https://git-scm.com/docs/gitignore)（Gitignore 模式格式）
- [PowerShell：关于通配符](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_wildcards)（PowerShell 中的通配符）