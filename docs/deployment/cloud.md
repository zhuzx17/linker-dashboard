# 自建云服务

[返回目录](../README.md)

本章供服务器管理员使用。只操作灵巧手的用户，请直接阅读[云端账号](../guide/cloud.md)。

## 准备

需要 Linux 服务器、管理员权限和可供手机及控制器访问的 HTTPS 地址。
建议使用独立子域名；已有网站的服务器可通过独立虚拟主机转发到云服务。
下面的 `cloud.example.com` 需要替换为自己的地址。

## 安装云进程

下载服务器架构对应的 Linux 发布包，在解压目录运行：

```bash
sudo ./install-cloud-service.sh ./linker-cloud
sudoedit /etc/linker-cloud.env
```

首次创建管理员时，在环境文件中填写 `LINKER_CLOUD_BOOTSTRAP_USERNAME` 和
`LINKER_CLOUD_BOOTSTRAP_PASSWORD`；用户名至少 3 位，密码至少 10 位。

```bash
sudo chmod 0600 /etc/linker-cloud.env
sudo systemctl restart linker-cloud
sudo journalctl -u linker-cloud -n 50 --no-pager
```

确认管理员已创建后，删除两项初始化值并重启服务。数据库保存在
`/var/lib/linker-cloud/cloud.db`，升级前停服务并备份整个数据目录。

## HTTPS 入口

云进程默认监听 `127.0.0.1:8090`。在 Nginx 或 Caddy 中为独立域名配置有效证书，将
`/healthz` 和 `/api/v1/*` 转发到该地址，并支持 WebSocket 升级。

发布包 `cloud/` 中提供反向代理模板，使用前替换其中的示例地址和证书路径。
如果 80 / 443 未被占用且已准备 Docker 和 DNS，可使用脚本附带的 Caddy 部署：

```bash
sudo env START_PROXY=1 LINKER_CLOUD_DOMAIN=cloud.example.com \
  ./install-cloud-service.sh ./linker-cloud
curl https://cloud.example.com/healthz
```

已有网站时使用现有反向代理，不要直接覆盖其配置。
必须维护证书续期，App 和控制器都应验证服务端证书。

## 接入控制器

先在 App 设置同一云地址并登录，生成一次性设备接入码。
优先通过[蓝牙添加](../guide/bluetooth.md)传给 Linux 控制器；
也可在主机终端中交互输入后启动一次：

```bash
read -r -s -p "设备接入码: " enrollment_code
printf '\n'
sudo ./linker-dashboard --data /var/lib/linker-dashboard \
  --cloud-url https://cloud.example.com --cloud-enrollment-code "$enrollment_code"
unset enrollment_code
```

命令参数可能被本机管理员查看，建议在受控主机完成首次登记。
成功后凭据写入控制台数据目录，之后使用同一数据目录即可自动连接。
Windows 主机也支持通过[启动参数](../reference/startup-options.md)指定云地址和设备接入码。
