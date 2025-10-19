# 版本发布说明

每当您开启一个拉取请求时，正文内容将根据此[拉取请求模板](https://github.com/zed-industries/zed/blob/main/.github/pull_request_template.md)自动填充。

```md
...

Release Notes:

- N/A _or_ Added/Fixed/Improved ...
```

每周三，我们会运行一个[`get-preview-channel-changes`](https://github.com/zed-industries/zed/blob/main/script/get-preview-channel-changes)脚本，该脚本会从进入预览版的拉取请求中提取`Release Notes`行内容，具体流程记录在我们的[版本发布](https://zed.dev/docs/development/releases)文档中。

该脚本会输出`Release Notes`行以下的所有内容，包括附加信息如拉取请求作者（若非Zed团队成员）及拉取请求链接。
若您使用`N/A`，脚本将完全跳过您的拉取请求。

## 编写`Release Notes`行的指导原则

- 只有当用户能够看到或感受到 Zed 中的差异时，才应编写 `Release Notes` 行。
- `Release Notes` 行的描述应让 Zed 用户理解变更内容。
  避免使用技术性编辑器开发术语，用普通文本编辑器用户能理解的语言描述变更。
- 若需为团队成员提供技术细节，请将其写在 `Release Notes` 行上方。
- 文档更新需标注为 `N/A`。
- 若拉取请求新增/修改了设置项或快捷键，必须明确提及该设置或快捷键。
  不要让用户翻阅文档或拉取请求来查找这些信息（尽管文档中也应包含）。
- 对于还原性拉取请求：
  - 若被还原内容**已发布**，需包含 `Release Notes` 行说明还原原因（这属于重大变更）。
  - 若被还原内容**未发布**，请将原拉取请求的 `Release Notes` 行修改为 `N/A`，否则该内容仍会被收录，可能导致发布说明编译时误将未实装功能标记为已发布。