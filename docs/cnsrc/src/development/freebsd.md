# 在 FreeBSD 上构建 Zed

请注意，FreeBSD 目前并非受支持平台，因此以下内容仍在完善中。

## 代码仓库

克隆 [Zed 代码仓库](https://github.com/zed-industries/zed)。

## 依赖项

- 安装必要的系统软件包和 rustup：

  ```sh
  script/freebsd
  ```

  如果习惯手动操作，可查阅 [`script/freebsd`](https://github.com/zed-industries/zed/blob/main/script/freebsd) 并逐步执行。

## 从源码构建

安装依赖项后，即可使用 [Cargo](https://doc.rust-lang.org/cargo/) 构建 Zed。

构建编辑器调试版本：

```sh
cargo run
```

运行测试：

```sh
cargo test --workspace
```

发布模式下，主要用户界面由 `cli` 包提供。开发环境中可通过以下命令运行：

```sh
cargo run -p cli
```

### WebRTC 说明

目前，在 FreeBSD 上构建 `webrtc-sys` 失败，原因是缺少上游支持且没有预构建的二进制文件。因此，一些依赖 WebRTC 的协作功能（音频通话和屏幕共享）暂时被禁用。

详情请参阅 [问题 #15309：FreeBSD 支持] 和 [讨论 #29550：Zed 的非官方 FreeBSD 移植版]。

## 故障排除

### Cargo 报错称某个依赖项使用了不稳定功能

尝试 `cargo clean` 和 `cargo build`。