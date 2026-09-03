# Changelog

本文件记录公开分发版本的用户可见变化。完整附件位于各版本的 GitHub Release 页面。

## [v26.9.0] - 2026-09-03

### Added

- 新增全型号剪刀石头布必胜模式与高频视觉跟踪（独立推理层，最高 1280×960 / 100 Hz）。
- 补齐 L6、O6、O7、L10、L20Lite、L20 经典 CAN 全功能码控制与前端功能码面板。
- 新增 Gesture 与 Dance 全链路：多型号手势数据模型、手势舞执行引擎与实时状态同步。
- 新增 CAN 帧日志（RX/TX）与多维过滤、明暗主题、Overview 四段式重构。

### Changed

- 上游 Go 依赖升级至 `gocan v1.1.0` 与 `socketcan-init v1.1.0`。
- 发布包随包携带 MiniCANFD 动态库：Linux amd64 `libcanbus.so`、Linux arm64
  `libcanbus_arm64.so`、Windows amd64 `HCanbus.dll`；Windows arm64 不含动态库。

### Fixed

- 修复流式上报设备（O20）读取极慢、并发写 WebSocket panic、重新扫描后设备消失等问题。

## [v26.8.4] - 2026-09-02

### Fixed

- 将并联灵巧手的型号名称和标识从 `L25` 统一更正为 `L20`。
- 同步设备识别、型号描述、内置手势/手势舞、2D 跟随映射、状态展示和文档。
- 保留已有 `L20Lite` 型号不变。

## [v26.8.3] - 2026-08-25

### Changed

- 将控制台默认监听端口从 `8080` 调整为 `7081`。
- 同步前端开发代理、Linux/Windows 自启动、树莓派部署检查和防火墙规则。
- 更新移动端控制器地址提示、安装说明和故障排查文档。
- 保留通过 `--addr` 或 Windows `-Port` 参数自定义监听端口的能力。

## [v26.8.2] - 2026-08-18

### Added

- 增加全型号 WRC 手势舞，并完善剪刀石头布必胜模式与高频视觉识别。
- 为 O6 和 O6i 手势舞增加独立严格时间线。

### Changed

- 更新 L10、L30、O30i 的内置手势、对指和 WAIC 序列。
- 完善 Linux 主机 BLE 发现、移动端密码提示和窄屏布局。

### Fixed

- 将 C0 查询返回 `NONE` 的设备识别为 O6i，并根据 CAN ID 判断左右手。
- 升级 `nanoid` 到安全版本，前端依赖审计恢复为 0 个已知漏洞。

## [v26.8.1] - 2026-08-04

### Added

- 新增云端账号认证、控制器注册和 WebSocket 指令中继。
- 支持控制器主动连接公网云服务，无需为树莓派开放入站端口。
- 增加 BLE 无屏配网，可从手机配置 Wi-Fi 并关联云端账号。
- 增加 Android、iOS 移动端工程，以及局域网发现、云端登录和蓝牙配置界面。
- 提供 Linux systemd、Windows 开机自启、云服务和树莓派部署工具。
- Release 新增可直接安装的 Android 调试版 APK。

### Changed

- 局域网控制不再要求配对码，并允许多个操作员同时接入。
- 手势舞运行、停止和设备状态通过后端统一维护并同步给所有客户端。
- Android 与 iOS 图标、启动页统一使用 LinkerBot 品牌素材。

## [v26.8.0] - 2026-08-04

### Added

- 增加全型号剪刀石头布必胜模式和高频视觉跟踪。
- 补齐经典 CAN 功能码控制与状态查询界面。
- 增加中英文界面切换。
- 提供 Linux/Windows 的 amd64 与 arm64 四个平台安装包。

### Fixed

- 修复前端依赖安全告警，发布时依赖审计为 0 个已知漏洞。
- 替换存在安全风险的路由依赖，同时保留单页应用的无刷新导航。
- 优化主题、交互反馈、日志区域和页面布局。

### Notes

- 这是接入云端服务器、移动端账号和 BLE 配对功能前的最后一个稳定版本。

## [v26.7.0] - 2026-07-25

### Added

- 建立 Linux amd64、Linux arm64、Windows amd64、Windows arm64 四平台打包流程。
- 发布包内嵌前端、型号描述、手势与手势舞数据，可使用单个控制台程序运行。
- 增加 SHA-256 独立校验文件和汇总 `SHA256SUMS`。

### Changed

- `gocan` 更新到 `v1.0.0`。
- `socketcan-init` 更新到 `v1.0.1`，移除本地 `replace` 依赖。

[v26.8.4]: https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.8.4
[v26.8.3]: https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.8.3
[v26.8.2]: https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.8.2
[v26.8.1]: https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.8.1
[v26.8.0]: https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.8.0
[v26.7.0]: https://github.com/zhuzx17/linker-dashboard/releases/tag/v26.7.0
