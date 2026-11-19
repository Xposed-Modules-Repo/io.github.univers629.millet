<div align="center">

<img src="https://raw.githubusercontent.com/univers629/XMillet/main/imgs/icon.webp" width="160" height="160" alt="XMillet icon" />

# XMillet

### 面向小米澎湃 OS 官方墓碑机制（Millet）的现代 LSPosed 控制与统计面板

![Android](https://img.shields.io/badge/Android-13%2B%20(API%2033)-3DDC84?style=flat-square&logo=android&logoColor=white)
![HyperOS](https://img.shields.io/badge/HyperOS-OS1.0%20~%20OS4.0-FF6900?style=flat-square&logo=xiaomi&logoColor=white)
![LSPosed](https://img.shields.io/badge/LSPosed-API%20102-FF4088?style=flat-square)
![License](https://img.shields.io/badge/License-GPL--3.0-blue?style=flat-square)
![Downloads](https://img.shields.io/github/downloads/Xposed-Modules-Repo/io.github.univers629.millet/total?style=flat-square&label=downloads)

</div>

> [!CAUTION]
> **框架与冲突提示**：本模块基于 **libxposed API 102** 构建（如 [LSPosed](https://github.com/LSPosed)），请勿与其他同样作用于「电量与性能（`com.miui.powerkeeper`）」或墓碑冻结调度的模块同时启用，以免产生冲突。

## 简介

**XMillet** 是针对小米澎湃 OS 官方墓碑机制（**Millet**，系统底层由 `GreezeManagerService` 与 `PowerKeeper` 协同调度与执行）构建的 LSPosed 模块，基于 libxposed API 102 与 Miuix Compose UI 实现。它不替换系统原有冻结机制，而是接管系统侧与电量侧的关键冻结/解冻闸门，提供状态观测、分应用策略与唤醒拦截统计面板。

LSPosed module for Xiaomi's official tombstone mechanism (Millet). Built on LibXposed API 102, it hooks `system_server` and PowerKeeper (`com.miui.powerkeeper`) to expose freezing and power-saving policies, together with log, statistics and diagnostic panels.

- **支持范围**：Android 13+（`minSdk 33`），实机适配 HyperOS 1.0 / 2.0 / 3.0 / 4.0（Android 13 ~ 17）
- **LSPosed 作用域**：仅需勾选「系统框架（`system`）」与「电量与性能（`com.miui.powerkeeper`）」
- **交互设计**：基于 [Miuix](https://github.com/compose-miuix-ui/miuix)（`0.9.4`）构建，支持液态玻璃悬浮底栏、实时毛玻璃模糊、动态取色（Monet）、首次引导（OOBE）与桌面图标隐藏

## 界面预览

<div align="center">

| 主页总览 | 进程列表 | 墓碑统计 |
| :---: | :---: | :---: |
| <img src="https://raw.githubusercontent.com/univers629/MacrodownNoteAssets/main/dev/XMillet/1.jpg" width="240" alt="XMillet 主页总览" /> | <img src="https://raw.githubusercontent.com/univers629/MacrodownNoteAssets/main/dev/XMillet/2.jpg" width="240" alt="XMillet 进程列表" /> | <img src="https://raw.githubusercontent.com/univers629/MacrodownNoteAssets/main/dev/XMillet/3.jpg" width="240" alt="XMillet 墓碑统计" /> |

</div>

## 核心能力

- **冻结与解冻闸门**：接管 `freezeUids` / `freezePids` / `freezeAction` 与 `thawUid(s)` / `thawUidAsync` / `thawAll` 等关键入口；支持按开关拦截亮屏解冻（默认开启，并自动豁免当前前台应用、系统核心进程、默认输入法、已启用无障碍服务与白名单）、插电解冻、网络唤醒与同步 Binder 解冻，并在查杀检查路径提供阻止查杀保护
- **分应用策略**：逐应用配置免冻白名单、消息应用、强力冻结、有前台服务时不冻结、忽略网络唤醒与 Doze 黑白名单；双开空间（user 999）应用独立分组展示与调度；支持配置 JSON 备份/恢复与 `bugreport-*.tar.gz` 诊断日志打包分享
- **内核态观测**：通过跨进程 Binder 实时读取 `system_server` 中 Greeze 运行态与 Hook 实装状态；冻结状态取自 `/sys/fs/cgroup` 内核真值，进程内存按 `/proc/<pid>/smaps_rollup` 的 PSS 统计；集成 XposedService 能力检测、一键授权作用域与失效进程热重载
- **墓碑统计面板**：提供今日 / 近 7 天 / 开机以来三个窗口，涵盖总览指标、息屏唤醒频次、唤醒原因分类、分时亮灭屏分布、拦截明细、多口径应用排行与最近动态时间线

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
2. 在 LSPosed 管理器中启用 XMillet，仅勾选「系统框架（`system`）」与「电量与性能（`com.miui.powerkeeper`）」作用域
3. 重启系统，或等待 API 102 免重启热重载完成
4. 打开 XMillet，按引导完成初始化检测

## 实机核验

四台不同代际设备的实机核验 Hook 实装情况如下（分母为当前版本探测点位：系统侧 34 个、电量侧 29 个）：

| 设备 | 代号 | 系统版本 | Android | 内核 | Root | LSPosed | Hook 实装（系统侧 + 电量侧） |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Redmi K40 游戏增强版 (M2012K10C) | `ares` | OS1.0.4.0.TKJCNXM | 13 (SDK 33) | `4.14.186-g844fd19c6` | Magisk | v2.2.1-log (7940) | 25/34 + 25/29 |
| Xiaomi Pad 5 Pro 12.4 (22081281AC) | `dagu` | OS2.0.10.0.ULZCNXM | 14 (SDK 34) | `4.19.157-perf-gb57830672689` | Magisk | v2.2.1-log (7939) | 25/30* + 25/28* |
| REDMI K80 Pro (24122RKC7C) | `miro` | OS3.0.304.0.WOMCNXM | 16 (SDK 36) | `6.6.118-android15-8-gc44b714366cc-abogki519650608-4k` | KernelSU | v2.2.1 (7939) | 28/34 + 17/29 |
| Xiaomi 15 (24129PN74C) | `dada` | OS4.0.4.0.XOCCNXM | 17 (SDK 37) | `6.6.118-android15-8-gf72a02f47f4b-abogki551678766-4k` | KernelSU | v2.2.1 (7939) | 29/30* + 14/28* |

> `ares` 与 `miro` 数据为当前版本默认关闭 `simplifyLog` 下的实测值（开启 `simplifyLog` 后可额外启用 `S16–S18` 等日志点位）；标注 `*` 的 `dagu` 与 `dada` 为早期 `30 + 28` 点位基线下的历史核验数据。

## 使用 / Usage

- **主页**：系统服务连接状态、当前冻结概览与快捷开关
- **应用**：逐应用查看进程、UID、冻结状态，并调整策略
- **统计**：墓碑统计与冻结耗时
- **日志**：按级别筛选、搜索与导出
- **设置**：策略默认值、日志开关、备份与恢复、诊断

## 下载 / Download

- 本仓库 Releases：https://github.com/Xposed-Modules-Repo/io.github.univers629.millet/releases
- 源码仓库 Releases：https://github.com/univers629/XMillet/releases

## 反馈 / Feedback

https://github.com/univers629/XMillet/issues

## 支持

<details>
<summary>欢迎打赏</summary>

<sub>如果 XMillet 对你有所帮助，欢迎请作者喝杯咖啡</sub>

| | |
| :---: | :---: |
| <img src="https://raw.githubusercontent.com/univers629/XMillet/main/app/src/main/res/drawable-nodpi/qr_donate_alipay.webp" width="220" alt="赞赏码 1" /> | <img src="https://raw.githubusercontent.com/univers629/XMillet/main/app/src/main/res/drawable-nodpi/qr_donate_wechat.webp" width="220" alt="赞赏码 2" /> |

</details>

## 开源许可 / License

本项目基于 [GNU General Public License v3.0](https://github.com/univers629/XMillet/blob/main/LICENSE) 开源发布。

## 免责声明 / Disclaimer

模块会修改系统冻结与省电行为，使用前请确认设备与系统版本符合要求；因使用本模块导致的异常由使用者自行承担。

The module modifies system freezing and power-saving behavior; confirm the device and system version meet the requirements before use. Any anomaly caused by using this module is borne by the user.
