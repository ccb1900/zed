# 账户

登录 Zed 并非强制要求。即使不登录，您也能使用代码编辑器应具备的大部分功能。以下我们将说明需要登录的少数功能及登录方式。

## 哪些功能需要登录？

1. 所有实时[协作功能](./collaboration.md)
2. [基于大语言模型的智能功能](./ai/overview.md)（若使用 Zed 作为大语言模型服务提供商）。您也可选择[自行配置 API 密钥](./ai/llm-providers.md#use-your-own-keys)来免登录使用这些功能

## 登录方式

Zed 采用 GitHub OAuth 流程进行用户认证，仅需获取 `read:user` 权限范围，该权限仅允许读取您的 GitHub 基础资料信息。

1. 打开 Zed，点击窗口右上角的 `Sign In` 按钮，或通过命令面板运行 `client: sign in` 命令（macOS 系统使用 `cmd-shift-p` 快捷键，Windows/Linux 系统使用 `ctrl-shift-p` 快捷键）。
2. 默认浏览器将自动打开 Zed 登录页面。
3. 根据提示使用 GitHub 账户完成身份验证。
4. 验证成功后，浏览器将显示确认信息，您将自动登录 Zed。

**注意**：若处于企业防火墙后，请确保允许访问 `zed.dev` 和 `collab.zed.dev`。

## 退出登录

可通过以下任一方式退出 Zed：

- 点击右上角个人资料图标，在下拉菜单中选择 `Sign Out`。
- 打开命令面板并运行 `client: sign out` 命令。

## 电子邮箱地址 {#email}

您的 Zed 账户邮箱地址由 GitHub OAuth 提供。若您设置了公开邮箱地址，系统将优先使用该地址，否则将采用您 GitHub 的主邮箱。通过[登录 zed.dev](https://zed.dev/sign_in) 可将 GitHub 的邮箱变更同步至 Zed 账户。

订阅服务由 Stripe 处理计费，开通时将自动使用您 Zed 账户的邮箱地址。请注意，修改 Zed 账户邮箱不会自动更新 Stripe 中的邮箱信息。具体变更方式请参阅[更新账单信息](./ai/billing.md#updating-billing-info)。

## 隐藏界面登录按钮

若无需使用登录功能，可通过 `show_sign_in` 设置属性隐藏界面中的登录按钮。
更多细节请参阅[视觉定制页面](./visual-customization.md)。