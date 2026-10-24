# 给 Codex 的刷机指令 v2：一加 13T — 先回氧 OS 打底，再刷 LineageOS GSI

把下面整段粘贴给 Codex 即可。（v2 替代旧版：旧版假设的起点是 ColorOS 原厂系统，与本机实际状态不符。）

---

# 任务：一加 13T 国行版 — 转换氧 OS 打底 + 刷入 LineageOS GSI

## 设备与现状
- 设备：一加 13T 国行版（代号 PKX110，别名 pagani），Bootloader **已解锁**
- 本机为备用机：不插 SIM 卡、无需保留任何数据、无银行类 App，不要为这些因素做额外处理
- **当前状态**：底层基底为 ColorOS 17（曾刷砖，经售后恢复为原厂基底）；系统层运行一个来源不明的 pagani「Pixel 17」移植包
- **当前症状**：拨号界面提示「飞行模式」（无线电/电话栈未正常初始化）；系统另有多处 Bug
- **故障假设**：该 Pixel 移植包大概率按氧 OS 基底适配，与 ColorOS 17 基底的 vendor/无线电配置对不上版，导致电话栈异常。本次先换基底验证，再换系统

## 目标
1. 先用 koaaN 转换脚本把整机（含基底）转换到一加 13s（CPH2723）氧 OS，验证「飞行模式」症状是否消失
2. 再在氧 OS 基底上刷入 LineageOS GSI（带 GApps 变体），作为最终第三方系统

## 参考资料（以此为据，不要用其他来源的包）
- 氧 OS 转换教程：DroidWin《How to Convert OnePlus 13T to 13S: ColorOS to OxygenOS》
  https://droidwin.com/how-to-convert-oneplus-13t-to-13s-coloros-to-oxygenos/
- 转换/回滚脚本：上述教程内链接（koaaN 发布，roms.danielspringer.at）
- LineageOS GSI：AndyYan / TrebleDroid 官方发布渠道，选 **arm64、A/B、bgN（含 GApps）** 变体，取最新发布版；报告确切文件名与版本

## 执行原则（必须遵守）
1. **分三阶段执行**：阶段一（只读检查）→ 等我确认 → 阶段二（氧 OS 转换与验证）→ 等我确认 → 阶段三（刷 GSI 与验收）。每个确认点都要停下来等我回复，不要自行推进。
2. 只执行下列步骤。遇到任何报错、设备无响应、版本不匹配，**立即停下并报告原始错误信息**，不要自行寻找替代方案、不要反复重试刷写。
3. **绝对禁止**：
   - 回锁 Bootloader（任何 `fastboot flashing lock` / `fastboot oem lock`）
   - 刷入非本机型（PKX110/pagani）的专机固件；GSI 必须是 arm64 A/B 通用镜像
   - 格式化或擦除下文未列出的分区；不要动基带相关分区（modem 等）的手动刷写，转换脚本自带的不在此限
4. 每执行一步，报告：所用命令 + 关键输出原文。
5. 本机有刷砖史（售后授权 EDL 救回），任何一步拿不准，先停下报告，不要赌。

## 阶段一：只读检查（不写入任何分区）
1. 确认电脑端 adb / fastboot 可用；手机开启 USB 调试并已授权这台电脑。
2. 读取并报告：
   - `fastboot getvar product`、`fastboot getvar unlocked`（应为 yes）
   - 当前系统版本号与基带版本：`adb shell getprop ro.build.display.id`、`adb shell getprop gsm.version.baseband`
3. 下载氧 OS 转换脚本包，重点核查并报告：包的发布日期/版本、说明是否支持**从 ColorOS 17 基底**转换。
   - **停机条件**：本机基底（2026 年的 ColorOS 17）比脚本包（2025 年发布）新，存在固件防回滚拒绝刷入旧分区的可能。若包说明未明确支持当前基底、或版本差距明显，停下报告，不要进入阶段二。
4. 下载 LineageOS GSI（arm64、A/B、bgN 变体），报告文件名、Android 版本与发布日期；同时报告 TrebleDroid 官方说明对本机型/基底的要求。
5. 下载 OxygenOS → ColorOS 回滚脚本包备用（只下载，不执行），报告路径。
6. 报告手机电量（应 > 50%）与连接方式（数据线直连，不经 USB Hub）。

## 阶段二：转换氧 OS 并验证（仅在我确认阶段一结果后执行）
1. 运行转换包中的 `Super_Flasher.bat`，按教程流程执行；FastbootD 界面选 **English**。
2. 脚本询问是否 Root → 选 **N**；询问是否清除数据 → 选 **YES** 并按提示格式化 data。
3. 开机进入氧 OS 后逐项验证并报告（通过/不通过 + 现象）：
   - 关于手机型号显示 CPH2723、系统版本
   - **拨号界面是否还提示「飞行模式」**（本次核心验证项）
   - Wi-Fi 可连接上网、蓝牙可开关并扫描到设备、相机可拍照
4. **停机条件**：若氧 OS 下拨号仍提示飞行模式，停下报告，不要进入阶段三——说明问题不在基底，需要重新诊断。
5. 关闭系统自动 OTA 更新，报告已关闭。

## 阶段三：刷入 LineageOS GSI（仅在我确认阶段二结果后执行）
1. 按 TrebleDroid/AndyYan 官方 GSI 安装说明执行，通用流程为：
   - 进入 FastbootD，处理 vbmeta（`--disable-verity --disable-verification`，以官方说明为准）
   - `fastboot flash system <GSI镜像文件>`
   - 若因 super 分区空间不足失败，按官方说明删除逻辑分区（product / system_ext）后重试一次；仍失败则停下报告
   - `fastboot -w` 清除数据后重启
2. 不要在此阶段刷入任何 Root 方案或额外模块。

## 阶段四：验收报告
逐项检查并报告（通过/不通过 + 具体现象）：
1. 正常开机、系统版本与 GSI 构建号
2. 拨号界面：飞行模式提示是否消失
3. Wi-Fi、蓝牙、定位、NFC 开关与基本功能
4. 相机拍照、屏下指纹录入与解锁、自动亮度、屏幕刷新率
5. Google Play 商店可打开并登录谷歌账号
6. 待机 10 分钟后有无异常发热或反复重启

---

## 附：背景说明（供 Codex 理解，不要据此改动步骤）
- 症状定位思路：先换基底（氧 OS）再换系统（GSI），目的是把「基底不匹配」与「ROM 本身 Bug」两个变量分开，任何一步出问题都可归因。
- 氧 OS 转换脚本会回刷 ColorOS 的 oplusstanvbk.img、odm.img 与 Find X8 Pro 的 my_region.img，这是教程既定步骤，不要省略。
