# 开机自启动

[返回目录](../README.md)

先按首页确认控制台能够正常连接设备，再配置自启动。
安装时使用完整解压后的目录，使随包动态库也能一并安装。

## Linux / 树莓派

在解压目录运行：

```bash
sudo ./install-service.sh ./linker-dashboard
sudo systemctl status linker-dashboard --no-pager
```

| 内容 | 位置 |
| --- | --- |
| 程序 | `/usr/local/bin/linker-dashboard` |
| 数据 | `/var/lib/linker-dashboard` |
| 环境配置 | `/etc/linker-dashboard.env` |

查看日志、停止或重启：

```bash
sudo journalctl -u linker-dashboard -n 100 --no-pager
sudo systemctl stop linker-dashboard
sudo systemctl restart linker-dashboard
```

脚本会配置已有的 ufw / firewalld，放行 TCP 7081 和 UDP 5353，并检查蓝牙配网相关依赖。
升级时停止服务，在新版本解压目录重新执行安装脚本，保留原有数据和配置。

停止开机自启但保留数据：

```bash
sudo systemctl disable --now linker-dashboard
```

## Windows

在解压目录用管理员 PowerShell 运行：

```powershell
Set-ExecutionPolicy -Scope Process Bypass
.\install-autostart.ps1
```

脚本创建 `LinkerBotDashboard` 计划任务，把程序和随包 DLL 安装到
`C:\Program Files\LinkerBot\Dashboard`，数据保存在 `%ProgramData%\LinkerBot\Dashboard`。

升级时在新版本目录重新执行脚本。卸载自启动但保留数据：

```powershell
.\install-autostart.ps1 -Uninstall
```

需要指定端口或数据目录时，使用脚本的 `-Port`、`-DataDir` 参数。
