# MSI Crosshair 16 HX D14VFKG EFI

微星 Crosshair 16 HX D14VFKG 黑苹果 EFI，更新于 2026-11-11。
当前运行 macOS 26.7.3（25G342），SMBIOS 使用 MacBookPro16,2。
本配置针对本机调试，同为第 14 代 Intel 处理器不代表可以直接通用。

## 硬件信息

| 项目 | 硬件 |
| --- | --- |
| 机型 | MSI Crosshair 16 HX D14VFKG |
| 处理器 | Intel Core i7-14700HX |
| 芯片组 | Intel HM770 |
| 内存 | Samsung 32 GB（16 GB × 2），5600 MHz |
| 独立显卡 | NVIDIA GeForce RTX 4060 Laptop GPU，8 GB，10de:28a0 |
| 集成显卡 | Intel Raptor Lake-S UHD Graphics，8086:a788 |
| 内置屏幕 | AUO / AUO07B2，240 Hz；当前 HiDPI 界面显示为 1920 × 1200 |
| 无线网卡 | Intel Wi-Fi 6E AX211，8086:7a70 |
| 有线网卡 | Realtek RTL8111/8168 系列千兆网卡，10ec:8168 |
| 音频 | Realtek ALC256；Intel Raptor Lake HDA / DSP，8086:7a50 |
| 内置麦克风 | Intel 数字麦克风（DMIC），使用 OhMyDSP |
| NVMe 控制器 | Samsung PM9C1a（144d:a80d）、BM9C1（144d:a80f） |
| USB 控制器 | Intel Raptor Lake XHCI，8086:7a60 |

## 可用状态

✓：已有可用反馈或运行证据；x：当前不可用或存在阻断问题。
“待验证”表示证据不足，不代表硬件不受支持。带 bug 的 ✓ 不代表功能完全正常。

| 硬件 / 功能 | 是否可用 | 备注 / bug |
| --- | :---: | --- |
| i7-14700HX / 系统启动 | ✓ | 当前系统可启动；CPU 名称已修正。未完整验证功耗和全部性能表现。 |
| 32 GB 内存 | ✓ | 系统识别 32 GB，两条 16 GB。 |
| RTX 4060 Laptop | ✓ | 当前自定义驱动可输出画面，系统报告 Metal 4；曾观察到 Siri 球体渲染时 GPU device lost。 |
| Intel 集成显卡 | 待验证 | 已识别硬件，不宣称具备 macOS 图形加速。 |
| 内屏 / HiDPI | ✓ | 当前界面 1920 × 1200 @ 240 Hz，渲染分辨率 3840 × 2400；渲染分辨率不等于面板物理分辨率。 |
| AX211 Wi-Fi | ✓ | 当前使用 AirportItlwm；尚未换用 ForbiddenItlwm。 |
| 蓝牙 | 待验证 | 控制器已开启；未完整验证连接、音频及连续互通功能。 |
| Realtek 有线网卡 | 待验证 | 已配置 RealtekRTL8111，未完成实际联网验证。 |
| ALC256 扬声器 / 耳机 | 待验证 | 内建输出已枚举；尚未完整验证各输出路径。启动音问题单列如下。 |
| Intel 数字麦克风（DMIC） | ✓ | OhMyDSP 核心 0.3.3 / HAL 0.3.4；本机已有实际收音反馈，需要安装完整系统组件。 |
| Siri 使用数字麦克风 | ✓ | bug：需先切换到“内建麦克风”，再切换到“Internal Microphone (DMIC)”才能使用。 |
| NVMe 存储 | 待验证 | 两个控制器见硬件清单；未逐盘验证 macOS 读写、休眠和长期稳定性。 |
| USB / 2.4 GHz 接收器 | ✓ | USBMap 已生效；曾出现开机后接收器需重新插拔，最新修改仍待冷启动确认。 |
| 键盘 / 触控板 | 待验证 | EFI 包含 VoodooPS2、VoodooI2C / HID，未完整核对所有按键与手势。 |
| 电池电量 | x | 曾出现拔电显示 1%、接电变 100%；当前未确认修复。 |
| 睡眠 / 唤醒 | x | 曾出现睡眠后重启，尚未确认修复。 |
| 隔空传送（AirDrop） | x | MacBook 无法发现本机，当前配置尚未解决。 |
| OpenCore 开机声音 | x | AudioDxe 已识别 ALC256，但仍反馈无声；最新扬声器、延迟和增益修改待重启验证。 |
| 安卓 / iPhone USB 网络共享 | 待验证 | 已加入对应配置 / 驱动，尚未分别完成联网确认。 |

## 数字麦克风驱动

[下载 OhMyDSP RPL-S 完整驱动包](OhMyDSP-RPLS-Install-20261111.zip)

驱动在本机可用。压缩包包含 kext、HAL、daemon、SOF 固件、拓扑、
源码和中文安装 / 回退步骤；这不是双击安装的 `.pkg`。
只复制 EFI 内的 OhMyDSP.kext 不足以完成系统麦克风安装。

1. 解压后阅读 `README-中文.md`，按文档安装系统组件并核对 EFI 条目。
2. 首次安装后手动重启；启动后核心默认等待约 30 秒初始化。
3. 在系统声音输入中选择 `Internal Microphone (DMIC)`。
4. Siri 无响应时，先将输入切换为“内建麦克风”，再切回数字麦克风驱动。

Siri 切换步骤为本机用户反馈的临时方法，未修复底层切换问题。
第三方应用授权、睡眠唤醒和共享音频控制器的稳定性仍需验证。
本驱动针对本机 RPL-S / 8086:7a50 与对应 DMIC 拓扑，不是第 14 代电脑通用驱动。

## 使用前

仓库的 `EFI/BOOT`、`EFI/OC` 是展开后的文件目录；Releases 另有 EFI 快照压缩包。
备份配置、隐藏元数据和日志未上传；序列号、MLB、UUID、ROM 及蓝牙控制器身份已清除，
使用前须自行生成 SMBIOS 身份信息。U 盘原始 EFI 未因上传而改动。

保留现有显卡、无线驱动及本机 AMFI 参数；本机曾因移除 AMFI 参数导致黑屏。
不要将这份配置直接覆盖到不同硬件上。第三方二进制与源码遵循各自上游许可证。
