# Shadowrocket 分流配置 & 个人工具箱

自用 Shadowrocket 分流配置的公开快照（基于 LOWERTOP/Shadowrocket 的 lazy_group.conf 定制），外加一套精简白名单规则和一份一加 13T 刷机指令。方便换机、分享与长期维护。

## 内容一览

```
.
├── shadowrocket.conf                      # 主配置：LOWERTOP lazy_group 定制版
├── shadowrocket/
│   └── sr_whitelist_lite_banks.conf       # 精简白名单变体（875 行）
├── mihomo/
│   └── mihomo-lite.yaml                   # 上款的 Mihomo 转换版 + 三件套增强规则集
├── oneplus-13t/
│   └── codex-instructions.md              # 一加 13T 分阶段刷机指令（给 AI 编程助手执行）
├── VERSION.md                             # 版本说明与已知限制
├── CHANGELOG.md                           # 更新记录
└── LICENSE
```

## 主配置：shadowrocket.conf

- `shadowrocket.conf`：2026-10-20 v2 DNS 调整版，保留香港金融服务直连规则和策略组。
- 不包含代理节点、节点订阅、账号凭证或 MITM 证书私钥。

### 使用

在 GitHub 打开 `shadowrocket.conf`，点击 Raw，复制原始文件链接。在 Shadowrocket 的「配置」中添加该链接，下载后选中配置；首页「全局路由」选择「配置」。节点或订阅需要另行添加。

本文件是 Shadowrocket 格式，不能直接作为 NekoBox / sing-box 配置导入。

### 说明

通用及备用 DNS 经代理，直连服务使用国内加密 DNS；节点域名使用独立 DNS。保留 IPv6、私有 IP 应答，禁用显式系统 DNS 回退。实际解析及连接行为仍需结合网络、节点和客户端版本检查。

此仓库是当前配置的快照，不会自动合并上游变更。引用的远程规则集由各自维护者更新。域名规则不能保证覆盖整个 App；支付、银行服务和 DNS 隐私表现需实际验证。

### 来源

- 基础配置：https://github.com/LOWERTOP/Shadowrocket
- 基础文件：https://lowertop.github.io/Shadowrocket/lazy_group.conf
- 远程规则来源保留在配置文件中，权利归各原作者。

## 精简白名单：shadowrocket/sr_whitelist_lite_banks.conf

另一个配置变体：基于 Johnshall 的白名单规则（无广告版）精简拼装，置顶加入香港银行 / 券商直连规则。原始带广告版规则有 6 万多行，此版本只保留白名单本体 + 银行直连段，共 875 行。

- `[General]`：DNS 使用阿里 / 腾讯公共 DoH，局域网与内网地址绕过
- `[Rule]`：顶部 4 条香港金融机构直连（众安 za.group、汇丰 hsbc.com.hk、蚂蚁 antbank.hk、腾达 gigamoney.com.hk），其后为白名单本体（可直连的境外站与中国网站直连，其余默认走代理 `FINAL,PROXY`）
- `[URL Rewrite]` / `[MITM]`：google.cn 跳转（沿用自上游）
- 一条远程规则集：Apple News 走代理，运行时从 blackmatrix7 的仓库拉取（第三方链接，若失效可直接删除该行）

**不包含：** 本文件**不含任何节点、服务器地址、订阅链接或密码**，也没有 `[Proxy]` / 策略组定义 —— 导入后需填入自己的节点，并确认 `PROXY` 策略指向自己的节点或策略组。导入方法：下载 `.conf` → Shadowrocket →「配置」→ 从文件导入（或用本仓库 raw 链接作为远程配置订阅）。

## Mihomo 版：mihomo/

`mihomo/mihomo-lite.yaml` 是上面精简白名单的 Mihomo（Clash Meta 内核）转换版，适用于 Clash Verge Rev、Mihomo Party 等桌面客户端，与 Shadowrocket 版同源同序：

- 银行 4 条直连同样钉在最前；IP-CIDR 规则统一补 `no-resolve`（Mihomo 惯例）
- 额外挂了三件套**远程**规则集（自动更新，不内嵌）：blackmatrix7 广告拦截（REJECT）、MetaCubeX 海外 AI 服务集（强制走代理）、MetaCubeX 中国域名集（直连兜底）；原配置的 Apple News 规则集改用同一上游的 Clash 格式版
- 原 Shadowrocket 配置的 `[URL Rewrite]` / `[MITM]` 段在 Mihomo 无对应机制，已去除
- 文件不含节点信息（`proxies` 为空）：导入后先添加自己的节点，`PROXY` 策略组已设 `include-all`，会自动纳入全部节点

## 一加 13T 刷机指令：oneplus-13t/

一份给 AI 编程助手（Codex）执行的**分阶段**刷机指令 v2：先用 koaaN 脚本把一加 13T 国行版（PKX110）转换到一加 13s 氧 OS 打底并验证，再刷入 LineageOS GSI（bgN 带 GApps 变体）。全程设有三个确认点和明确的停机条件，禁止回锁 Bootloader。

⚠️ **先读完再动手：**

- 仅适用于 **一加 13T 国行版（PKX110 / pagani）且 Bootloader 已解锁** 的机器；指令假设的起点状态是「ColorOS 17 基底 + 来源不明的移植包、拨号提示飞行模式」，起点不同，结论可能不适用
- 刷机会**清除全部数据**，且存在**变砖**风险（作者这台就刷砖后经售后救回过）；保修、信号、相机修复等问题请自行评估
- 这不是一键脚本：每个阶段都要人工确认后才推进，遇到报错先停下，不要赌

## 版本与更新

各组件版本见 [VERSION.md](VERSION.md)，历次变更见 [CHANGELOG.md](CHANGELOG.md)。本仓库为个人维护的快照，不承诺更新频率。

## 署名与来源

不冒充原创，各部分出处如下：

- 主配置基于 **LOWERTOP** 的 [Shadowrocket](https://github.com/LOWERTOP/Shadowrocket) lazy_group.conf 定制（详见上文「来源」）
- 精简白名单核心来自 **Johnshall** 的 [Shadowrocket-ADBlock-Rules-Forever](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever)（作者 Moshel 与 Johnshall），使用的是其无广告白名单（2026-10-19 构建）
- Apple News 远程规则集来自 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)
- 刷机流程参考 DroidWin 的 OnePlus 13T → 13S 转换教程、koaaN 发布的转换脚本，以及 AndyYan / TrebleDroid 的 LineageOS GSI 发布

本仓库的增量工作是：配置定制、精简拼装、香港银行直连段与刷机指令的整理。 上游内容仍归其原作者所有，遵循其各自的授权与署名要求。

## 免责声明

本仓库内容按「现状」提供，仅供学习与个人使用，不做任何明示或暗示的保证。因使用本仓库内容（包括但不限于刷机）造成的设备损坏、数据丢失或其他损失，由使用者自行承担。
