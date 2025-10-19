# 计费说明

我们采用 Stripe 作为支付服务提供商，并使用 Orb 处理发票与计量计费。所有专业版方案均需通过信用卡或其他支持的支付方式完成付款。
若需基于发票的结算方式，请选择企业版方案。欢迎发送邮件至 [sales@zed.dev](mailto:sales@zed.dev) 了解更多详情。

## 账户信息管理 {#settings}

您可访问 [zed.dev/account](https://zed.dev/account) 查看账户信息与设置。
当前页面主要集成自我们的发票/计量合作伙伴 Orb 提供的数据（我们正计划尽快推出更原生的使用体验！）

## 结算周期 {#billing-cycles}

Zed 采用月度结算模式，以您首次订阅日期为计费起点。在您订阅 Zed 专业版期间，每月将至少收到一张发票；若当月增量令牌消费金额超过 10 美元，则会收到多张发票。

## 阈值结算 {#threshold-billing}

Zed采用阈值计费模式，以确保及时收回欠款并防止滥用。每当您使用Zed托管模型的费用超过10美元阈值时，系统将生成新账单并重置阈值至0美元。

例如：
- 假设您在2月1日订阅，首张账单金额为10美元
- 若2月份累计使用12美元增量代币，其中前10美元产生于2月15日，则当日您将收到10美元账单
- 到3月1日，您将收到12美元账单：包含10美元（3月专业订阅费）及2美元（未达阈值的剩余代币费用）

## 支付失败处理 {#payment-failures}

若账单支付失败，Zed将暂停您使用托管模型的权限直至完成付款。如需协助请邮件联系[billing-support@zed.dev](mailto:billing-support@zed.dev)。

## 历史账单查询 {#invoice-history}

您可以通过访问 [zed.dev/account](https://zed.dev/account) 并点击嵌入式 Orb 门户中的 `Invoice history` 来查看历史发票记录。

如需获取 Stripe 平台的历史发票，请发送邮件至 [billing-support@zed.dev](mailto:billing-support@zed.dev)

## 更新账单信息 {#updating-billing-info}

如需更新支付方式、姓名、地址或税务信息，请发送邮件至 [billing-support@zed.dev](mailto:billing-support@zed.dev) 获取帮助。

> 我们即将升级账户页面，届时将支持自助更新功能，敬请期待！

请注意：账单信息的修改**仅会影响未来发票**——**我们无法对历史发票进行任何修改**。

## 销售税 {#sales-tax}

Zed 与 [Sphere](https://www.getsphere.com/) 合作，根据客户所在地及销售产品计算发票的间接税率。税费将作为独立项目列在发票上，优先依据您的账单地址计算，其次依据 Stripe 记录的信用卡发卡国家/地区。

若您拥有增值税/商品及服务税登记号，可在结账过程中填写。请勾选表明您为企业用户的选项。

请注意：对增值税/商品及服务税登记号及地址的修改**仅会影响后续发票**——**我们无法修改历史发票**。
如有疑问或问题，请发送邮件至 [billing-support@zed.dev](mailto:billing-support@zed.dev)。