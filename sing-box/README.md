# macOS VLESS 内置规则版

文件：[sing-box-mac-vless-inline.json](sing-box-mac-vless-inline.json)。基于 sing-box 1.14.4 整理并通过 `sing-box check`；旧版客户端不保证兼容。

## 行为

- 国内域名、国内 IP、局域网直连；其余走 VLESS，广告域名在 DNS 与路由层拦截。
- 单节点手动选择，不启动 urltest 自动测速。
- 国内 DNS 为阿里 DoH 直连，国外为 Cloudflare DoH 经代理；`.lan` / `.local` 交给本地解析器。
- TUN 使用 system 栈；HTTP/SOCKS 入口仅监听 `127.0.0.1:17890`。
- 三个规则集直接内置，启动无需从 GitHub 下载。快照不会自动更新，节点也不会自动订阅更新。

## 使用

1. 下载 JSON 到本地。将 `vless-node` 的服务器地址、端口、UUID、SNI、Reality 公钥、short_id 替换为自己的节点参数。示例地址 `192.0.2.1`、全零 UUID 与示例公钥不能联网；Vision/Reality 参数须与服务端一致。
2. 执行 `sing-box check -c sing-box-mac-vless-inline.json` 检查，或在 SFM 中从文件导入。
3. 关闭其他代理客户端的 TUN 与系统代理，再手动连接 SFM。不要同时开启多个 TUN。

## 验证范围

对应私人配置在作者 Mac 上完成连接，百度直连、Cloudflare 与维基百科经代理访问成功；Google 曾出现握手超时，未保证所有网站可用。公开版本已将节点替换为占位符，不能直接复现联网测试。

与旧模板相比，本版使用当前 DNS 类型与路由 action 写法，移除旧的 block/dns 出站、入站 sniff 字段与自动测速，避免首次启动依赖远程规则下载。没有加入香港银行等额外直连规则。

## 内置规则来源

快照日期：2026-10-29。由 sing-box 1.14.4 `rule-set decompile` 从以下官方二进制规则转换，来源内容仍遵循上游授权：

- https://raw.githubusercontent.com/SagerNet/sing-geosite/rule-set/geosite-cn.srs
- https://raw.githubusercontent.com/SagerNet/sing-geosite/rule-set/geosite-category-ads-all.srs
- https://raw.githubusercontent.com/SagerNet/sing-geoip/rule-set/geoip-cn.srs

精确二进制校验值见 [rules-sources.json](rules-sources.json)。

内置规则转换自 SagerNet 项目，按 GPL-3.0-or-later 保留授权；转换形式为二进制规则反编译后嵌入 JSON，未修改规则内容。许可证通知见 LICENSE-sing-geosite 与 LICENSE-sing-geoip；对应可编辑规则就在配置的 route.rule_set[].rules 中。完整许可文本：https://www.gnu.org/licenses/gpl-3.0.txt 。
