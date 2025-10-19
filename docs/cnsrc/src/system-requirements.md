# 系统要求

## 苹果系统

### macOS

Zed 支持以下 macOS 版本：

| 版本          | 代号     | 苹果支持状态   | Zed 支持状态        |
| ------------- | -------- | -------------- | ------------------- |
| macOS 15.x    | Sequoia  | 官方支持       | 完全支持            |
| macOS 14.x    | Sonoma   | 官方支持       | 完全支持            |
| macOS 13.x    | Ventura  | 官方支持       | 完全支持            |
| macOS 12.x    | Monterey | 2024-09-16终止 | 完全支持            |
| macOS 11.x    | Big Sur  | 2023-09-26终止 | 部分支持            |
| macOS 10.15.x | Catalina | 2022-09-12终止 | 部分支持            |

标注为"部分支持"的 macOS 版本（Big Sur 和 Catalina）不支持通过 Zed 协作功能进行屏幕共享。该功能基于 [LiveKit SDK](https://livekit.io) 实现，需要依赖仅适用于 macOS 12（Monterey）及更新版本的 [ScreenCaptureKit.framework](https://developer.apple.com/documentation/screencapturekit/) 框架。

### Mac 硬件要求

Zed支持采用英特尔（x86_64）或苹果（aarch64）处理器且满足上述macOS要求的设备：

- MacBook Pro（2015年初及更新机型）
- MacBook Air（2015年初及更新机型）
- MacBook（2016年初及更新机型）
- Mac Mini（2014年末及更新机型）
- Mac Pro（2013年末及更新机型）
- iMac（2015年末及更新机型）
- iMac Pro（所有机型）
- Mac Studio（所有机型）

## Linux系统

Zed支持64位英特尔/AMD（x86_64）和64位ARM（aarch64）处理器。

Zed需要Vulkan 1.3驱动程序及以下桌面门户支持：
- [[代码块_0]]
- [[代码块_1]]
- [[代码块_2]]或[[代码块_3]]

## Windows系统

Zed支持以下Windows版本：
| 版本                      | 微软支持状态       | Zed支持状态        |
| ------------------------- | ------------------ | ------------------- |
| Windows 11（所有版本）    | 官方支持           | 支持                |
| Windows 10（64位）        | 官方支持           | 支持                |

### Windows硬件要求

Zed 支持符合上述 Windows 要求的英特尔或 AMD 64 位（x86_64）处理器设备：

- Windows 11（64位）
- Windows 10（64位）
- 显卡：支持 DirectX 11 的 GPU（2012 年后生产的大多数 PC 均可满足）
- 驱动：最新版 NVIDIA/AMD/Intel 驱动程序（不支持微软基础显示适配器）

## FreeBSD

暂未提供官方下载版本，可通过[源码编译](./development/freebsd.md)安装。

## Web

当前暂不支持，请参阅我们的[平台支持议题](https://github.com/zed-industries/zed/issues/5391)。