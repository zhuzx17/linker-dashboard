# 树莓派批量部署

[返回目录](../README.md)

单台树莓派可直接使用 Linux arm64 发布包，按[首页](../../README.md)启动或安装[自启服务](autostart.md)。
本章的一键脚本适合维护多台设备，需要在 Linux 开发机上取得源码并准备构建环境。

## 一键部署

1. 为树莓派准备 64 位 Linux，启用 SSH，并确保部署账号有 sudo 权限。
2. 将树莓派接到开发机指定网卡所在的网络。
3. 在源码仓库根目录执行，示例网卡名需替换为实际值：

```bash
./scripts/one-click-deploy.sh enp1s0
```

脚本会检查 DHCP、发现可信设备、准备 SSH 公钥并部署全部匹配的树莓派。
缺少密钥时会隐藏询问部署账号密码；后续使用密钥登录。
DHCP 应用于部署设备所在网络，避免与其他 DHCP 服务冲突。

主机架构相同的设备共享一次构建结果。具体构建依赖见
[Gitea 开发指南](https://gitea.linkerhub.work/vertex/linker_dashboard/src/branch/main/docs/development/build.md)。

## 分步检查

在源码目录运行：

```bash
./scripts/setup-host-dhcp.sh --check
./scripts/discover-linkerhand.sh
./scripts/deploy-raspberry-pi.sh
```

也可以指定目标：

```bash
SSH_IDENTITY="$HOME/.ssh/id_ed25519" ./scripts/deploy-raspberry-pi.sh linkerhand@linkerhand.local
```

这里的密钥是部署机用来登录树莓派的 SSH 密钥，与日常手机控制无关。

## 部署后使用

手机与控制器在同一局域网时可以直接连接，不要求控制器能访问公网。
之后可用[蓝牙配网](../guide/bluetooth.md)配置 Wi-Fi；访问云端控制需要主机能联网。
多台控制器应分别关联账号，不能共用一次性设备接入码。
