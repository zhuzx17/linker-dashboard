# 蓝牙配网

[返回目录](../README.md) · 相关：[云端账号](cloud.md)

蓝牙用于给无屏主机配置 Wi-Fi 和关联账号。配置完成后，实际控制通过局域网或云端进行。

## 准备主机

支持树莓派及 Linux 电脑，要求蓝牙适配器支持低功耗蓝牙广播，并安装 BlueZ。
需要从手机写入 Wi-Fi 时，还需 NetworkManager：

```bash
sudo apt install -y bluez network-manager
sudo systemctl enable --now bluetooth
```

已联网的主机可沿用现有网络，不必重新配置 Wi-Fi。
Windows 主机目前使用[局域网连接](mobile.md)，不提供本功能的蓝牙广播。

## 手机操作

1. 启动或重启主机上的控制台，默认开放 15 分钟配网窗口。
2. 手机开启蓝牙，允许 App 使用附近设备权限，并登录云端账号。
3. 选择“通过蓝牙添加”，选择名称以 `LH-` 开头的控制器。
4. 选择沿用网络、手机当前 Wi-Fi 或其他 Wi-Fi，按提示输入名称和密码。
5. 点击配置并关联，等待完成；回到控制器列表，在线后即可控制。

Android 可以读取当前 Wi-Fi 名称，但不能获取已保存的密码，密码需手动输入。
加密 Wi-Fi 密码至少 8 位；开放网络密码留空。

未联网主机可以先通过蓝牙获取网络配置，云端关联需要主机随后能访问云服务。

## 配网窗口已关闭

安装为服务的主机可以运行：

```bash
sudo systemctl restart linker-dashboard
```

终端直接启动的主机则重启控制台程序。仍无法扫描时，查看[故障排查](troubleshooting.md)。
