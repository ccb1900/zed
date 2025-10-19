# 工作区持久化

Zed 会创建本地 SQLite 数据库来存储与工作区及项目相关的数据。这些数据库保存的内容包括：您在项目中打开的标签页与窗格、每个打开文件的滚动位置、所有已打开项目的列表（用于最近项目模态选择器）等。您可以在以下位置找到并查看这些数据库：

- macOS：`~/Library/Application Support/Zed`
- Linux 与 FreeBSD：`~/.local/share/zed`（或位于 `XDG_DATA_HOME` 或 `FLATPAK_XDG_DATA_HOME` 内）
- Windows：`%LOCALAPPDATA%\Zed`

这些数据库的命名规范遵循 `0-<zed_channel>` 格式：

- 稳定版：`0-stable`
- 预览版：`0-preview`

**如果在 Zed 中遇到工作区持久化问题，删除数据库并重启 Zed 通常可以解决问题，因为数据库可能在某个时刻已损坏。** 若重启 Zed 并重新生成数据库后问题仍然存在，请[提交问题报告](https://github.com/zed-industries/zed/issues/new?template=10_bug_report.yml)。

## 设置

您可以通过以下设置自定义工作区恢复行为：

```json [settings]
{
  // Workspace restoration behavior.
  //   All workspaces ("last_session"), last workspace ("last_workspace") or "none"
  "restore_on_startup": "last_session",
  // Whether to attempt to restore previous file's state when opening it again.
  // E.g. for editors, selections, folds and scroll positions are restored
  "restore_on_file_reopen": true,
  // Whether to automatically close files that have been deleted on disk.
  "close_on_file_delete": false
}
```