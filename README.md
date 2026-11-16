# MSI Crosshair 16 HX D14VFKG EFI

微星 Crosshair 16 HX D14VFKG 黑苹果 EFI，功能状态更新于 2026-11-15。
本 EFI 驱动基于 NullMoth 的 1401 修改。
本仓库 EFI 已包含 [MSI Control](https://github.com/Amanyous/MSI-Control/releases/tag/v0.3.23-build139) 所需的 EFI 改动，包括 MSIECBridge 驱动和配套键盘配置，无需重复修改这些条目。MSI Control 应用仍需在系统内另行安装并运行。
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
| AX211 Wi-Fi | ✓ | 当前使用 [ForbiddenItlwm](https://github.com/retropineaple/ForbiddenItlwm)，本机安装版本为 0.5.4-beta.2，搭配 BTLEAdvFixup 0.9.1；已开启 Wi-Fi 6，不支持 WPA3。 |
| 蓝牙 | ✓ | 可用。 |
| Realtek 有线网卡 | 待验证 | 已配置 RealtekRTL8111，未完成实际联网验证。 |
| ALC256 扬声器 | ✓ | 可用。 |
| Intel 数字麦克风（DMIC） | ✓ | OhMyDSP 核心 0.3.3 / HAL 0.3.4；本机已有实际收音反馈，需要安装完整系统组件。 |
| 键盘 | ✓ | 可用。 |
| F4 触控板开关 | ✓ | 需要 MSI Control。 |
| F5 麦克风禁用 | ✓ | 需要 MSI Control。 |
| F6 蓝牙开关 | ✓ | 需要 MSI Control。 |
| Num Lock 提示灯 | ✓ | 需要 MSI Control。 |
| 键盘功能提示灯 | ✓ | 需要 MSI Control。 |
| 触控板 | ✓ | 可用。 |
| 内置相机 | ✓ | FaceTime 可用；bug：Photo Booth 黑屏。 |
| Apple 账户 | ✓ | 本机可用。上传文件已清除三码，使用前须自行生成并填写；不要共用他人的三码，以降低账户异常风险，不保证避免封号。 |
| 电池电量 | ✓ | 电量不准确，不会及时同步。 |
| 睡眠 / 唤醒 | x | NullMoth 3.0 下仍复现睡眠后自动重启，报告为 EFI Sleep Wake Failure `0x1f`；原因仍在排查。 |
| 隔空传送（AirDrop） | ✓ | 本机已确认可用。 |
| 安卓 / iPhone USB 网络共享 | ✓ | 可用。 |

### 键盘与 MSI Control

F4 可切换触控板，F5 可禁用麦克风，F6 可切换蓝牙，均需要运行 MSI Control。
Num Lock 和键盘功能提示灯已确认正常，也需要 MSI Control。
这些状态不表示所有功能键都已统一为按住 Fn 才触发。

以上为本机当前系统的实测反馈，不代表仅复制仓库 EFI 就能获得全部功能。
显卡、数字麦克风及 MSI Control 相关功能还需要对应的系统驱动或应用。

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

## HiDPI 安装

本仓库提供 [HiDPI-Minimal 显示覆盖配置](HiDPI-Minimal/) 和 [完整安装 / 恢复教程](HiDPI-Minimal/安装教程.md)，适用于本机 AUO07B2（厂商 ID `6af`、产品 ID `7b2`）2560 × 1600 面板。

1. 先进入系统，按 [NullMoth 项目说明](https://github.com/nullmoth/nvidia-macos-driver) 安装显卡驱动，在实际启动的 OpenCore `boot-args` 中加入 `nvfbhidpi=1`。上传的 EFI 未预置该参数。
2. 下载仓库 ZIP 并解压，把其中的 `HiDPI-Minimal` 文件夹放到桌面。已有显示覆盖文件时，先按完整教程备份。
3. 在终端执行以下命令安装本机覆盖配置：

```sh
sudo mkdir -p /Library/Displays/Contents/Resources/Overrides/DisplayVendorID-6af
sudo install -o root -g wheel -m 644 "$HOME/Desktop/HiDPI-Minimal/DisplayVendorID-6af/DisplayProductID-7b2" /Library/Displays/Contents/Resources/Overrides/DisplayVendorID-6af/DisplayProductID-7b2
```

4. 手动重启，运行 `nvram boot-args` 确认包含 `nvfbhidpi=1`。
5. 安装 [BetterDisplay（GitHub）](https://github.com/waydabber/BetterDisplay)，在应用中开启 HiDPI，并调整分辨率；240 Hz 是否可选以实际显示为准。

目标 HiDPI 档位包括 1280 × 800、1440 × 900、1600 × 1000 和 1920 × 1200，是否全部可用及能否同时使用 240 Hz 仍需实机确认。

**文件类型说明：** 本包的 `DisplayProductID-7b2` 是 macOS 显示覆盖 plist，不含修改版 EDID。它应安装到上面的 Overrides 路径，不能在 BetterDisplay 的“加载 EDID 数据”中导入。如果另外采用修改 EDID 的方案，需要准备单独的 EDID 文件。本教程不写入屏幕 EEPROM，也不需要修改 `/System`。

随包教程中“此前已配置”“无需重复安装”等描述指作者本机；首次使用仓库配置时，请按上述步骤满足前置条件。

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

仓库的 `EFI/BOOT`、`EFI/OC` 为 2026-11-16 硬盘 EFI 的脱敏副本；旧 Releases 压缩包不代表当前目录版本。
备份配置、隐藏元数据和日志未上传；序列号、MLB、UUID、ROM 及蓝牙控制器身份已清除，
使用前须自行生成 SMBIOS 身份信息。硬盘和 U 盘原始 EFI 均未因上传而改动，Microsoft 启动目录不包含在仓库中。

上传的 EFI 已移除 NullMoth 的 NVIDIA 启动参数、配套 AMFI / IPC 启动参数、`IONDRVSupport` 屏蔽项和 NullMothSafe 工具，进入系统后须按 NullMoth 上游说明安装驱动并配置。表格中的可用状态为本机安装驱动后的反馈。
本机已安装驱动的系统曾因移除 AMFI 参数导致黑屏；此次仅清理上传副本，U 盘正在使用的配置保持不变。
不要将这份配置直接覆盖到不同硬件上。第三方二进制与源码遵循各自上游许可证。

### 填写三码与 ROM

先备份现有可启动 EFI，使用 ProperTree 打开准备安装的 `EFI/OC/config.plist`，编辑 `PlatformInfo → Generic`。不要修改正在使用的唯一启动配置后直接覆盖。

1. 使用 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) 为 `MacBookPro16,2` 生成一组新的身份信息，不要使用真实 Mac 或其他人的三码。
2. 将生成结果按下表填写；保留本配置的 `SystemProductName=MacBookPro16,2`，不要随意改机型。
3. 保存并检查 plist，先通过备用启动介质确认可启动，再替换硬盘 EFI。已有正常工作的个人身份时，优先从自己的备份恢复，避免反复生成和更换。

| Generic 字段 | 类型 | 填写内容 |
| --- | --- | --- |
| `SystemSerialNumber` | String | GenSMBIOS 的 Serial |
| `MLB` | String | GenSMBIOS 的 Board Serial |
| `SystemUUID` | String | GenSMBIOS 的 SmUUID |
| `ROM` | Data | 本机固定网卡的 MAC 地址，去掉冒号后共 12 个十六进制字符，即 6 字节；不是蓝牙地址 |

例如 `AA:BB:CC:DD:EE:FF` 在 Data 编辑框中填写为 `AABBCCDDEEFF`，仅为格式示例，不要照抄。不要把 12 个字符作为普通字符串填写。
三码和 ROM 不要上传到 GitHub。填写正确也不保证 Apple 服务一定可用，更不能保证账户不会受限。可参考 [OpenCore Apple 服务配置说明](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html)。

### 蓝牙控制器配置

蓝牙身份与 SMBIOS 三码不是同一套数据，也不能用 ROM 或网卡 MAC 代替 `bluetoothInternalControllerInfo`。
上传副本已移除原机的蓝牙 NVRAM 注入值，保留蓝牙驱动及 USB 映射。

1. 首次使用先保留这些身份注入项缺省，从这份 EFI 启动后检查系统是否能正常识别蓝牙；正常则不需要手工补写。
2. 如果有同一台机器、同一个蓝牙控制器的正常 EFI 备份，可从该备份恢复 `NVRAM → Add → 7C436110-AB2A-4BBB-A880-FE41995C9F82` 下原有的蓝牙项，保持原类型和值。不要复制作者或其他机器的数据。
3. `bluetoothInternalControllerInfo` 如需恢复，应使用本机正常备份中的完整 Data，不能按其他机型模板拼接。没有可靠备份时，不要随便填零或伪造地址。
4. `bluetoothExternalDongleFailed` 是状态变量，不是控制器地址；不应拿它填写蓝牙 MAC。
5. 没有备份又无法启用时，先检查 IntelBluetoothFirmware / BlueToolFixup 的加载情况及蓝牙所在 USB 端口映射，再按所用驱动的说明处理。不要把“补身份”当作所有蓝牙故障的通用修复。

重新配对可能是必要的。重置 NVRAM 会影响其他启动设置，不作为安装必做步骤；确需操作时先保留备用启动介质与配置备份。

### 本次 EFI 更新

包含当前硬盘的 CPUFriend / CPUFriendDataProvider 和 MSIECBridge 0.4.9。EC 驱动的新增频率采集接口需要匹配的 MSI Control 应用；上方旧版下载链接不代表包含全部新功能。全核频率采集受内核版本限制，未验证其他机器的兼容性。
