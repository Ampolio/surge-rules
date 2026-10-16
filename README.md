# Claude 域名规则 · Surge

用于将 Claude / Anthropic 相关域名分流到你选择的代理节点或策略组。核对日期：**2026-10-16（Asia/Singapore）**。

## 直接导入

推荐订阅地址：

```text
https://raw.githubusercontent.com/Ampolio/surge-rules/main/Claude.list
```

在 Surge 当前配置的 **`[Rule]` 段**加入下面这一行；如果已有 `[Rule]`，不要再创建重复段落：

```ini
RULE-SET,https://raw.githubusercontent.com/Ampolio/surge-rules/main/Claude.list,Claude,update-interval=86400
```

**把最后的 `Claude` 替换为你已有的代理节点或策略组名称。** 例如，已有策略组名为 `AI`，则写成：

```ini
RULE-SET,https://raw.githubusercontent.com/Ampolio/surge-rules/main/Claude.list,AI,update-interval=86400
```

将这一行放在可能先匹配的通用规则之前，并放在 `GEOIP`、`FINAL` 等兜底规则之前。Surge 按顺序匹配，先匹配的规则决定策略。

也可以在 Surge 的规则编辑界面添加远程规则集（`RULE-SET`），填入订阅地址并选择目标策略。界面名称可能随版本不同。

## 主规则包含什么

主规则共 **9 条**：8 条后缀规则和 1 条精确主机规则。后缀规则会同时覆盖根域名和全部子域名。

| 域名 | 匹配 | 收录依据与范围 |
| --- | --- | --- |
| `anthropic.com` | 根域及子域 | 官方网络文档；覆盖 `api`、`mcp-proxy` 等子域 |
| `claude.ai` | 根域及子域 | 官方网络文档；覆盖网页、账号认证、`downloads` 等子域 |
| `claude.com` | 根域及子域 | 官方网络文档；覆盖登录跳转、`platform`、`code`、帮助文档等子域 |
| `claudeusercontent.com` | 根域及子域 | 官方网络文档列出 `bridge` 与 `*.frame`；后缀规则兼顾内容子域 |
| `claude.dev` | 根域及子域 | 官方 Cookie 清单列为第一方域名；不代表聊天必需 |
| `clau.de` | 根域及子域 | 社区规则兼容补充 |
| `claudemcpclient.com` | 根域及子域 | 社区规则兼容补充，未据官方资料确认具体功能或必要性 |
| `claudemcpcontent.com` | 根域及子域 | 社区规则兼容补充，未据官方资料确认具体功能或必要性 |
| `servd-anthropic-website.b-cdn.net` | 仅此主机 | 社区收录的 CDN 主机；不扩大匹配至整个 `b-cdn.net` |

这些是分流范围，不代表每个域名都会在你的客户端中出现，也不是官方完整白名单。来源与证据边界见 [SOURCES.md](SOURCES.md)。

## 可选：共享服务与遥测

下面两份文件不包含主规则。仅在相应功能确有需要时添加，并替换策略名：

```ini
# Claude Code 安装、插件、更新和发布说明等共享服务（可选）
RULE-SET,https://raw.githubusercontent.com/Ampolio/surge-rules/main/Claude-Shared-Services.list,Claude,update-interval=86400

# Claude Code 可选遥测（可选）
RULE-SET,https://raw.githubusercontent.com/Ampolio/surge-rules/main/Claude-Telemetry.list,Claude,update-interval=86400
```

共享服务文件精确匹配 `storage.googleapis.com`、`registry.npmjs.org`、`github.com`、`raw.githubusercontent.com`、`formulae.brew.sh`；遥测文件精确匹配官方文档中的两个 Datadog 接收主机。

**这些是多应用共用的主机。启用后，其他应用访问同一主机的请求也会走该策略。** GitHub 插件的完整下载流程还可能访问其他资源主机；这些列表不保证覆盖任意插件、MCP 服务、外部登录提供商、云模型提供商或第三方网站。

主规则没有泛匹配整个 `googleapis.com`、`githubusercontent.com`、`cloudflare.com`、`auth0.com`、`sentry.io` 或 `datadoghq.com`。使用 Google 登录、特定连接器或云平台时，应按实际请求另外分流。

## 可选：DOMAIN-SET 格式

如果偏好纯域名文件，可用等价的 `Claude.domainset`：

```ini
DOMAIN-SET,https://raw.githubusercontent.com/Ampolio/surge-rules/main/Claude.domainset,Claude,update-interval=86400
```

主规则的两种格式选一种即可。不要把 `.domainset` 文件作为 `RULE-SET` 导入，也不要把 `.list` 文件作为 `DOMAIN-SET` 导入。

## 验证与维护

发布前检查规则语法、重复项、两种格式的一致性、已知官方端点覆盖和域名匹配边界；这不等于已在每个 Surge 版本中完成真实登录、对话或下载测试。

导入后，在 Surge 请求记录中检查 `claude.ai`、`api.anthropic.com`、`platform.claude.com`、`bridge.claudeusercontent.com` 等实际请求是否命中本规则及预期策略。未安装或未使用的功能不一定产生这些请求。

`update-interval=86400` 让 Surge 每 24 小时重新拉取仓库文件；**仓库内容需要人工核对更新，目前没有自动抓取域名的任务。** 本规则只决定网络分流，服务是否可用还取决于节点、账号、组织配置和服务端状态。
