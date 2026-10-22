# Shadowrocket 分流配置

自用分流配置，基于 LOWERTOP/Shadowrocket 的 lazy_group.conf 定制。

## 文件

- `shadowrocket.conf`：2026-10-20 v2 DNS 调整版，保留香港金融服务直连规则和策略组。
- 不包含代理节点、节点订阅、账号凭证或 MITM 证书私钥。

## 使用

在 GitHub 打开 `shadowrocket.conf`，点击 Raw，复制原始文件链接。在 Shadowrocket 的「配置」中添加该链接，下载后选中配置；首页「全局路由」选择「配置」。节点或订阅需要另行添加。

本文件是 Shadowrocket 格式，不能直接作为 NekoBox / sing-box 配置导入。

## 说明

通用及备用 DNS 经代理，直连服务使用国内加密 DNS；节点域名使用独立 DNS。保留 IPv6、私有 IP 应答，禁用显式系统 DNS 回退。实际解析及连接行为仍需结合网络、节点和客户端版本检查。

此仓库是当前配置的快照，不会自动合并上游变更。引用的远程规则集由各自维护者更新。域名规则不能保证覆盖整个 App；支付、银行服务和 DNS 隐私表现需实际验证。

## 来源

- 基础配置：https://github.com/LOWERTOP/Shadowrocket
- 基础文件：https://lowertop.github.io/Shadowrocket/lazy_group.conf
- 远程规则来源保留在配置文件中，权利归各原作者。