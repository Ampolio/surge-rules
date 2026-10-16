# Claude 域名规则 · Surge

将 Claude / Anthropic 相关请求分流到你选择的策略。核对日期：**2026-10-16（Asia/Singapore）**。

## 选择订阅

| 文件 | 条数 | 范围 |
| --- | ---: | --- |
| [Claude.list](Claude.list) | 10 | 主规则，包含桌面端 `claude.app` 和专用 CDN |
| [Claude-Extended.list](Claude-Extended.list) | 26 | 主规则 + Artifacts CDN/字体 + Code 共享服务 + 遥测 + Gerrit 模式 |
| [Claude.domainset](Claude.domainset) | 10 | 与主规则等价的 DOMAIN-SET 格式 |

**希望一条订阅覆盖本次核对的官方网络端点及社区补充项，可选扩展规则。** 共享主机也供其他应用使用，它们访问相同主机时也会走所选策略。扩展版包含一条 `DOMAIN-WILDCARD`，需要 Surge 支持该类型。主规则与扩展规则选一份即可。

## 导入

将下面一行加入当前配置已有的 **`[Rule]` 段**：

```ini
RULE-SET,https://raw.githubusercontent.com/Ampolio/surge-rules/main/Claude-Extended.list,Claude,update-interval=86400
```

**将策略名 `Claude` 替换为你已有的代理节点或策略组名**，如 `AI` 或 `Proxy`。将这行放在可能先匹配的通用规则、`GEOIP`、`FINAL` 之前。

只需要主规则，或版本不支持 `DOMAIN-WILDCARD`，则使用：

```ini
RULE-SET,https://raw.githubusercontent.com/Ampolio/surge-rules/main/Claude.list,Claude,update-interval=86400
```

也可在 Surge 中添加远程 RULE-SET，填入相应 Raw 地址并选择策略。已有主规则订阅地址保持可用；手动刷新远程规则可立即拉取新版。

## 主规则覆盖

共 **9 条后缀规则 + 1 条精确主机规则**。后缀规则覆盖根域及全部子域，包括动态子域。

| 域名 | 来源与范围 |
| --- | --- |
| `anthropic.com` | 官方网络文档；API、应用资源、连接器代理等 |
| `claude.ai` | 官方网络文档；网页、下载、预览等 |
| `claude.app` | 官方桌面网络文档；动态预览子域 |
| `claude.com` | 官方网络文档；登录、Console、文档等 |
| `claudeusercontent.com` | 官方网络文档；内容、Artifacts、浏览器桥接 |
| `claudemcpcontent.com` | 官方网络文档；MCP Apps 交互组件 |
| `claude.dev` | 官方 Cookie 清单中的第一方域名，不代表聊天必需 |
| `clau.de` | 社区兼容补充 |
| `claudemcpclient.com` | 社区兼容补充，未据官方文档确认具体用途 |
| `servd-anthropic-website.b-cdn.net` | 社区兼容补充，仅精确匹配该主机 |

## 按功能单独添加

以下文件不含主规则；使用扩展规则时无需重复添加。

| 文件 | 条数 | 内容 |
| --- | ---: | --- |
| [Claude-Artifacts.list](Claude-Artifacts.list) | 7 | 官方列出的字体与 JavaScript 库 CDN |
| [Claude-Shared-Services.list](Claude-Shared-Services.list) | 5 | Code 安装、插件、发布说明和 Homebrew 检查 |
| [Claude-Telemetry.list](Claude-Telemetry.list) | 3 | 两个官方遥测主机及一个社区收录的分析主机 |
| [Claude-Developer-Extras.list](Claude-Developer-Extras.list) | 1 | 官方列出的 Gerrit 模式，仅相关开发场景需要 |

例如：

```ini
RULE-SET,https://raw.githubusercontent.com/Ampolio/surge-rules/main/Claude-Artifacts.list,Claude,update-interval=86400
```

共享主机不都是聊天必需；分流规则也不会主动开启客户端中关闭的遥测。

仅主规则提供等价纯域名格式：

```ini
DOMAIN-SET,https://raw.githubusercontent.com/Ampolio/surge-rules/main/Claude.domainset,Claude,update-interval=86400
```

不要把 `.domainset` 当作 RULE-SET，或把 `.list` 当作 DOMAIN-SET。扩展版包含通配模式，不提供 DOMAIN-SET 格式。

## 验证与维护

逐项核对官方 CLI、桌面网络要求与 Artifacts 外部依赖，并对照四个社区项目。详见 [AUDIT.md](AUDIT.md)、[SOURCES.md](SOURCES.md) 和 [覆盖清单](audit/coverage.json)。

```sh
python3 scripts/build_rules.py --check
```

检查格式、重复与重叠、主规则两种格式一致性、扩展版等于组件并集、来源模式覆盖和相似域名边界。检查基于人工审阅后记录的来源快照，不自动抓取官网，也不是 Surge 原生解析或实际登录测试。

`update-interval=86400` 表示每 24 小时拉取仓库文件；名单仍需人工核对。导入后可在 Surge 请求记录中确认实际请求命中预期策略。

固定名单不能穷尽任意第三方登录、企业 SSO、支付、插件、MCP 服务器、外部网站与自定义网关，也不能预知版本新增端点；这些场景应按实际请求补充。规则只决定分流，不保证节点、账号和服务端状态。
