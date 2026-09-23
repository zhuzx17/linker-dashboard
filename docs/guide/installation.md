# 安装与平台要求

[返回目录](../README.md) · 下一步：[连接设备](devices.md)

## 选择安装包

从 [GitHub Releases](https://github.com/zhuzx17/linker-dashboard/releases/latest) 下载与操作系统和架构匹配的压缩包。
普通 Intel / AMD 电脑选择 amd64，运行 64 位系统的树莓派选择 linux-arm64。Linux 可运行 `uname -m` 确认：
`x86_64` 对应 amd64，`aarch64` 对应 arm64。

Android 下载 APK。当前提供调试签名 APK；iOS 暂无可直接下载的安装包。
Android App 用于连接电脑或树莓派上的控制台，不能代替主机连接 CAN 适配器。

## Windows

1. 完整解压 ZIP，保留程序和 DLL 在同一目录。
2. 按所用适配器准备驱动及动态库。
3. 双击 `linker-dashboard.exe`，浏览器打开 <http://127.0.0.1:7081>。
4. 需要手机连接时，在防火墙提示中允许专用网络访问。

| 适配器 | 准备事项 | 用途 |
| --- | --- | --- |
| MiniCANFD | windows-amd64 包自带的 `HCanbus.dll`、WinUSB 驱动、厂家要求的 VC++ x64 运行库 | CAN FD |
| PEAK PCAN | PEAK 官方驱动、与程序架构匹配的 `PCANBasic.dll` | 经典 CAN |
| CANable 2.0 SLCAN-FD | 确认系统能识别串口；官方设备自动扫描 | CAN FD |

MiniCANFD 不支持 windows-arm64；不要用 x86 或旧版 DLL 替换随包 DLL。
运行 MiniCANFD 不需要 `.h` 或 `.lib`。

兼容 SLCAN-FD 设备未被自动发现时，在 PowerShell 中指定串口：

```powershell
.\linker-dashboard.exe --slcan COM5,COM6
```

## Linux / 树莓派

使用支持 SocketCAN 的 Linux 系统。Ubuntu / Debian 可先安装：

```bash
sudo apt update
sudo apt install -y iproute2 udev
```

解压安装包，在解压目录运行：

```bash
chmod +x linker-dashboard
sudo ./linker-dashboard
```

本机访问 <http://127.0.0.1:7081>；其他设备使用 `http://<主机 IP>:7081`。
SocketCAN 网口初始化需要管理员权限。MiniCANFD 使用随包动态库，不会在系统中显示为 `can0` 网口。

BlueZ 和 NetworkManager 仅在需要[蓝牙配网](bluetooth.md)时准备。
长期运行可配置[开机自启动](../deployment/autostart.md)。

## 校验下载文件

每个附件都有对应的 `.sha256` 文件。Linux 在下载目录执行，例如：

```bash
sha256sum -c linker-dashboard-v26.9.2-linux-amd64.tar.gz.sha256
```

Windows PowerShell 中比较以下两个结果的哈希值：

```powershell
Get-FileHash .\linker-dashboard-v26.9.2-windows-amd64.zip -Algorithm SHA256
Get-Content .\linker-dashboard-v26.9.2-windows-amd64.zip.sha256
```

将示例版本号替换为实际下载版本。下载完整版本的全部附件后，也可运行 `sha256sum -c SHA256SUMS`。
