# 系统要求

## Apple

### macOS

Zed 支持以下 macOS 版本：

| 版本          | 代号     | Apple 支持状态 | Zed 支持状态        |
| ------------- | -------- | -------------- | ------------------- |
| macOS 15.x    | Sequoia  | 官方支持       | 完全支持            |
| macOS 14.x    | Sonoma   | 官方支持       | 完全支持            |
| macOS 13.x    | Ventura  | 官方支持       | 完全支持            |
| macOS 12.x    | Monterey | 2024-09-16终止 | 完全支持            |
| macOS 11.x    | Big Sur  | 2023-09-26终止 | 部分支持            |
| macOS 10.15.x | Catalina | 2022-09-12终止 | 部分支持            |

标记为"部分支持"的 macOS 版本（Big Sur 和 Catalina）不支持通过 Zed Collaboration 进行屏幕共享。这些功能使用了 [LiveKit SDK](https://livekit.io)，该 SDK 依赖于 [ScreenCaptureKit.framework](https://developer.apple.com/documentation/screencapturekit/)，此框架仅在 macOS 12（Monterey）及更新版本中可用。

### Mac 硬件

Zed 支持采用英特尔（x86_64）或苹果（aarch64）处理器且满足上述 macOS 系统要求的设备：

- MacBook Pro（2015 年初及更新机型）
- MacBook Air（2015 年初及更新机型）
- MacBook（2016 年初及更新机型）
- Mac Mini（2014 年末及更新机型）
- Mac Pro（2013 年末及更新机型）
- iMac（2015 年末及更新机型）
- iMac Pro（所有机型）
- Mac Studio（所有机型）

## Linux 系统

Zed 支持 64 位英特尔/AMD（x86_64）和 64 位 ARM（aarch64）处理器。

Zed 需要 Vulkan 1.3 驱动程序及以下桌面门户组件：

- `org.freedesktop.portal.FileChooser`
- `org.freedesktop.portal.OpenURI`
- `org.freedesktop.portal.Secret` 或 `org.freedesktop.Secrets`

## Windows 系统

Zed 支持以下 Windows 版本：
| 版本 | 微软支持状态 | Zed 支持状态 |
| ------------------------- | ------------------ | ------------------- |
| Windows 11（所有版本） | 受支持 | 受支持 |
| Windows 10（64 位） | 受支持 | 受支持 |

### Windows 硬件要求

Zed 支持采用英特尔或 AMD 64位（x86_64）处理器且满足以下Windows系统要求的设备：

- Windows 11（64位）
- Windows 10（64位）
- 显卡：支持 DirectX 11 的 GPU（2012年后生产的大多数电脑均符合）
- 驱动：最新版 NVIDIA/AMD/Intel 驱动程序（不支持微软基础显示适配器）

## FreeBSD

暂未提供官方下载版本，可通过[源码编译](./development/freebsd.md)安装。

## Web

当前暂不支持，请参阅我们的[平台支持议题](https://github.com/zed-industries/zed/issues/5391)。