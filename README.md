# XMillet

面向小米官方墓碑机制（Millet）的 LSPosed 模块。通过 LibXposed API 102 接入 `system_server` 与「电量与性能」（`com.miui.powerkeeper`），对冻结与省电策略做可视化控制，并提供日志、统计与诊断面板。

LSPosed module for Xiaomi's official tombstone mechanism (Millet). Built on LibXposed API 102, it hooks `system_server` and PowerKeeper (`com.miui.powerkeeper`) to expose freezing and power-saving policies, together with log, statistics and diagnostic panels.

## 功能特性 / Features

- **策略控制**：查看并调整墓碑冻结策略、免冻白名单、强力冻结与消息应用等逐应用配置
- **亮灭屏门控**：亮屏解冻豁免覆盖实时前台、白名单、系统关键 UID、前台服务与活跃无障碍服务
- **双开应用**：按 `userId = 999` 识别双开空间，应用列表与配置按主应用 / 双开空间分组
- **输入法保护**：当前默认输入法在冻结、解冻与终止路径统一豁免
- **日志与统计**：墓碑统计、冻结耗时记录与日志导出
- **诊断**：钩子安装点覆盖、XposedService 能力检查、作用域状态与失效目标热重载
- **备份与恢复**：配置备份/恢复，bugreport 以 `tar.gz` 打包
- **热重载**：`autoHotReload` 开启时，代码更新后自动重载钩子

## 环境要求 / Requirements

| 项目 | 要求 |
| --- | --- |
| 设备 | 小米 / 红米，搭载澎湃 OS（HyperOS）官方墓碑机制（Millet） |
| 系统版本 | Android 13 及以上（`minSdk 33`，`targetSdk 34`） |
| 框架 | LSPosed 2.2.x，支持 LibXposed API 102 |
| 作用域 | 系统框架（`system`）、电量与性能（`com.miui.powerkeeper`） |
| 架构 | arm64-v8a |

未搭载官方墓碑机制的 ROM（AOSP、类原生、其它厂商）不在适配范围内。

## 安装 / Installation

1. 下载并安装 APK
2. 在 LSPosed 管理器中启用 XMillet，勾选「系统框架」与「电量与性能」作用域
3. 重启系统，或等待热重载完成
4. 打开 XMillet，按引导完成初始化

## 使用 / Usage

- **主页**：显示系统服务连接状态、当前冻结概览与快捷开关
- **应用**：逐应用查看进程、UID、冻结状态，并调整策略
- **统计**：墓碑统计与冻结耗时
- **日志**：按级别筛选、搜索与导出
- **设置**：策略默认值、日志开关、备份与恢复、诊断

## 下载 / Download

- 本仓库 Releases
- 源码仓库 Releases：https://github.com/univers629/XMillet/releases

## 源码 / Source

https://github.com/univers629/XMillet

## 反馈 / Feedback

https://github.com/univers629/XMillet/issues

## 许可证 / License

GPL-3.0

## 免责声明 / Disclaimer

模块会修改系统冻结与省电行为，使用前请确认设备与系统版本符合要求；因使用本模块导致的异常由使用者自行承担。
