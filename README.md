# Linker Dashboard

LinkerBot 灵巧手控制台，支持电脑浏览器和 Android 手机操作。

## 下载与准备

从 [GitHub 下载最新版](https://github.com/zhuzx17/linker-dashboard/releases/latest)，也可使用 [Gitea 下载](https://gitea.linkerhub.work/vertex/linker_dashboard/releases)。

| 平台 | 选择的安装包 | 启动前需要 |
| --- | --- | --- |
| Windows，Intel / AMD 电脑 | `windows-amd64.zip` | 适配器驱动；MiniCANFD 使用随包的 `HCanbus.dll` 和厂家要求的 VC++ x64 运行库 |
| Windows，ARM 电脑 | `windows-arm64.zip` | 与架构匹配的适配器驱动；暂不支持 MiniCANFD |
| Linux，Intel / AMD 电脑 | `linux-amd64.tar.gz` | `iproute2`、`udev`；使用 SocketCAN 时需管理员权限 |
| 64 位 Linux，树莓派 / ARM 主机 | `linux-arm64.tar.gz` | 与 Linux 电脑相同，使用 64 位操作系统 |
| Android 7.0 及以上 | `.apk` | 已启动控制台的电脑或树莓派；手机与主机连接同一局域网 |

请完整解压安装包，保留主程序旁边的动态库。使用发布包无需安装 Go 或 Node.js。
适配器的具体准备步骤见[安装指南](docs/guide/installation.md)。

## Windows 启动

双击解压后的 `linker-dashboard.exe`，保持程序窗口开启，在浏览器访问：

<http://127.0.0.1:7081>

## Linux / 树莓派启动

在解压目录打开终端：

```bash
chmod +x linker-dashboard
sudo ./linker-dashboard
```

本机浏览器访问 <http://127.0.0.1:7081>。从手机或其他电脑访问时，使用
`http://<控制台主机 IP>:7081`。

## Android 启动

安装 APK 后，打开 App，扫描附近控制器并选择已启动的主机；扫描不到时，手动输入
`http://<控制台主机 IP>:7081`。局域网连接无需账号或配对码。
当前 APK 为调试签名版本，适合现场测试和内部使用。

进入页面后，在“设备管理”中勾选灵巧手，即可调节关节、执行手势或手势舞。
首次连接遇到问题时，查看[设备连接](docs/guide/devices.md)。

[完整使用指南](docs/README.md) · [版本记录](CHANGELOG.md)
