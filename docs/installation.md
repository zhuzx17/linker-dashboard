# 安装与首次运行

## 1. 选择安装包

先确认操作系统和 CPU 架构：

| 系统 | 架构 | 文件名后缀 |
| --- | --- | --- |
| 常见 Intel/AMD Linux | x86_64 / amd64 | `linux-amd64.tar.gz` |
| 树莓派、ARM Linux | aarch64 / arm64 | `linux-arm64.tar.gz` |
| 常见 Intel/AMD Windows | AMD64 | `windows-amd64.zip` |
| ARM Windows | ARM64 | `windows-arm64.zip` |

Linux 可执行 `uname -m` 查看架构。输出 `x86_64` 选择 amd64，输出 `aarch64` 选择 arm64。

下载后先按 [README](../README.md#文件完整性) 校验 SHA-256，再解压运行。不要直接运行来源
不明或校验失败的二进制。

## 2. Linux 与树莓派

以下示例以 v26.8.3 arm64 为例：

```bash
tar -xzf linker-dashboard-v26.8.3-linux-arm64.tar.gz
cd linker-dashboard-v26.8.3-linux-arm64
chmod +x linker-dashboard
sudo ./linker-dashboard --addr :7081 --data /var/lib/linker-dashboard
```

控制台通常需要 root 或 `CAP_NET_ADMIN` 权限来初始化 SocketCAN。启动成功后在本机验证：

```bash
curl http://127.0.0.1:7081/api/controller/info
```

浏览器访问 `http://127.0.0.1:7081`。手机或其他电脑与控制台在同一局域网时，访问
`http://<Linux 主机 IP>:7081`。

### 安装为开机自启服务

v26.8.3 的 Linux 包包含 systemd 安装脚本：

```bash
sudo ./install-service.sh
sudo systemctl status linker-dashboard --no-pager
```

安装位置：

| 内容 | 路径 |
| --- | --- |
| 控制台程序 | `/usr/local/bin/linker-dashboard` |
| 运行数据 | `/var/lib/linker-dashboard` |
| 环境配置 | `/etc/linker-dashboard.env` |
| systemd 服务 | `linker-dashboard.service` |

查看日志和重启：

```bash
sudo journalctl -u linker-dashboard -n 100 --no-pager
sudo systemctl restart linker-dashboard
```

树莓派要使用手机 BLE 配网，应安装并启动 BlueZ；要让 App 写入新 Wi-Fi，还需要
NetworkManager：

```bash
sudo apt update
sudo apt install -y bluez network-manager
sudo systemctl enable --now bluetooth
```

## 3. Windows

1. 解压与系统架构匹配的 zip，不要在压缩包预览窗口内直接运行。
2. 连接适配器并安装对应驱动。
3. 在解压目录打开 PowerShell，执行：

```powershell
.\linker-dashboard.exe --addr :7081 --data .\data
```

4. Windows 防火墙提示时允许“专用网络”访问。
5. 浏览器打开 `http://127.0.0.1:7081`。

### Windows 适配器

- PEAK PCAN 用于经典 CAN。安装 PEAK 官方驱动，并将与程序架构一致的
  `PCANBasic.dll` 放在程序目录或 `PATH` 中。
- 官方 CANable 2.0 SLCAN-FD（VID:PID `16D0:117E`）可自动发现，用于 CAN FD。
- 其他兼容 SLCAN-FD 固件需要显式指定串口：

```powershell
.\linker-dashboard.exe --slcan COM5,COM6
```

### Windows 开机自启

v26.8.3 包含 `install-autostart.ps1`。以管理员身份打开 PowerShell，在解压目录执行：

```powershell
Set-ExecutionPolicy -Scope Process Bypass
.\install-autostart.ps1
```

脚本会把程序安装到 `C:\Program Files\LinkerBot\Dashboard`，数据保存到
`C:\ProgramData\LinkerBot\Dashboard`，创建开机任务并放行 TCP 7081 与 UDP 5353。

## 4. 首次数据释放与升级

控制台把基础型号描述、手势和手势舞内嵌在二进制中。仅当 `--data` 指定的目录不存在或为空
时，程序才会释放预设；目录已有任何文件时不会覆盖。

升级前建议：

1. 停止控制台服务。
2. 备份整个 data 目录。
3. 替换程序文件，保留原 data 目录。
4. 启动后确认型号、手势和设备覆盖配置正常。

要重新生成新版本的内嵌预设，应先备份并移走旧 data 目录，再启动新程序。不要直接删除仍需
保留的自定义手势和手势舞。

## 5. 连接并控制设备

1. 接好电源、灵巧手和 CAN 适配器。
2. 打开控制台，进入设备管理区域。
3. 点击重新连接或重新扫描，等待型号和左右手识别完成。
4. 勾选要控制的设备，先使用低速和简单手势验证方向。
5. 再执行手势舞或批量控制。

多人使用时，设备状态和运动状态由控制台后端统一广播。新运动会终止受影响设备上正在运行的
旧运动，再执行新动作。
