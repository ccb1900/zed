# Zed 在 Linux 系统上的安装

## 标准安装方法

对于大多数用户，我们推荐使用[下载页面](https://zed.dev/download)提供的脚本安装 Zed：

```sh
curl -f https://zed.dev/install.sh | sh
```

我们还提供 Zed 的预览版本，该版本会比稳定版提前约一周获得更新。您可以通过以下命令安装：

```sh
curl -f https://zed.dev/install.sh | ZED_CHANNEL=preview sh
```

通过脚本安装的 Zed 在以下系统中运行效果最佳：

- 配备兼容 Vulkan 的 GPU（例如搭载 M 系列芯片的 MacBook 上的 Linux 系统）
- 具备系统级 glibc 环境（NixOS 和 Alpine 系统默认不包含）
  - x86_64 架构（Intel/AMD）：glibc 版本 ≥ 2.31（Ubuntu 20 及以上版本）
  - aarch64 架构（ARM）：glibc 版本 ≥ 2.35（Ubuntu 22 及以上版本）

Nix和Alpine系统均有第三方Zed软件包可供使用（不过目前版本会滞后几周）。若希望使用我们官方构建的版本，需安装glibc兼容层方可运行。在NixOS系统上可尝试[nix-ld](https://github.com/Mic92/nix-ld)，Alpine系统则需安装[gcompat](https://wiki.alpinelinux.org/wiki/Running_glibc_programs)。

以下情况需要从源码编译安装：

- 非64位Intel或64位ARM架构设备（例如32位或RISC-V架构机器）
- 所有架构的Redhat Enterprise Linux 8.x、Rocky Linux 8、AlmaLinux 8、Amazon Linux 2系统
- aarch64架构的Redhat Enterprise Linux 9.x、Rocky Linux 9.3、AlmaLinux 8、Amazon Linux 2023系统（x86_x64架构不受影响）

## 其他Linux系统安装方式

Zed是开源软件，支持[从源码安装](./development/linux.md)。

### 通过包管理器安装

有多个第三方Zed软件包适用于不同的Linux发行版和包管理器，有时以`zed-editor`形式提供。您可以通过以下软件包安装Zed：

- Flathub：[`dev.zed.Zed`](https://flathub.org/apps/dev.zed.Zed)
- Arch：[`zed`](https://archlinux.org/packages/extra/x86_64/zed/)
- Arch（AUR）：[`zed-git`](https://aur.archlinux.org/packages/zed-git)、[`zed-preview`](https://aur.archlinux.org/packages/zed-preview)、[`zed-preview-bin`](https://aur.archlinux.org/packages/zed-preview-bin)
- Alpine：`zed`（[aarch64](https://pkgs.alpinelinux.org/package/edge/testing/aarch64/zed)）（[x86_64](https://pkgs.alpinelinux.org/package/edge/testing/x86_64/zed)）
- Nix：`zed-editor`（[unstable](https://search.nixos.org/packages?channel=unstable&show=zed-editor)）
- Fedora/Ultramarine（Terra）：[`zed`](https://github.com/terrapkg/packages/tree/frawhide/anda/devs/zed/stable)、[`zed-preview`](https://github.com/terrapkg/packages/tree/frawhide/anda/devs/zed/preview)、[`zed-nightly`](https://github.com/terrapkg/packages/tree/frawhide/anda/devs/zed/nightly)
- Solus：[`zed`](https://github.com/getsolus/packages/tree/main/packages/z/zed)
- Parabola：[`zed`](https://www.parabola.nu/packages/extra/x86_64/zed/)
- Manjaro：[`zed`](https://packages.manjaro.org/?query=zed)
- ALT Linux（Sisyphus）：[`zed`](https://packages.altlinux.org/en/sisyphus/srpms/zed/)
- AOSC OS：[`zed`](https://packages.aosc.io/packages/zed)

请查阅 [Repology](https://repology.org/project/zed-editor/versions) 获取各软件仓库中 Zed 软件包的完整列表。

安装第三方软件包时请注意，这些版本可能不是最新版本，且可能与官方打包的 Zed 存在细微差异（常见改动包括将二进制文件重命名为 `zedit` 或 `zeditor` 以避免与其他软件包冲突）。

我们诚挚邀请您协助让 Zed 惠及更多用户。若您使用的包管理器尚未收录 Zed，且愿意协助完善，请参阅我们整理的 [打包指南](./development/linux.md#notes-for-packaging-zed)。

### 手动下载安装

如果您倾向于手动安装，可通过下载我们预构建的 .tar.gz 压缩包完成安装。该文件与自动安装脚本使用的文件完全相同，但您可通过以下步骤自定义安装路径：

下载 `.tar.gz` 文件：

- [zed-linux-x86_64.tar.gz](https://zed.dev/api/releases/stable/latest/zed-linux-x86_64.tar.gz) ([预览版](https://zed.dev/api/releases/preview/latest/zed-linux-x86_64.tar.gz))
- [zed-linux-aarch64.tar.gz](https://zed.dev/api/releases/stable/latest/zed-linux-aarch64.tar.gz)
  ([预览版](https://zed.dev/api/releases/preview/latest/zed-linux-aarch64.tar.gz))

然后确保压缩包中的 `zed` 可执行文件位于您的系统路径中。最简单的方法是解压压缩包并创建符号链接：

```sh
mkdir -p ~/.local
# extract zed to ~/.local/zed.app/
tar -xvf <path/to/download>.tar.gz -C ~/.local
# link the zed binary to ~/.local/bin (or another directory in your $PATH)
ln -sf ~/.local/zed.app/bin/zed ~/.local/bin/zed
```

如果您需要与符合 XDG 标准的桌面环境集成，还需安装 `.desktop` 文件：

```sh
cp ~/.local/zed.app/share/applications/zed.desktop ~/.local/share/applications/dev.zed.Zed.desktop
sed -i "s|Icon=zed|Icon=$HOME/.local/zed.app/share/icons/hicolor/512x512/apps/zed.png|g" ~/.local/share/applications/dev.zed.Zed.desktop
sed -i "s|Exec=zed|Exec=$HOME/.local/zed.app/libexec/zed-editor|g" ~/.local/share/applications/dev.zed.Zed.desktop
```

## 卸载 Zed

### 标准卸载方式

如果 Zed 是通过默认安装脚本安装的，可以通过在 `zed` shell 命令中添加 `--uninstall` 标志来卸载

```sh
zed --uninstall
```

如果没有出现错误，shell 会提示您选择保留还是删除偏好设置。做出选择后，您应该会看到 Zed 已成功卸载的提示信息。

如果在您的 PATH 环境变量中找不到 `zed` shell 命令，可以尝试以下命令之一

```sh
$HOME/.local/bin/zed --uninstall
```

或

```sh
$HOME/.local/zed.app/bin.zed --uninstall
```



























