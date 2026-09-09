# Surge Rules

个人维护的 Surge 服务分流与隐私拦截规则。

## 规则集

| 文件 | 用途 | 建议策略 |
| --- | --- | --- |
| `services/modio.list` | mod.io API 与 CDN | `🎮Steam` |
| `services/nexusmods.list` | Nexus Mods 与文件 CDN | `🎮Steam` |
| `reject/ads-telemetry.list` | 已从实际请求确认的广告、追踪和遥测 | `REJECT` |
| `reject/noisy-telemetry.list` | 会高频重试的非必要遥测 | `REJECT-DROP` |
| `reject/optional-telemetry.list` | 可能影响可选功能的遥测，不默认启用 | `REJECT` |

## Surge 配置

```ini
RULE-SET,https://raw.githubusercontent.com/wangkezun/surge-rules/main/reject/noisy-telemetry.list,REJECT-DROP,extended-matching
RULE-SET,https://raw.githubusercontent.com/wangkezun/surge-rules/main/reject/ads-telemetry.list,REJECT,extended-matching
RULE-SET,https://raw.githubusercontent.com/wangkezun/surge-rules/main/services/modio.list,🎮Steam
RULE-SET,https://raw.githubusercontent.com/wangkezun/surge-rules/main/services/nexusmods.list,🎮Steam
```

拦截规则应放在各类服务规则、国内直连、China IP 和 `FINAL` 之前。服务分流规则应放在通用 CDN、下载、国内/全球兜底和 `FINAL` 之前。

## 维护原则

- 只提交域名规则，不提交请求日志、设备名、IP、URL 参数或其他私人数据。
- 新域名必须先确认服务归属和用途，再进入规则集。
- 纯广告、追踪与非必要遥测进入 `ads-telemetry.list`。
- 被拦截后会产生高频重试的遥测进入 `noisy-telemetry.list`，并使用 `REJECT-DROP,extended-matching`。
- 可能影响同步、诊断、奖励或产品可选功能的项目进入 `optional-telemetry.list`，不自动订阅。
- 优先使用精确 `DOMAIN`，只有确认整个注册域用途一致时才使用 `DOMAIN-SUFFIX`。
