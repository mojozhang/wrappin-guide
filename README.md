# WrapPin 与 SideStore：安装、刷新、过期恢复说明书

整理日期：2026 年 10 月 31 日  
更新日期：2026 年 11 月 7 日（补充过期恢复、开发者信任与 .rppairing 修复）  
适用场景：自己的 iPhone，用免费 Apple 账号安装 WrapPin 标准版；Mac 用于首次安装和配对。  
本文依据 2026 年 10 月 31 日至 11 月 7 日的实际安装和过期恢复经历整理，不包含个人账号、设备标识或配对文件，可分享给别人。

## 快速查阅：我现在该做什么？

| 当前情况 | 应该怎么做 |
|---|---|
| 第一次安装 | 依次阅读第 3–7 节 |
| 两个 App 还能打开，想续签 | Wi-Fi + LocalDevVPN → SideStore → My Apps → Refresh All |
| SideStore 已过期打不开 | 参考“过期恢复”：用 Mac 上的 iLoader、原 Apple 账号覆盖安装 |
| 弹出“不受信任的开发者” | 设置 → 通用 → VPN 与设备管理 → 对应开发者账号 → 信任 |
| SideStore 已恢复 7 DAYS，WrapPin 仍 EXPIRED | 给 WrapPin 刷新；若配对报错，先通过 iLoader 放置配对文件 |
| 出现 `.rppairing UDID not found` | 参考“过期恢复”和第 9 节 E：Manage Pairing File → SideStore → Place |
| 离开 Wi-Fi 后 WrapPin 无法启动 | 第 8 节蜂窝网络引导 |

## 1. 先了解各个工具的作用

| 工具 | 安装位置 | 用途 |
|---|---|---|
| iLoader | Mac | 给 iPhone 首次安装 SideStore，管理设备配对文件 |
| SideStore | iPhone | 给 WrapPin 签名、安装，并定期续签 |
| LocalDevVPN | iPhone | 建立访问手机本机开发者服务的通道，不是代理上网工具 |
| WrapPin 标准版 | iPhone | 选择模拟位置、模拟路线、恢复真实位置 |

WrapPin 使用 iOS 开发者位置模拟服务。它不会改变公网 IP、Apple 账号地区或 App Store 地区，也不保证所有第三方 App 都接受模拟位置。

官方入口：

- WrapPin 项目和安装说明：https://github.com/suversal/WrapPin
- WrapPin 最新安装包：https://github.com/suversal/WrapPin/releases/latest
- iLoader：https://iloader.app
- SideStore 文档：https://docs.sidestore.io
- LocalDevVPN：从 WrapPin 项目 README 的 App Store 链接进入，核对应用名称。项目说明国区商店可能无法下载。

## 2. 本次使用的环境和结果

| 项目 | 本次记录 |
|---|---|
| iPhone 系统 | iOS 27.0 |
| Mac 上的 iLoader | 2.4.0 |
| SideStore | 日志显示曾安装 0.7.0-20260911.328+6032424a；最终成功时的版本未再次核实 |
| LocalDevVPN | 1.3.0 |
| WrapPin | 标准版 1.0.20，Build 66 |
| 最终结果 | 首次安装和模拟定位成功；11 月 7 日过期恢复过程中，SideStore 显示 7 DAYS，随后处理配对文件错误后用户反馈恢复成功 |

版本会更新。以后优先看官方最新说明，不必为了复现本文而专门安装旧版或 Nightly。遇到问题先检查现有配置，不要立即删除重装。

## 3. 安装前准备

- 实体 iPhone：本文所用 WrapPin 版本要求 iOS 27 或更高版本。
- 手机已设置锁屏密码。
- Mac 和一根可传数据的 USB 线。
- 一个能正常登录、接收双重认证验证码的 Apple 账号，免费账号即可。
- Wi-Fi 网络。
- 后续 iLoader 和 SideStore 使用同一个 Apple 账号。

免费签名一般只有 7 天有效期。需要在到期前刷新 SideStore 和 WrapPin。

## 4. 首次安装 SideStore

如果 SideStore 已安装，先跳到第 5 节尝试刷新，避免无必要的重装。

1. 在 Mac 从官方来源安装 iLoader。
2. 用数据线连接 iPhone，解锁手机。如果出现“信任此电脑”，在手机上确认。
3. 打开 iLoader，确认能选中这台手机。
4. 按提示登录 Apple 账号，并输入双重认证验证码。
5. 按官方当前说明安装合适的 SideStore 版本，通常先选 Stable。
6. 等待安装结束，中途不要拔线。
7. 如果 iLoader 提示撤销已有证书，先看清影响：可能让同一个账号在其他侧载工具中签名的 App 失效。

手机上完成以下设置：

1. “设置 → 通用 → VPN 与设备管理”，找到开发者账号并信任。
2. “设置 → 隐私与安全性 → 开发者模式”，按系统提示开启、重启并再次确认。
3. 如果系统提示没有可用开发团队，按项目说明访问 Apple 开发者注册页面并完成所需的免费账号设置；不必为标准版购买开发者会员。

## 5. 配置 LocalDevVPN，完成 SideStore 第一次刷新

1. 在 iPhone 安装并打开 LocalDevVPN。
2. 点击 Connect，按系统提示允许添加 VPN 配置。
3. 确认 LocalDevVPN 已连接，手机也连接 Wi-Fi。
4. 暂停其他正在占用系统 VPN 的工具，避免抢占本机隧道。
5. 打开 SideStore，用安装时的同一个 Apple 账号登录。
6. 在“My Apps”点击 SideStore 右侧的“5 DAYS / 7 DAYS”等剩余天数按钮，完成刷新。

判断结果：剩余有效期更新至接近 7 天，且没有失败提示。仅登录成功、看到自己的账号，或看到“5 DAYS”，都不代表已刷新成功。

如果报错，先看第 9 节。SideStore 自身刷新成功后，再安装 WrapPin。

## 6. 下载和安装 WrapPin

### 6.1 选标准版

免费 Apple 账号请选择文件名包含 `Standard` 的未签名 IPA。

本次文件：

`WrapPin-Standard-1.0.20-build66-unsigned.ipa`

不要误选：

- `WrapPin-Tunnel-…`：内置隧道版本，需要具备相应 Packet Tunnel 权限的签名证书和描述文件，免费签名不适用。
- `WLocApp-…`：是另一个项目，并非 WrapPin 安装包。

本次已从官方 Release 下载，并校验了标准版文件。SHA-256 为：

```text
7e2065489239572d983ebd08588d75e05d99221ba9e6f5ed32837f2b2dd4fb0b
```

这个校验值只对应上述 1.0.20 Build 66 标准版。新版本应使用对应 Release 公布的值。

Mac 校验命令：

```sh
shasum -a 256 ~/Downloads/WrapPin-Standard-1.0.20-build66-unsigned.ipa
```

### 6.2 传到手机并侧载

1. 把 IPA 从 Mac 通过 AirDrop 发到 iPhone，保存到“文件”App。
2. 确认 Wi-Fi 和 LocalDevVPN 已连接。
3. 打开 SideStore → My Apps → 左上角“＋”。
4. 从“文件”中选择刚才的 IPA。
5. 保持 SideStore 在前台，等待签名和安装完成。
6. 回到桌面，确认 WrapPin 图标出现并能打开。

按项目说明，应先下载 IPA 再从本地选择，避免使用“通过 URL 安装”导致部分 SideStore 版本误判 Bundle ID。

## 7. WrapPin 本机配对和首次验证

1. 打开 WrapPin，完成首次使用引导。
2. 进入“设备连接 → 配对本机”。
3. 允许本地网络访问。
4. 按引导打开“设置 → 隐私与安全性 → 开发者模式 → 与 WrapPin 配对”。
5. 如系统要求，先输入手机锁屏密码，再输入 WrapPin 显示的六位配对码。这是两种不同的码。
6. 返回 WrapPin，确认显示“已配对”。
7. 确认 LocalDevVPN 已连接。
8. 选择一个容易辨认的地点，点击“开始模拟定位”。
9. 等待“模拟定位中”，打开 Apple 地图，点击定位按钮，检查蓝点。
10. 测试结束后回到 WrapPin，点击“停止模拟并恢复”，保持前台直到完成，再核对真实位置。

国内地点可先试 GCJ-02，海外地点可先试 WGS84。如果存在偏移，切换坐标模式后再核对；这只是起点，并非所有地点的准确性保证。

## 8. 日常使用、续签和更新

### 每次使用

1. 连接 LocalDevVPN。
2. 打开 WrapPin，选地点或路线，开始模拟。
3. 用地图检查实际位置。
4. 用完点击“停止模拟并恢复”，确认真实位置恢复。

不要用“关闭 VPN”代替“停止模拟并恢复”。

### 没有 Wi-Fi，只使用 4G/5G 时怎么启动？

WrapPin 使用 LocalDevVPN 时，可以在纯蜂窝网络下启动模拟定位，但建立本机连接需要额外的引导步骤。不要把 SideStore 刷新时的 Wi-Fi 要求等同于 WrapPin 日常使用必须连接 Wi-Fi。

1. 在 WrapPin → 设置 → 隧道跳转应用，确认选择 **LocalDevVPN**。
2. 先在 WrapPin 选好地点，点击“开始模拟定位”。
3. 按提示打开 LocalDevVPN，连接本地隧道，再返回 WrapPin。
4. 出现蜂窝网络引导后，**暂时关闭蜂窝数据**。不要用飞行模式代替这一步。
5. 回到 WrapPin，等待自动发现本机；必要时点击“继续”。
6. 等安全连接建立后，**按页面提示重新打开蜂窝数据**。
7. 确认显示“模拟定位中”，再到 Apple 地图检查实际位置。

关闭蜂窝数据只是建立连接时的临时步骤。连接建立后可以恢复移动数据，不需要在整个模拟期间关闭。

注意：

- 这套临时关闭蜂窝数据的引导适用于 **LocalDevVPN**。其他隧道应用不使用这套流程；如果在蜂窝网络下连接失败，按官方说明切换 Wi-Fi 重试。
- 真正连接 Wi-Fi 时，不要误选“我正在使用蜂窝网络”。
- 此步骤来自官方使用手册；本次已收到用户关于纯蜂窝网络启动失败的反馈，尚未收到按此流程操作后的成功反馈。
- 如果仍报错，保留完整错误或复制诊断信息，核对 WrapPin 的连接检测页面，不要直接重装或修改已经能正常使用的 SideStore 登录配置。

官方来源：https://github.com/suversal/WrapPin/blob/main/Documentation/UserGuide.zh-CN.md#wi-fi-与蜂窝网络

### 每周续签

1. 到期前连接 Wi-Fi 和 LocalDevVPN。
2. 打开 SideStore → My Apps。
3. 点击 Refresh All，或分别刷新 SideStore 和 WrapPin。
4. 确认两者有效期都已延长，没有失败提示。

建议每隔几天主动检查一次，不要等到最后一天。此指南没有自动创建提醒。

### 过期恢复：SideStore 已打不开怎么办？

免费签名到期后，通常需要电脑重新签名。先保留手机上的 SideStore 和 WrapPin，尝试覆盖安装，以保留原有数据。

1. 用 USB 数据线连接 iPhone 和 Mac，保持手机解锁。
2. 打开 Mac 上的 iLoader，选中正确设备。
3. 使用最初安装 SideStore 的同一个 Apple 账号登录。
4. 重新安装 SideStore，优先覆盖现有版本。如果提示必须删除、不能覆盖，先保存完整提示再排查，不要直接删除。
5. 如果手机弹出“不受信任的开发者”，按照下一小节完成信任。
6. 打开 SideStore，连接 Wi-Fi 和 LocalDevVPN。
7. 在 My Apps 点击 Refresh All，分别核对 SideStore 和 WrapPin 的有效期。

官方排查说明支持不先删除原 App、重新侧载以保留数据，但这不是数据保留的绝对保证。遇到错误先排查，不要反复切换账号或撤销证书。

参考：https://docs.sidestore.io/docs/troubleshooting

### 覆盖安装后弹出“不受信任的开发者”

这是本次过期恢复中实际遇到的提示。

1. 关闭弹窗，打开“设置 → 通用 → VPN 与设备管理”。
2. 在“开发者 App”下，点击与本次签名一致的 Apple 账号。
3. 点击“信任”，按系统提示确认；如果要求重启，按提示完成。
4. 如果显示“验证 App”，保持联网并点击验证。
5. 返回桌面，再次打开 SideStore。

信任开发者与开启开发者模式是两个不同步骤。若验证失败，保留详情页提示继续排查，不要把过期、未信任和证书失效都当成同一种问题。

### SideStore 已显示 7 DAYS，WrapPin 仍显示 EXPIRED

这说明 SideStore 自身已获得新的有效期，WrapPin 仍需单独续签。

1. 连接 Wi-Fi 和 LocalDevVPN。
2. 在 SideStore → My Apps 点击 Refresh All，或点击 WrapPin 的 EXPIRED 按钮尝试刷新。
3. 如果出现“pairing file is invalid or missing”或“.rppairing UDID not found”，先修复 SideStore 的配对文件：

   - USB 连接手机和 Mac，保持手机解锁。
   - iLoader 选中手机 → Manage Pairing File → SideStore 旁的 Place。
   - 等待绿色提示“Pairing file placed successfully!”。
   - 从后台关闭 SideStore，再重新打开，重试刷新。

4. 若仍是相同错误，在 iLoader 中 Delete Stored Pairing → Refresh，按提示重新配对或信任，再执行 Manage Pairing File → SideStore → Place。
5. 如仍无效，按第 9 节 B 的官方流程，在 SideStore 重置配对文件后重新生成并放置。
6. 确认 WrapPin 的红色 EXPIRED 变成正常剩余有效期，并实际打开 WrapPin 检查。

这里修复的是 **SideStore 用于签名安装的配对文件**，不等同于 WrapPin 内的“配对本机”。不要因此直接清除 WrapPin 的收藏、设置或本机配对。

本次过期恢复中，截图确认 SideStore 已显示 7 DAYS、WrapPin 仍 EXPIRED，并报“.rppairing UDID not found”；提供上述配对文件修复步骤后，用户反馈“搞定了”。没有记录最终执行的是哪一个分支。

### 更新 WrapPin

下载官方新版本标准版 IPA，在 SideStore 中覆盖安装。按项目说明不要先删除旧 App，以免丢失收藏、历史、设置，并可能需要重新配对。

### 锁屏后会不会跳回真实位置？

存在不确定性。项目已验证前台固定位置和路线等流程，但长时间锁屏保持仍缺少充分测试，不保证稳定。

- 保持 LocalDevVPN 连接，避免切换到其他 VPN。
- 不要从后台划掉 WrapPin。
- 锁屏或切换 App 后，可以用地图重新核对位置。
- 连接中断时，不应假定一定立即恢复真实位置，也不应假定模拟位置一定保持。
- 可以先锁屏 5–10 分钟测试自己的设备，但短测通过不代表长期稳定。

## 9. 本次遇到的报错与处理方法

### A. 登录失败：-80045

典型提示：

```text
Failed to Sign In
This action cannot be completed at this time. (-80045)
```

这表示 Apple 账号登录请求失败。仅凭这个错误不能判断密码错误或账号被封。SideStore 项目中有同类登录兼容性报告。

本次提供以下社区排查步骤后，用户反馈能够登录；未逐项记录手机实际执行了哪些设置，因此不能断言某一个开关就是唯一原因。

1. 关闭登录弹窗，进入 SideStore → Settings → User Customizations。
2. 关闭 On-Device Anisette。
3. 在 Anisette Servers 中选择 SideStore (New)，核对地址为 `https://new.ani.sidestore.io`。
4. 关闭 Enable Auto Rotation / 自动轮换服务器。
5. 使用 Reset adi.pb；如出现 Keep Headers，不勾选。
6. 从后台彻底关闭 SideStore，重新打开。
7. 连接 LocalDevVPN，用安装 SideStore 时的同一个 Apple 账号尝试登录一次。

这些是社区临时方案，不保证所有账号、版本和时段都适用。Reset adi.pb 重置的是登录辅助数据，不是设备配对文件。菜单名称和位置可能随版本改变。

如果新错误变成 HTTP 502，表示 Anisette 服务请求失败，不代表原登录问题已经解决。官方项目讨论还列出 `https://mac-ani.jkcoxson.com` 作为可尝试的替代服务，但可用性需要当时核实。不要无休止反复登录，也不要随意把账号或配对文件交给陌生服务。

参考：https://github.com/SideStore/SideStore/issues/1664

### B. 无法读取 UDID：Developer Portal Error

典型提示：

```text
SideStore could not determine this device's UDID.
Please replace your pairing using iloader.
```

含义：SideStore 无法取得设备标识，需要检查连接以及配对记录。出现这个弹窗，不等于整个登录和设备注册流程已经全部完成。

按 SideStore 官方文档，可重新生成并放置配对文件：

1. 在 SideStore 设置里找到 Reset Pairing File，按提示重置。
2. 用数据线连接 iPhone 和 Mac，解锁手机。
3. 打开 iLoader，确认选中了正确手机。
4. 点击 Delete Stored Pairing，清除旧的配对缓存。
5. 点击 Refresh，按提示重新配对；手机出现信任提示时确认。
6. 在 iLoader 点击 Manage Pairing File。
7. 在 SideStore 旁点击 Place，确认出现绿色的“Pairing file placed successfully!”。
8. 重新打开 SideStore，连接 Wi-Fi 和 LocalDevVPN，再尝试登录或刷新。

本次确实出现过此报错，但没有记录用户最终是否执行上述全部步骤。这是官方排查流程，不是本次已确认执行的操作清单。

配对文件含设备访问所需的信息，请保留在自己的设备上，不要发到群里或附在分享教程中。

参考：

- https://docs.sidestore.io/docs/troubleshooting/error-codes
- https://docs.sidestore.io/docs/advanced/pairing-file

### C. VPN 已连接，但无法刷新：DeviceEndpointNotInitialized

典型提示：

```text
SideStore is unable to reach the device endpoint:
VPN tunnel iface is up but tunnel peer IP is not yet reachable
Cause: DeviceEndpointNotInitialized
Please check your Connection Configuration in Settings.
```

含义：VPN 接口已经启动，但 SideStore 没有成功连接到手机本机服务。显示账号、显示 VPN 图标，都不能单独证明这条连接正常。

按以下顺序排查：

1. 手机连接 Wi-Fi。
2. 在“设置 → App → SideStore”中确认本地网络权限已开启；不同系统版本也可能在“隐私与安全性 → 本地网络”下。
3. 暂停其他正在占用系统 VPN 的代理工具。
4. 断开 LocalDevVPN，从后台彻底关闭 SideStore。
5. 重新打开 LocalDevVPN 并 Connect，再打开 SideStore 刷新一次。
6. 如仍相同，重启手机，解锁后重新连接 LocalDevVPN，再试。
7. 查看 SideStore → Settings → Connection Configuration / Connection Config。
8. 查看 LocalDevVPN 中的 Tunnel IP、Device IP，结合实际配置、版本和服务发现结果继续排查。

**不要照抄别人手机的端口或随意修改 IP。**同类社区讨论中存在多种地址和端口处理方法，并非通用答案。

本次用户随后明确反馈“更新成功”，之后完成了 WrapPin 安装、配对和定位。但没有记录最终修改了哪个连接参数，因此无法提供一组声称已经在本机验证的修复数值。

参考：https://github.com/SideStore/SideStore/issues/1460

### D. Wi-Fi 下可用，关闭 Wi-Fi 后 WrapPin 启动定位失败

先确认报错来自 WrapPin 的“开始模拟定位”，还是 SideStore 的“刷新”。两者的操作要求不同。

- **WrapPin 启动定位**：检查隧道应用是否为 LocalDevVPN，按第 8 节“没有 Wi-Fi，只使用 4G/5G 时怎么启动？”中的流程，临时关闭蜂窝数据建立连接，再按提示恢复。
- **SideStore 刷新或安装**：本文采用的官方安装流程要求 Wi-Fi 和 LocalDevVPN 同时连接。
- **连接后无法上网**：属于另一种网络故障，需要单独检查；不要仅凭“无 Wi-Fi”就认定是同一个问题。

仍失败时，提供 WrapPin 完整报错、连接检测结果，以及 LocalDevVPN 是否已连接。不要发送账号密码或配对文件。

### E. 过期恢复后：.rppairing UDID not found

典型提示：

```text
The current pairing file is invalid or missing.
Reason: .rppairing UDID not found
```

当前配对文件缺失、无效或未提供所需设备标识，不能靠反复点击登录解决。先在 iLoader 为 SideStore 重新 Place 配对文件；如仍失败，再重新生成。完整操作见第 8 节“SideStore 已显示 7 DAYS，WrapPin 仍显示 EXPIRED”。

如果文件放置成功仍出错，保留 SideStore 与 iLoader 的完整版本和错误，不要直接把陌生人的配对文件导入自己的手机。

## 10. 下次出问题时，先确定卡在哪一层

| 现象 | 优先检查 |
|---|---|
| Mac 找不到手机 | USB 数据线、解锁、信任电脑、iLoader 刷新设备列表 |
| 登录报 -80045 | Anisette 配置、SideStore 版本和当时的登录兼容性 |
| 无法确定 UDID | 本机连接、配对文件是否需要重新生成和放置 |
| 不受信任的开发者 | VPN 与设备管理里信任本次签名账号 |
| .rppairing UDID not found | iLoader 为 SideStore Place 配对文件，必要时重新生成 |
| SideStore 过期打不开 | 原账号通过 iLoader 覆盖安装，再处理信任和续签 |
| DeviceEndpointNotInitialized | LocalDevVPN、权限、连接配置和服务发现 |
| SideStore 能登录但仍显示旧天数 | 刷新是否真的完成；登录成功不等于续签成功 |
| 选 IPA 后安装失败 | 标准版还是隧道版、完整报错、免费签名 App 数量及 App ID 限制 |
| WrapPin 已配对但无法定位 | LocalDevVPN、开发者模式、WrapPin 连接诊断 |
| Wi-Fi 下可定位，纯 4G/5G 下启动失败 | 第 8 节蜂窝连接引导：临时关闭蜂窝数据，建立连接后按提示恢复 |
| 地图位置偏移 | 坐标模式、所选坐标、地图缓存 |

向别人求助时提供：系统版本、SideStore 完整版本、LocalDevVPN 版本、发生步骤、完整错误和相关配置截图。截图应遮住账号等个人信息，不要提供密码、验证码或配对文件。

## 11. 成功检查清单

- [ ] SideStore 已登录正确账号。
- [ ] SideStore 自身刷新成功，有效期已延长。
- [ ] 已安装 WrapPin 标准版。
- [ ] WrapPin 显示本机已配对。
- [ ] LocalDevVPN 连接正常。
- [ ] 模拟位置已在 Apple 地图中核实。
- [ ] “停止模拟并恢复”后已核实真实位置。
- [ ] 已记住在免费签名到期前刷新两个 App。

本次已确认模拟定位成功；停止恢复和长时间锁屏保持并未收到独立测试结果，建议使用者自行完成最后的验证。

## 12. 本次经历中已确认与尚未验证的部分

- 已确认：SideStore 首次刷新成功，WrapPin 安装、配对和模拟定位成功。
- 已确认：过期恢复时，SideStore 已显示 7 DAYS；WrapPin 仍 EXPIRED 并出现配对文件错误。提供配对修复流程后，用户反馈恢复成功。
- 未逐项记录：登录 -80045 和本机隧道连接问题最终改动的每一个开关或数值。因此本文提供排查顺序，不把某一组配置声称为通用解法。
- 尚未收到独立成功反馈：纯蜂窝网络连接流程、长期锁屏保持和停止后真实位置恢复测试。相关操作依据官方说明，需在自己的设备上核对。

分享时只需分享此说明书，不必附带聊天截图、Apple 账号、设备 UDID、证书、验证码或配对文件。
