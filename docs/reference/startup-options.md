# 启动参数

[返回目录](../README.md)

通常使用默认值即可。需要自定义时，在终端启动程序并追加参数。

| 参数 | 默认 | 说明 |
| ---- | ---- | ---- |
| `--addr` | `:7081` | HTTP/HTTPS 监听地址 |
| `--data` | `./data` | 运行时数据目录（手势库、控制服务身份等） |
| `--slcan` | 空 | Windows 额外 SLCAN-FD 串口，逗号分隔 |
| `--minicanfd-lib` | `MINICANFD_LIBRARY_PATH` | MiniCANFD 厂商动态库路径；未配置时自动查找随包库文件 |
| `--name` | `LinkerBot <hostname>` | 手机发现列表中的控制服务名称 |
| `--advertise` | `true` | 通过 mDNS 广播 `_linkerhand._tcp.local` |
| `--allowed-origins` | Capacitor/Ionic 本机 Origin | 允许跨域连接的 App Origin 列表 |
| `--tls-cert` / `--tls-key` | 空 | 同时提供时启用 HTTPS/WSS |
| `--cloud-url` | `LINKER_CLOUD_URL` | 公网云服务地址；配置后主动建立出站 WSS |
| `--cloud-enrollment-code` | `LINKER_CLOUD_ENROLLMENT_CODE` | 首次云端登记使用的一次性接入码 |
| `--ble-provisioning` | Linux 为 `true` | 通过 BlueZ 提供无屏 BLE 配网；非 Linux 平台默认关闭 |
| `--ble-window` | `15m` | 每次启动开放 BLE 配网的时长；`0` 表示持续开放 |
| `--debug` | `false` | 开启 debug 日志 |

## 示例

修改端口与数据目录：

```powershell
.\linker-dashboard.exe --addr :7082 --data C:\LinkerDashboard\data
```

指定控制器名称：

```bash
sudo ./linker-dashboard --name "展台左侧"
```

Linux 上持续开放蓝牙配网窗口：

```bash
sudo ./linker-dashboard --ble-window 0
```

完整参数可运行 `linker-dashboard --help` 查看。
云端接入码仅首次登记需要；配网、云服务和 HTTPS 配置见对应部署章节。
