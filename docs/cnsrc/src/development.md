# Zed 开发指南

请参阅以下平台特定的源码构建说明：

- [macOS](./development/macos.md)
- [Linux](./development/linux.md)
- [Windows](./development/windows.md)

如需开发协作功能，请额外参阅：

- [本地协作开发](./development/local-collaboration.md)

## 密钥链访问说明

Zed 将密钥信息存储于系统密钥链中。

但在 macOS 平台（及其他平台可能也存在）运行开发版本时，访问密钥链会触发频繁的密码输入弹窗。

在 macOS 上，此问题源于开发版本缺乏稳定的身份标识。即便选择"始终允许"选项，当可执行文件发生任何变更时，系统仍会再次要求输入密码。

这种情况会快速降低开发效率，影响开发体验。

因此，在默认情况下，运行 Zed 的开发版本时会使用替代凭据提供程序，以绕过系统密钥链。

> 注意：这**仅**适用于开发版本。对于所有非开发发布渠道，始终使用系统密钥链。

如果您需要在开发版本中使用真实的系统密钥链测试某些功能，请设置以下环境变量并运行 Zed：

```
ZED_DEVELOPMENT_USE_KEYCHAIN=1
```

## 性能测量

Zed 内置了帧时间测量系统，可用于分析渲染每一帧所需的时间。这在比较不同版本之间的渲染性能或优化帧渲染代码时尤其有用。

### 使用 ZED_MEASUREMENTS

要启用性能测量，请设置 `ZED_MEASUREMENTS` 环境变量：

```sh
export ZED_MEASUREMENTS=1
```

启用后，Zed 会将帧渲染计时信息输出到标准错误流，显示每帧渲染所需时长。

### 性能对比工作流程

以下是不同版本间帧渲染性能对比的典型工作流程：

1. **启用测量功能：**

   ```sh
   export ZED_MEASUREMENTS=1
   ```

2. **测试第一个版本：**

   - 切换到需要测量的提交版本
   - 在发布模式下运行 Zed 并使用 5-10 秒：`cargo run --release &> version-a`

3. **测试第二个版本：**

   - 切换到需要对比的另一个提交版本
   - 在发布模式下运行 Zed 并使用 5-10 秒：`cargo run --release &> version-b`

4. **生成对比报告：**

   ```sh
   script/histogram version-a version-b
   ```

`script/histogram` 工具可接受任意数量的测量文件，并生成直方图可视化报告，对比不同版本的帧渲染性能数据。

### 使用 `util_macros::perf`

要对单元测试进行基准测试，请使用 `util_macros` 包中的 `#[perf]` 属性进行标注。然后运行 `cargo
perf-test -p $CRATE` 来执行基准测试。关于详细示例和说明，请参阅 `crates/util_macros` 和 `tooling/perf` 的 rustdoc 文档。

## 贡献者链接

- [贡献指南](https://github.com/zed-industries/zed/blob/main/CONTRIBUTING.md)
- [版本发布](./development/releases.md)
- [调试崩溃问题](./development/debugging-crashes.md)
- [行为准则](https://zed.dev/code-of-conduct)
- [Zed 贡献者许可协议](https://zed.dev/cla)