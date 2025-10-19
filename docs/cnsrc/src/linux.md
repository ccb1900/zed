# Zed 在 Linux 系统上的安装

## 标准安装方法

对于大多数用户，我们推荐使用[下载页面](https://zed.dev/download)提供的脚本安装 Zed：

[[代码块_0]]

我们还提供 Zed 的预览版本，该版本会比稳定版提前约一周获得更新。您可以通过以下命令安装：

[[代码块_1]]

通过脚本安装的 Zed 在以下系统中运行效果最佳：

- 配备兼容 Vulkan 的 GPU（例如搭载 M 系列芯片的 MacBook 上的 Linux 系统）
- 具备系统级 glibc 环境（NixOS 和 Alpine 系统默认不满足）
  - x86_64 架构（Intel/AMD）：glibc 版本 ≥ 2.31（对应 Ubuntu 20 及以上版本）
  - aarch64 架构（ARM）：glibc 版本 ≥ 2.35（对应 Ubuntu 22 及以上版本）

Nix和Alpine系统均有第三方Zed软件包可供使用（不过目前版本会滞后几周）。若希望使用我们官方构建的版本，需安装glibc兼容层方可运行。在NixOS系统上可尝试[nix-ld](https://github.com/Mic92/nix-ld)，Alpine系统则需使用[gcompat](https://wiki.alpinelinux.org/wiki/Running_glibc_programs)。

以下情况需要从源码编译安装：

- 非64位Intel或64位ARM架构的设备（例如32位或RISC-V架构机器）
- Redhat Enterprise Linux 8.x、Rocky Linux 8、AlmaLinux 8、Amazon Linux 2系统（所有架构）
- Redhat Enterprise Linux 9.x、Rocky Linux 9.3、AlmaLinux 8、Amazon Linux 2023系统（仅限aarch64架构，x86_x64架构不受影响）

## 其他Linux系统安装方式

Zed是开源项目，支持[从源码编译安装](./development/linux.md)。

### 通过包管理器安装

针对不同的 Linux 发行版和软件包管理器，存在多个第三方 Zed 软件包，有时这些包会以 `zed-editor` 的名称提供。您或许可以通过以下软件包安装 Zed：

- Flathub：[`dev.zed.Zed`](https://flathub.org/apps/dev.zed.Zed)
- Arch：[`zed`](https://archlinux.org/packages/extra/x86_64/zed/)
- Arch (AUR)：[`zed-git`](https://aur.archlinux.org/packages/zed-git)、[`zed-preview`](https://aur.archlinux.org/packages/zed-preview)、[`zed-preview-bin`](https://aur.archlinux.org/packages/zed-preview-bin)
- Alpine：`zed` ([aarch64](https://pkgs.alpinelinux.org/package/edge/testing/aarch64/zed)) ([x86_64](https://pkgs.alpinelinux.org/package/edge/testing/x86_64/zed))
- Nix：`zed-editor` ([unstable](https://search.nixos.org/packages?channel=unstable&show=zed-editor))
- Fedora/Ultramarine (Terra)：[`zed`](https://github.com/terrapkg/packages/tree/frawhide/anda/devs/zed/stable)、[`zed-preview`](https://github.com/terrapkg/packages/tree/frawhide/anda/devs/zed/preview)、[`zed-nightly`](https://github.com/terrapkg/packages/tree/frawhide/anda/devs/zed/nightly)
- Solus：[`zed`](https://github.com/getsolus/packages/tree/main/packages/z/zed)
- Parabola：[`zed`](https://www.parabola.nu/packages/extra/x86_64/zed/)
- Manjaro：[`zed`](https://packages.manjaro.org/?query=zed)
- ALT Linux (Sisyphus)：[`zed`](https://packages.altlinux.org/en/sisyphus/srpms/zed/)
- AOSC OS：[`zed`](https://packages.aosc.io/packages/zed)

请参阅 [Repology](https://repology.org/project/zed-editor/versions) 查看各软件源中的 Zed 软件包列表。

安装第三方软件包时请注意，其版本可能并非最新，且与官方打包的 Zed 可能存在细微差异（常见改动是将二进制文件重命名为 `zed-editor` 或 `zed-preview` 以避免与其他软件包冲突）。

我们期待您帮助更多人用上 Zed。若您使用的包管理器尚未收录 Zed，且愿意协助解决，请参阅 [打包指南](./development/linux.md#notes-for-packaging-zed) 了解相关说明。

### 手动下载安装

您也可以通过下载我们预构建的 .tar.gz 压缩包来安装 Zed。该文件与自动安装脚本使用的完全相同，但您可通过修改以下步骤自定义安装路径：

下载 `zed-0.0.1-linux-x86_64.tar.gz` 文件：

- [zed-linux-x86_64.tar.gz](https://zed.dev/api/releases/stable/latest/zed-linux-x86_64.tar.gz) ([预览版](https://zed.dev/api/releases/preview/latest/zed-linux-x86_64.tar.gz))
- [zed-linux-aarch64.tar.gz](https://zed.dev/api/releases/stable/latest/zed-linux-aarch64.tar.gz)
  ([预览版](https://zed.dev/api/releases/preview/latest/zed-linux-aarch64.tar.gz))

然后确保压缩包中的 `zed` 二进制文件位于您的系统路径中。最简单的方法是解压压缩包并创建符号链接：

```sh
mkdir -p ~/.local
# extract zed to ~/.local/zed.app/
tar -xvf <path/to/download>.tar.gz -C ~/.local
# link the zed binary to ~/.local/bin (or another directory in your $PATH)
ln -sf ~/.local/zed.app/bin/zed ~/.local/bin/zed
```

如需与兼容 XDG 的桌面环境集成，您还需安装 `.desktop` 文件：

## 卸载 Zed

### 标准卸载方法

如果 Zed 是通过默认安装脚本安装的，可以通过在 `zed` shell 命令中添加 `--uninstall` 标志来卸载

```sh
zed --uninstall
```

如果没有错误，shell 将提示您是否保留或删除您的偏好设置。做出选择后，您应该会看到 Zed 已成功卸载的消息。

如果在您的 PATH 中找不到 `zed` shell 命令，您可以尝试以下命令之一

```sh
$HOME/.local/bin/zed --uninstall
```

或

```sh
$HOME/.local/zed.app/bin.zed --uninstall
```

第一种情况可能会失败，如果`$HOME/.local/bin/zed`和`$HOME/.local/zed.app/bin.zed`之间的符号链接未正确建立。但第二种情况应该有效，只要Zed安装在其默认位置。

如果Zed安装在其他位置，您必须调用该安装目录中的`zed`二进制文件，并以与之前命令相同的格式传递`--uninstall`标志。

### 包管理器

如果Zed是通过包管理器安装的，请查阅该包管理器的文档以了解如何卸载软件包。

## 故障排除

Linux可在多种不同配置的系统上运行。我们主要在标准的Ubuntu设置上测试Zed，因为这是我们用户最常用的发行版，但我们也期望它能在各种不同的机器上正常工作。

### Zed无法启动

若遇到类似 "/lib64/libc.so.6: version 'GLIBC_2.29' not found" 的错误，说明您系统当前的 glibc 版本过旧。您可以选择升级系统，或[通过源码安装 Zed](./development/linux.md)。

### 图形显示问题

#### Zed 无法启动窗口

Zed 需要 GPU 才能高效运行。我们底层使用 [Vulkan](https://www.vulkan.org/) 与您的 GPU 进行通信。如果遇到性能问题或 Zed 加载失败，Vulkan 可能是问题根源。

若看到提示 `Zed failed to open a window: NoSupportedDeviceFound`，说明 Vulkan 无法找到兼容的 GPU。您可以尝试运行 [vkcube](https://github.com/krh/vkcube)（该工具通常包含在各发行版的 `vulkaninfo` 或 `vulkan-tools` 软件包中）来诊断问题来源，具体操作如下：

```
vkcube
```

> **_注意_**：尝试通过运行 `vkcube -m [x11|wayland]` 在 X11 和 Wayland 模式下分别测试。某些版本的 `vkcube` 使用 `vkcube` 在 X11 下运行，使用 `vkcube-wayland` 在 Wayland 下运行。

这将输出一行描述当前图形设置的文字，并显示一个旋转的立方体。如果无法正常运行，您可能需要安装兼容 Vulkan 的 GPU 驱动程序，但某些情况下可能尚未支持 Vulkan。

您可以通过查看 Zed 日志（`~/.local/share/zed/logs/Zed.log`）中的 `Using GPU: ...` 来确认 Zed 正在使用哪个显卡。

如果出现类似 `ERROR_INITIALIZATION_FAILED`、`GPU Crashed` 或 `ERROR_SURFACE_LOST_KHR` 的错误，您可以尝试为 GPU 安装不同的驱动程序，或选择其他 GPU 来运行。（参见 [#14225](https://github.com/zed-industries/zed/issues/14225)）

在某些系统上，可以通过 `/etc/prime-discrete` 文件配合 [PRIME](https://wiki.archlinux.org/title/PRIME) 功能强制使用独立显卡。根据具体配置，您可能需要将该文件内容改为 "on"（强制使用独立显卡）或 "off"（强制使用集成显卡）。

在其他系统上，您可以在运行 Zed 时设置 `DRI_PRIME=1` 环境变量来强制启用独立显卡。

若您使用 AMD 显卡且在选中长文本行时 Zed 发生崩溃，请尝试设置 `ZED_PATH_SAMPLE_COUNT=0` 环境变量。（详见 [#26143](https://github.com/zed-industries/zed/issues/26143)）

若您使用 AMD 显卡并遇到“管道破裂”错误，请尝试改用 RADV 或 Mesa 驱动程序。（详见 [#13880](https://github.com/zed-industries/zed/issues/13880)）

如果您正在使用 `amdvlk`（默认的开源 AMD 显卡驱动），可能会发现 Zed 始终无法启动。这是部分用户遇到的已知问题，例如在 Omarchy 系统上（详见 [#28851](https://github.com/zed-industries/zed/issues/28851)）。要解决此问题，您需要更换驱动程序。建议卸载 `amdvlk` 和 `lib32-amdvlk` 软件包，改为安装 `vulkan-radeon`（详见 [#14141](https://github.com/zed-industries/zed/issues/14141)）。

更多信息可参考 [Arch 的 Vulkan 指南](https://wiki.archlinux.org/title/Vulkan)，其中提供的步骤适用于大多数发行版。

#### 强制 Zed 使用特定 GPU

有以下几种方法可强制 Zed 使用特定 GPU：

##### 方案 A

通过设置 `ZED_DEVICE_ID={device_id}` 环境变量，指定需要 Zed 使用的 GPU 设备 ID。

您可以通过运行 `lspci -nn | grep VGA` 获取 GPU 的设备 ID，该命令会将每个 GPU 单独一行输出，例如：

```
08:00.0 VGA compatible controller [0300]: NVIDIA Corporation GA104 [GeForce RTX 3070] [10de:2484] (rev a1)
```

此处的设备 ID 是 `2484`。该值为十六进制格式，若要强制 Zed 使用此特定 GPU，您需要按如下方式设置环境变量：

```
ZED_DEVICE_ID=0x2484 zed
```

如果选择在 `.bashrc` 或类似文件中全局定义该变量，请确保将其导出。

##### 选项 B

如果使用 Mesa，可以运行 `MESA_VK_DEVICE_SELECT=list zed --foreground` 获取可用 GPU 列表，然后通过导出 `MESA_VK_DEVICE_SELECT=xxxx:yyyy` 来选择特定设备。此外，还可以通过额外导出 `WAYLAND_DISPLAY=""` 回退至 xwayland。

##### 选项 C

使用 [vkdevicechooser](https://github.com/jiriks74/vkdevicechooser)。

#### 报告图形问题

若 Vulkan 已正确配置，但 Zed 仍无法正常运行，请提交 issue 并提供尽可能详细的信息。

在 GitHub 上报告因图形初始化错误导致 Zed 无法启动的问题时，可能无法像我们在问题模板中指导的那样运行 `zed: copy system specs into clipboard` 命令。针对这种情况，我们特别提供了另一种收集系统规格的方式。

通过向 Zed 传递 `--system-specs` 参数，例如：

```sh
zed --system-specs
```

即可在终端中输出系统规格信息。强烈建议将输出内容原样复制到 GitHub 的 issue 中，因其采用 Markdown 格式确保可读性。

此外，在报告此类问题时，提供 Zed 日志内容会非常有帮助。日志通常位于 `~/.local/share/zed/logs/Zed.log`。生成有用日志文件的推荐流程如下：

```sh
truncate -s 0 ~/.local/share/zed/logs/Zed.log # Clear the log file
ZED_LOG=blade_graphics=info zed .
cat ~/.local/share/zed/logs/Zed.log
# copy the output
```

或者，如果您已设置 Zed 命令行工具，可以执行：

```sh
ZED_LOG=blade_graphics=info /path/to/zed/cli --foreground .
# copy the output
```

同时强烈建议在将日志粘贴到 GitHub 问题时，使用以下模板：

> **_注意_**：模板中的空格非常重要，若不保留会导致格式错误。

````
<details><summary>Zed Log</summary>

```
{zed 日志内容}
```

</details>
```

这将使日志默认处于折叠状态，让问题描述更易于阅读。

### 我无法打开任何文件

### 点击链接无效

这些功能由 XDG 桌面门户提供，具体包括：

- `org.freedesktop.portal.FileChooser`
- `org.freedesktop.portal.OpenURI`

某些窗口管理器（例如 `Hyprland`）默认不提供文件选择器。请参阅[此列表](https://wiki.archlinux.org/title/XDG_Desktop_Portal#List_of_backends_and_interfaces)作为备选方案的起点。

### Zed 无法记住我的 API 密钥

### Zed 无法记住我的登录信息

这些功能同样需要 XDG 桌面门户支持，具体为：

- `org.freedesktop.portal.Secret` 或
- `org.freedesktop.Secrets`

Zed 需要一个安全存储密钥的位置（例如您的 Zed 登录凭证或 OpenAI API 密钥），我们使用系统提供的密钥环来实现此功能。提供此功能的软件包包括 `gnome-keyring`、`KWallet` 和 `keepassxc` 等。

### 无法启动 inotify

Zed 依赖 inotify 监控文件系统的变更。如果无法启动 inotify，Zed 将无法稳定运行。

如果你看到“打开文件过多”的提示，首先尝试执行`sysctl fs.inotify`。

- 应确保 max_user_instances 的数值为 128 或更高（可通过`sudo sysctl fs.inotify.max_user_instances=1024`调整此限制）。Zed 仅需 1 个 inotify 实例。
- 应确认`max_user_watches`的数值为 8000 或更高（可通过`sudo sysctl fs.inotify.max_user_watches=64000`调整此限制）。Zed 需要为每个打开项目中的目录创建监视节点（每个目录 1 个），每个 git 仓库额外需要 1 个，并为设置、主题、键位映射和扩展等功能预留少量节点。

也可能是文件描述符耗尽所致。可通过`ulimit`查看当前限制，并通过编辑`/etc/security/limits.conf`进行调整。

### 无声或输出设备异常

如果在 Zed 中听不到声音或音频被路由到错误的设备，可能是由于音频系统不匹配导致的。Zed 依赖 ALSA，而你的系统可能正在使用 PipeWire 或 PulseAudio。要解决此问题，你需要配置 ALSA 通过 PipeWire/PulseAudio 路由音频。

如果你的系统使用 PipeWire：

1. **安装 PipeWire ALSA 插件**

   在基于 Debian 的系统上，运行：

   ```bash
   sudo apt install pipewire-alsa
   ```

2. **配置 ALSA 使用 PipeWire**

   将以下配置添加到 ALSA 设置文件中。你可以使用 `~/.asoundrc`（用户级别）或 `/etc/asound.conf`（系统级别）：

   ```bash
   pcm.!default {
       type pipewire
   }

   ctl.!default {
       type pipewire
   }
   ```

3. **重启系统**

### 强制设置 X11 缩放比例

在 X11 系统中，Zed 会自动检测高 DPI 显示器的合适缩放比例。缩放比例按以下优先级顺序确定：

1. `GPUI_X11_SCALE_FACTOR` 环境变量（若已设置）
2. 从 X 资源数据库 (xrdb) 读取 `Xft.dpi`
3. 基于显示器分辨率与物理尺寸的 RandR 自动检测

若需自定义 Zed 自动检测之外的缩放系数，可通过以下方式实现：

#### 查看当前缩放系数

可通过以下命令验证 `Xft.dpi` 是否已设置：

```sh
xrdb -query | grep Xft.dpi
```

若该命令无输出，则表示 Zed 正在使用 RandR（X11 显示器管理扩展）根据显示器报告的分辨率与物理尺寸自动计算缩放系数。

#### 方案一：设置 Xft.dpi（X 资源数据库）

`Xft.dpi` 是标准 X11 设置项，被众多应用程序用于统一字体与界面缩放。设置此参数可确保 Zed 与其他遵循该设置的 X11 应用程序保持相同缩放效果。

编辑或创建 `~/.Xresources` 文件：

```sh
vim ~/.Xresources
```

添加以下内容（DPI 值请按需设定）：

```sh
Xft.dpi: 96
```

常见 DPI 数值：

- [[代码块_3]] 对应标准 1 倍缩放
- [[代码块_4]] 对应 1.5 倍缩放
- [[代码块_5]] 对应 2 倍缩放
- [[代码块_6]] 对应 3 倍缩放

加载配置：

[[代码块_0]]

重启 Zed 使更改生效。

#### 方案二：使用 GPUI_X11_SCALE_FACTOR 环境变量

此 Zed 专用环境变量可直接设置缩放系数，绕过所有自动检测功能。

[[代码块_1]]

您可使用小数数值（例如 [[代码块_7]]、[[代码块_8]]、[[代码块_9]]），或设置 [[代码块_10]] 来强制启用基于 RandR 的检测（即使已设置 [[代码块_11]]）。

如需永久生效，请将其添加至您的 shell 配置文件或桌面启动项。

#### 方案三：调整全局 RandR DPI 设置

此操作将修改整个 X11 会话报告的 DPI 值，影响 RandR 为所有使用该服务的应用程序计算缩放比例的方式。

将以下内容添加至 [[代码块_12]] 或 [[代码块_13]]：

[[代码块_2]]

将 `192` 替换为您所需的 DPI 值。这将全局影响系统，并在 `Xft.dpi` 未设置时由 Zed 的自动 RandR 检测功能使用。

### 字体渲染参数

当使用 Blade 渲染器（Linux 平台及启用 Blade 渲染器的自编译版本）时，Zed 会读取 `ZED_FONTS_GAMMA` 和 `ZED_FONTS_GRAYSCALE_ENHANCED_CONTRAST` 环境变量作为字体渲染参数值。

`ZED_FONTS_GAMMA` 对应 [getgamma](https://learn.microsoft.com/en-us/windows/win32/api/dwrite/nf-dwrite-idwriterenderingparams-getgamma) 数值。
允许范围：[1.0, 2.2]，超出范围的数值将被截断。
默认值：1.8

`ZED_FONTS_GRAYSCALE_ENHANCED_CONTRAST` 对应 [getgrayscaleenhancedcontrast](https://learn.microsoft.com/en-us/windows/win32/api/dwrite_1/nf-dwrite_1-idwriterenderingparams1-getgrayscaleenhancedcontrast) 数值。
允许范围：[0.0, ...)，超出范围的数值将被截断。
默认值：1.0