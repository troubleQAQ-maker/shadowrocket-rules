# 更新记录

## [1.1.0] - 2026-10-25

- 新增 Mihomo 版配置 `mihomo/mihomo-lite.yaml`：由 Shadowrocket 精简白名单逐条转换（755 条域名规则、24 条 IP-CIDR、2 条关键词，域名集合与源文件完全一致），银行直连 4 条置顶
- 新增三件套远程规则集：blackmatrix7 广告拦截（REJECT）、MetaCubeX 海外 AI 服务集（category-ai-!cn，强制代理）、MetaCubeX 中国域名集（geosite cn，直连兜底）；Apple News 改用上游 Clash 格式版
- 转换差异记录：IP-CIDR 补 `no-resolve`；原 `[URL Rewrite]` / `[MITM]` 段因 Mihomo 无对应机制去除；配置文件不含节点信息，`PROXY` 组以 `include-all` 自动纳入用户节点

## [1.0.0] - 2026-10-24

本材料包首次发布，并入既有仓库 shadowrocket-rules（仓库内原有的 `shadowrocket.conf` 主配置与说明保持不变，README 已合并）。

- 新增 Shadowrocket 精简白名单配置（875 行）：Johnshall 无广告白名单（2026-10-19 构建）+ 香港银行 / 券商直连规则置顶；不含任何节点与订阅信息
- 新增一加 13T（PKX110）分阶段刷机指令 v2：氧 OS 转换打底 + LineageOS GSI，含分阶段确认点与停机条件
- 新增 README、版本说明与 LICENSE，并注明上游署名与来源
