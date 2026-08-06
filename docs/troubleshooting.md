# 故障排查

## 控制台无法启动

先在终端运行程序查看错误，不要只双击后观察窗口闪退。

Linux：

```bash
sudo ./linker-dashboard --addr :8080 --data /var/lib/linker-dashboard --debug
```

Windows PowerShell：

```powershell
.\linker-dashboard.exe --addr :8080 --data .\data --debug
```

如果提示端口被占用，更换端口，例如 `--addr :8081`，并用新端口访问。

## 浏览器或 App 无法连接

在控制台主机上执行：

```bash
curl http://127.0.0.1:8080/api/controller/info
```

- 本机也失败：检查程序是否运行、端口是否被占用以及服务日志。
- 本机成功、手机失败：确认两端在同一网络，并放行 TCP 8080。
- 手动 IP 可用、自动扫描不到：检查 UDP 5353、mDNS 和路由器的客户端隔离设置。
- 酒店或展会访客 Wi-Fi 经常禁止终端互访，可改用自建热点或云端控制。

Linux 防火墙示例：

```bash
sudo ufw allow 8080/tcp
sudo ufw allow 5353/udp
```

## App 扫描不到蓝牙控制器

```bash
sudo systemctl status bluetooth --no-pager
sudo systemctl restart bluetooth
sudo systemctl restart linker-dashboard
sudo journalctl -u linker-dashboard -n 100 --no-pager
```

确认日志出现 BLE 配网已开放，手机蓝牙已开启并授予附近设备权限。默认窗口为启动后 15 分钟，
超时后需要重启服务。

## Wi-Fi 配置失败

```bash
nmcli device status
nmcli connection show
```

- 加密 Wi-Fi 密码至少 8 位；开放网络密码留空。
- 检查 SSID 大小写和隐藏网络设置。
- 确认控制器安装了 NetworkManager，并且无线网卡未被其他网络管理器独占。
- 配网期间控制器暂时断开旧 Wi-Fi 属于正常现象，App 应等待它重新连接云端。

## 控制器在云端显示离线

```bash
curl https://8.145.58.150/healthz
sudo test -s /var/lib/linker-dashboard/cloud.json
sudo journalctl -u linker-dashboard -n 100 --no-pager
```

控制器需要能出站访问 TCP 443，系统时间必须正确。`cloud.json` 缺失时需重新使用设备接入码或
BLE 关联；文件存在时不要随意删除或复制到其他设备。

## CAN 设备未识别

通用检查：

1. 确认灵巧手供电和急停状态。
2. 确认 CAN_H、CAN_L、地线和终端电阻。
3. 确认选择了匹配的经典 CAN 或 CAN FD 适配器。
4. 在控制台点击重新连接，再观察 CAN 日志。
5. 确认型号、左右手和固件版本与所选设备一致。

Linux 可查看 SocketCAN：

```bash
ip -details link show type can
```

Windows PCAN：

- 安装 PEAK 官方驱动；
- 使用与程序架构相同的 `PCANBasic.dll`；
- 确认 DLL 位于程序目录、`PATH` 或 `PCANBASIC_DLL_PATH` 指定位置。

Windows SLCAN-FD：

- 在设备管理器确认 COM 口；
- 非官方 VID/PID 设备使用 `--slcan COM5` 显式指定；
- 不要把普通串口误填为 SLCAN-FD。

## 手势或手势舞没有更新

程序不会覆盖已有 data 目录。升级后如果需要新版本内嵌预设：

1. 停止服务。
2. 备份旧 data 目录。
3. 将旧目录改名或移走。
4. 启动新版本，让程序释放新预设。
5. 按需迁移自定义文件，不要覆盖同名的新格式文件。

## APK 无法覆盖安装

通常是旧 APK 与新 APK 的签名不同。备份必要配置，卸载旧版本后重新安装。当前 Release 中的
APK 使用调试签名，不应与未来正式签名版本混装。
