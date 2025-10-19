# Zed 版本发布

请阅读 Zed 的[发布渠道说明](https://zed.dev/faq#what-are-the-release-channels)。

## 周三发布流程

执行此操作需要具备 Zed 代码库的写入权限。

流程中使用的各项服务凭证可在 1Password 中获取。

请通过 `releases` Slack 频道通知团队即将开始版本发布。
对于周三的常规小版本更新而言这主要是形式流程，但在执行补丁发布时尤其重要，因为其他开发人员可能已提交希望遴选修复的代码。

---

1. 检出 `main` 并确保工作区干净无修改

1. 运行 `git fetch && git pull` 确保本地已获取最新提交

1. 运行 `git fetch --tags --force` 强制同步本地标签与远程仓库

1. 运行 `./script/get-stable-channel-release-notes`
   - 根据脚本最终提示整合发布说明

1. 运行 `./script/bump-zed-minor-versions`

- 按照指示推送标签和分支。

1. 运行 `./script/get-preview-channel-changes`。

   - 获取脚本输出，将每个发布说明条目按类别整理成发布说明。
   - 使用之前的发布内容作为初始框架。
   - 确保将 `Credit` 行（如果存在）附加到发布说明条目的末尾。

1. 当发布草稿出现在 [GitHub Releases](https://github.com/zed-industries/zed/releases) 上后，将预览版和稳定版的发布说明分别粘贴到对应位置并**保存**。

   - **不要发布草稿！**

1. 检查发布资源。

   - 确保稳定版和预览版的发布任务已成功完成且无错误。
   - 确保每个草稿包含正确数量的资源——目前每个发布应包含 11 个资源。
   - 下载每个发布草稿的构建产物，并在本地测试是否可以正常运行。

1. 发布草稿。

- 逐一发布稳定版和预览版草稿。
  - 使用 [Vercel](https://vercel.com/zed-industries/zed-dev) 检查网站重建进度。
    重建完成后，版本将对外发布。

1. 将稳定版发布说明发布至社交媒体。
  - Bluesky 和 X 平台的推文已预先在 [Buffer](https://buffer.com) 中生成草稿。
  - 逐条发布，确保两条内容均同步至对应平台。

1. 发送稳定版发布说明邮件。
  - 邮件内容已预先在 [Kit](https://kit.com) 中生成草稿。

1. 根据预览版中的热门内容创建社交媒体推文。

- 在 [推文](https://zed.dev/channel/tweets-23331) 频道中撰写文案草稿。
- 创建预览媒体（视频、截图）。
  - 对于需要录制视频的功能，尝试制作仅包含图片的版本，用于邮件中，因为视频和 GIF 在邮件中效果不佳。
  - 将所有创建的媒体存储到我们 Google Drive 的 `Feature Media` 中。
- 在 [Buffer](https://buffer.com) 中构建 X 和 Bluesky 的帖子草稿（文案和媒体），以便在下周的稳定版发布时发送。

**注意：这些是预览内容，你可能会发现一些错误。**
**这是向团队报告这些发现的绝佳时机！**

1. 根据预览中的热门内容构建邮件。
   - 可以重复使用预览社交媒体帖子中的文案和图片媒体。
   - 在 [Kit](https://kit.com) 中创建邮件草稿，以便在下周的稳定版发布时发送。

## 补丁发布流程

如果你的 PR 修复了程序崩溃或闪退问题，请将其精选到当前的稳定版和预览版分支。
如果你的 PR 修复了近期发布代码中的回归问题，请将其精选到预览版分支。

执行此操作需要拥有 Zed 代码库的写入权限：

---

1. 照常提交包含你修改的 PR 至 `main`。

1. 合并后，在本地将提交精选到任一发布分支（`v0.XXX.x`）。

   - 某些情况下可能需要处理合并冲突。
     这种情况多发生于向稳定版分支精选时，因为稳定版分支相比预览版分支更“陈旧”。

1. 完成提交精选后，运行 `./script/trigger-release {preview|stable}`。
   该命令将更新版本号、创建新发布标签并触发发布构建。

- 也可通过 [GitHub Actions 界面](https://github.com/zed-industries/zed/actions/workflows/bump_patch_version.yml)执行此操作：
  ![](https://github.com/zed-industries/zed/assets/1486634/9e31ae95-09e1-4c7f-9591-944f4f5b63ea)

1. 当 [GitHub Releases](https://github.com/zed-industries/zed/releases) 页面出现发布草稿后，请校对并编辑版本说明，完成后**保存**。

   - **暂不发布草稿。**

1. 检查发布资源。

   - 确保稳定版/预览版的发布任务已无错误完成。
   - 确认每个草稿包含正确数量的资源——当前每个版本应包含10项资源。
   - 下载每个发布草稿的构件，并在本地测试能否正常运行。

1. 逐项发布稳定版/预览版草稿。
   - 通过 [Vercel](https://vercel.com/zed-industries/zed-dev) 监测网站重建进度。
     重建完成后，版本将对外发布。

## 夜间发布流程

除了公开发布版本外，我们还设有每日构建版本，鼓励员工使用。
每日版本由定时任务每日自动发布，且可根据需要随时推送。
该版本不提供发布说明或公告，您只需将更改合并到主分支并运行`./script/trigger-release nightly`即可。