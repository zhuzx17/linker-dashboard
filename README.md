# Linker Dashboard

Linker Dashboard 是 LinkerBot 全系列灵巧手的统一控制台。本仓库用于公开分发 Gitea
主项目构建出的 Linux、Windows 和 Android 安装包，并提供版本记录与使用教程；这里不是
源代码镜像。

源代码、工单和开发工作位于
[Gitea 主项目](https://gitea.linkerhub.work/vertex/linker_dashboard)（访问权限以 Gitea
仓库设置为准）。

## 下载最新版

当前推荐版本为 **v26.9.0**。

| 平台 | 适用设备 | 下载 |
| --- | --- | --- |
| Linux amd64 | 常见 x86-64 电脑、工控机和服务器 | [tar.gz](https://github.com/zhuzx17/linker-dashboard/releases/download/v26.9.0/linker-dashboard-v26.9.0-linux-amd64.tar.gz) |
| Linux arm64 | 运行 64 位 Linux 的树莓派和 ARM 主机 | [tar.gz](https://github.com/zhuzx17/linker-dashboard/releases/download/v26.9.0/linker-dashboard-v26.9.0-linux-arm64.tar.gz) |
| Windows amd64 | 常见 Intel/AMD Windows 电脑 | [zip](https://github.com/zhuzx17/linker-dashboard/releases/download/v26.9.0/linker-dashboard-v26.9.0-windows-amd64.zip) |
| Windows arm64 | ARM64 Windows 设备 | [zip](https://github.com/zhuzx17/linker-dashboard/releases/download/v26.9.0/linker-dashboard-v26.9.0-windows-arm64.zip) |
| Android | Android 手机和平板 | [APK](https://github.com/zhuzx17/linker-dashboard/releases/download/v26.9.0/linker-dashboard-v26.9.0-android-debug.apk) |

[下载 v26.9.0 的全部附件和 SHA-256 校验文件](https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.9.0)

> Android APK 当前使用调试证书签名，适合现场测试和内部使用。正式商店发布前需要改用长期
> 保存的 release keystore；不同签名的 APK 不能直接覆盖安装。

## 版本选择

| 版本 | 建议用途 | 主要能力 |
| --- | --- | --- |
| [v26.9.0](https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.9.0) | 当前完整版本 | 剪刀石头布必胜模式、经典 CAN 全功能码、Gesture/Dance 全链路与 CAN 帧日志；发布包随包携带 MiniCANFD 动态库 |
| [v26.8.4](https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.8.4) | 稳定回退版本 | 将并联灵巧手型号从 L25 统一更正为 L20 |
| [v26.8.3](https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.8.3) | 当前完整版本 | 默认使用 7081 端口，并同步开发、部署和移动端配置 |
| [v26.8.2](https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.8.2) | 动作与设备维护版本 | O6i 识别维护、L10/L30/O30i 动作更新、WRC 手势舞、移动端交互修复 |
| [v26.8.1](https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.8.1) | 云端与移动端基础版本 | 局域网免配对、多操作员状态同步、云端控制、BLE 无屏配网、Android APK |
| [v26.8.0](https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.8.0) | 不需要云端或手机配对的稳定部署 | 全型号控制、手势与手势舞、游戏中心、Windows/Linux 四平台 |
| [v26.7.0](https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.7.0) | 旧环境兼容与回退 | 基础控制台、Windows/Linux 四平台发布包 |

完整变更见 [CHANGELOG.md](CHANGELOG.md)。

## 快速开始

1. 从上表下载与系统架构匹配的压缩包。
2. 使用 Release 中的 `SHA256SUMS` 校验文件完整性。
3. 解压后启动 `linker-dashboard` 或 `linker-dashboard.exe`。
4. 在浏览器打开 `http://127.0.0.1:7081`；同一局域网内使用
   `http://<控制台主机 IP>:7081`。
5. 连接 CAN/PCAN/SLCAN 适配器，在设备区重新连接并确认识别结果。

详细步骤：

- [安装与首次运行](docs/installation.md)
- [手机、局域网、蓝牙与云端控制](docs/mobile-cloud.md)
- [故障排查](docs/troubleshooting.md)

## 支持的设备与适配器

- 经典 CAN 型号：L6、O6、O7、L10、L20Lite、L20
- CAN FD 型号：L30、O20、O30i
- Linux：SocketCAN
- Windows 经典 CAN：PEAK PCAN
- Windows CAN FD：CANable 2.0 SLCAN-FD 及兼容设备

具体能力由设备固件、适配器和所用版本共同决定。控制运动前应确认左右手、型号和目标设备
选择正确，并在安全空间内低速测试。

## 文件完整性

Linux：

```bash
sha256sum -c linker-dashboard-v26.9.0-linux-amd64.tar.gz.sha256
```

Windows PowerShell：

```powershell
Get-FileHash .\linker-dashboard-v26.9.0-windows-amd64.zip -Algorithm SHA256
Get-Content .\linker-dashboard-v26.9.0-windows-amd64.zip.sha256
```

两处哈希值必须完全一致。校验失败时不要运行文件，应删除后重新下载。

下载了该版本全部主要附件时，也可以使用 `sha256sum -c SHA256SUMS` 一次性校验。
