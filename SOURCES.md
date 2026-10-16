# 来源与收录边界

核对日期：2026-10-16（Asia/Singapore）。

## 官方资料

- [Claude Code 网络要求](https://code.claude.com/docs/en/network-config#network-access-requirements)：逐项核对 CLI 表格、桌面与 MCP Apps 说明、Artifacts 外部依赖。
- [Claude Desktop 网络要求](https://code.claude.com/docs/en/desktop#network-access-requirements)：补充 `claude.app`，确认动态预览及内容子域；`claudemcpcontent.com` 已由官方确认用于交互组件。
- [Anthropic Cookie 清单](https://privacy.claude.com/en/articles/10023541-what-cookies-does-anthropic-use)：确认 `claude.dev` 的第一方关联，不证明聊天必需。
- [Windows 部署](https://support.claude.com/en/articles/12622703-deploy-claude-desktop-for-windows)与 [Cowork 架构](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview)：补充检查部署与云端执行的网络边界。
- [Surge 规则集](https://manual.nssurge.com/rules/ruleset.html)与 [域名规则](https://manual.nssurge.com/rules/domain.html)：核对匹配语义及文件格式。

后缀规则比官方精确主机清单覆盖更广，是本项目的维护选择；本项目不是企业防火墙最小权限白名单。

## 社区交叉核对

直接查看以下实际文件。部分项目复用上游，因此四个项目不等于四份独立抓包证据。

- [v2fly：anthropic](https://raw.githubusercontent.com/v2fly/domain-list-community/master/data/anthropic)
- [MetaCubeX：anthropic](https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/anthropic.list)
- [blackmatrix7：Claude](https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Surge/Claude/Claude.list) 与 [Anthropic](https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Surge/Anthropic/Anthropic.list)
- [ACL4SSR：Claude](https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Ruleset/Claude.list)

社区特有补充为 `clau.de`、`claudemcpclient.com`、专用 CDN 和 `cdn.usefathom.com`；最后一项归入可选分析服务。收录只说明维护者将其归类为相关域名，不证明当前客户端一定访问。

## 排除与限制

- `claude.site`：历史 Artifact 链接中出现过，但本次审阅的当前网络要求及上述名单均未确认现时必要性，暂不收录。
- `anthropic.auth0.com`、`challenges.cloudflare.com`、泛匹配 Sentry/Statsig：本次证据不足以统一标为当前必需；若特定认证流程实测需要，应按精确主机补充。
- 官方 [开发容器防火墙示例](https://raw.githubusercontent.com/anthropics/claude-code/main/.devcontainer/init-firewall.sh)还列出 VS Code、Sentry/Statsig 等；它是特定示例环境，未机械合并为全部客户端的现时要求。
- 不泛匹配整片 Google/AWS/Cloudflare CDN。模型平台、组织 SSO、网关、支付及任意插件/MCP 目标按实际地址配置。
- 未进行已登录客户端抓包或全部平台实测，不宣称永久无遗漏。

逐项清单见 [audit/coverage.json](audit/coverage.json)，审阅结论见 [AUDIT.md](AUDIT.md)。
