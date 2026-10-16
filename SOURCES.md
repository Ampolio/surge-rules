# 来源与收录边界

核对日期：2026-10-16（Asia/Singapore）。

## 官方资料

- [Claude Code：Enterprise network configuration](https://code.claude.com/docs/en/network-config#network-access-requirements)：核对 API、登录、连接器代理、下载、内容与浏览器桥接端点，以及按功能需要的共享服务和可选遥测。主规则用四条根域后缀规则覆盖该文档中的 Claude / Anthropic 专属主机；将精确主机扩大为根域后缀是本项目的维护选择，并非官方防火墙白名单的逐条复制。
- [Anthropic：What Cookies Does Anthropic Use?](https://privacy.claude.com/en/articles/10023541-what-cookies-does-anthropic-use)：列出 `claude.dev` 为第一方 Cookie 域名，支持其关联性；不证明它是 Claude 聊天必需端点。
- [Surge：Rule Sets](https://manual.nssurge.com/rules/ruleset.html)：外部规则集使用不含策略名的规则行；由导入行指定统一策略；支持 `update-interval`。
- [Surge：Domain Rules](https://manual.nssurge.com/rules/domain.html)：`DOMAIN` 精确匹配；`DOMAIN-SUFFIX` 匹配根域及子域；`DOMAIN-SET` 中前导点表示后缀匹配。

## 社区兼容补充

- [v2fly/domain-list-community：anthropic](https://github.com/v2fly/domain-list-community/blob/master/data/anthropic)（[原始文件](https://raw.githubusercontent.com/v2fly/domain-list-community/master/data/anthropic)）：直接查看上游维护的域名名单，补充 `clau.de`、`claudemcpclient.com`、`claudemcpcontent.com` 和精确 CDN 主机 `servd-anthropic-website.b-cdn.net`。这只能证明上游将其归入 Anthropic 分类；本次未从官方资料验证这四项的具体用途或必要性。上游文件会变化，核对日期对应本次查看的内容。

## 未作出的承诺

- 不将域名归类、DNS 可解析或官网提及等同于某客户端必然访问该域名。
- 不保证穷尽全部客户端版本、地区、认证提供商、支付、第三方连接器、插件或云平台端点。
- 不使用固定 IP、IP 段或域名关键词猜测补全；CDN 与服务地址可能变化。
- 不因域名在历史 Artifact 链接中出现，就直接纳入当前主规则。例如本次未将 `claude.site` 纳入主规则。
- 分流规则不能保证账号状态、服务端地区判断或可用性，也不改变云端连接器的网络出口。
