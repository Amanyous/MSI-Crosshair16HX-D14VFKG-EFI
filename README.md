# MSI Crosshair 16 HX D14VFKG EFI

微星 Crosshair 16 HX D14VFKG 黑苹果 EFI，更新于 2026-11-11。
本 EFI 驱动基于 NullMoth 的 1401 修改。
本配置在独显模式下可用，混合模式尚未测试。
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
| 内置屏幕 | AUO / AUO07B2，2.5K，240 Hz |
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
| RTX 4060 Laptop | ✓ | 需要进入系统后安装 [nullmoth](https://github.com/nullmoth/nvidia-macos-driver) 驱动。 |
| Intel 集成显卡 | x | 不可用。 |
| 内屏 / HiDPI | ✓ | 默认只支持 1280 × 600 和 2500 × 1600。如需合适的分辨率，安装 NullMoth 驱动后按其说明配置 `nvfbhidpi=1`，修改 EDID 并导入，再在 BetterDisplay 中调整分辨率。上传的 EFI 未预置此参数。 |
| 屏幕亮度 | ✓ | 在 1401 App 中点击 `Turn brightness on`，或手动在 OpenCore 的 `boot-args` 中添加 `nvrmacpibl=1`，重启后生效；开启后亮度调节正常。上传的 EFI 未预置此参数。 |
| AX211 Wi-Fi | ✓ | 当前使用 AirportItlwm，已开启 Wi-Fi 6，不支持 WPA3。 |
| 蓝牙 | ✓ | 可用。 |
| Realtek 有线网卡 | 待验证 | 已配置 RealtekRTL8111，未完成实际联网验证。 |
| ALC256 扬声器 | ✓ | 可用。 |
| Intel 数字麦克风（DMIC） | ✓ | OhMyDSP 核心 0.3.3 / HAL 0.3.4；本机已有实际收音反馈，需要安装完整系统组件。 |
| 键盘 | ✓ | 可用。 |
| 触控板 | ✓ | 可用。 |
| 电池电量 | ✓ | 电量不准确，不会及时同步。 |
| 睡眠 / 唤醒 | x | 曾出现睡眠后重启，尚未确认修复。 |
| 隔空传送（AirDrop） | x | MacBook 无法发现本机，当前配置尚未解决。 |
| 安卓 / iPhone USB 网络共享 | ✓ | 可用。 |

## BIOS 设置

本机 BIOS 快捷键：`Alt + F2 + 右 Shift + Copilot`（用户提供）。
以下设置针对 MSI Crosshair 16 HX D14VFKG，其他机型的按键和选项名称可能不同。

| 设置项 | 设置值 |
| --- | --- |
| 安全启动（Secure Boot） | 关闭（Disabled） |
| Intel VMD | 关闭（Disabled） |
| 快速启动（Fast Boot） | 关闭（Disabled） |
| SATA 模式 | AHCI |

保存设置后启动安装介质。RTX 4060 Laptop 使用的 NullMoth 驱动需要进入系统后另行安装。

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

上传的 EFI 已移除 NullMoth 的 NVIDIA 启动参数、配套 AMFI / IPC 启动参数及 `IONDRVSupport` 屏蔽项，进入系统后须按 NullMoth 上游说明安装驱动并配置。表格中的可用状态为本机安装驱动后的反馈。
本机已安装驱动的系统曾因移除 AMFI 参数导致黑屏；此次仅清理上传副本，U 盘正在使用的配置保持不变。
不要将这份配置直接覆盖到不同硬件上。第三方二进制与源码遵循各自上游许可证。
