# /intelligence：Microsoft SharePoint / AD FS 7 月在野漏洞簇

| | |
|--|--|
| 类型 | 网络安全情报 |
| 窗口 | 约 2026-04～07 · 主峰 2026-07 · 截至 2026-07-25 |
| 模式 | security · 事实信心 高 · 舆论信心 中 |
| 信源 | CISA Alert/KEV、MSRC、THN/HelpNetSecurity 等二手 |
| skill | raystyle/intelligence ≥ v1.0.2（不交付 payload；可做 PoC/处置信度） |

## 结论（先读）

1. 2026 年 **本地（on-prem）SharePoint Server** 连续多枚漏洞进入 **CISA KEV**，并伴随 **7 月 Patch Tuesday** 前后的在野利用；同周期 **AD FS 提权 CVE-2026-56155** 被微软标为在野。
2. 当前应优先视为**一组**，不要只盯单 CVE：RCE（含反序列化）→ 窃 **IIS machine key** → 持久化/落毒是公开叙事主线。
3. **有本地 SharePoint 或 AD FS：立刻补 7 月相关更新 + CISA 加固；补丁后轮换 machine key（先做入侵排查）**。纯 M365 云 SharePoint、无本地 Server 不在本簇主焦点。
4. 本机 mac 桌面默认**不直接暴露**该攻击面。

## 技术要点（编号簇 · 不完全列表）

| CVE | 组件 | 类型（公开） | KEV / 在野时间线（公开） | 备注 |
|-----|------|--------------|-------------------------|------|
| CVE-2026-32201 | SharePoint | 不当输入验证等 | KEV 约 **2026-04-14** | 较早入 KEV，CISA 7 月加固文仍并提 |
| CVE-2026-45659 | SharePoint | 反序列化 | KEV **2026-07-01**（联邦 due 约 07-04） | 媒体称 5 月已有 OOB 补丁后仍被利用 |
| CVE-2026-56164 | SharePoint | 提权 | KEV **2026-07-14** | 7 月 Patch Tuesday 波次 |
| CVE-2026-58644 | SharePoint | 反序列化 RCE · CVSS 约 **9.8** | KEV **2026-07-16**；**补丁前在野** | CISA：未授权网络侧代码执行叙事；媒体称 Site Owner 等前提有争议表述 |
| CVE-2026-50522 | SharePoint | 反序列化 RCE · CVSS 约 **9.8** | KEV **2026-07-22**（联邦 due 约 07-25） | **公开 PoC 后**多处观测在野；watchTowr 等称窃 machine key |
| CVE-2026-56155 | **AD FS** | 本地提权 | 微软 7 月标在野 | 需已有立足点再提权的常见前提；身份 tier-0 |

**影响版本（SharePoint 侧公开）：** 全部受支持本地版本 — Subscription Edition / 2019 / 2016 等。  
**机理层级（不附 payload）：** 不可信数据反序列化 / 会话令牌处理路径（如公开提到的 `SessionSecurityTokenHandler` 相关描述）→ 服务账户上下文 RCE → 抽 IIS machine key 做长期访问。

## 利用与 PoC 信度

| 项 | 判断 |
|----|------|
| 在野 | **高置信**。CISA KEV 多 CVE + 微软确认在野（含 58644 补丁前、56155 AD FS）；非单一自媒体。 |
| PoC | **CVE-2026-50522：公开 PoC 已出现**，且报道称 PoC 后数小时内蜜罐见利用（watchTowr 等）。其它编号以 KEV/厂商为准，未逐仓库审计。 |
| 真伪/水分 | 在野叙事**水分低**。注意：个别媒体对「未认证 vs Site Owner」前提表述不一，以 **MSRC 原文**为准；勿把旧 ToolShell/历史 SharePoint 洞 PoC 改名冒充本簇。 |
| 本文边界 | 不交付可复制 exploit / payload；只写状态与处置信度 |

## 影响面

| 谁 | 优先级 |
|----|--------|
| 互联网或半暴露的本地 SharePoint | 最高 |
| 内网 SharePoint + 可横向 | 高 |
| AD FS（SSO/联邦身份） | 高（身份面） |
| 仅 M365 云 SharePoint、无本地 Server | 低～观察（仍跟云侧通告） |
| 无上述组件的端点/开发机 | 基本无关 |

## 处置建议（可执行）

1. 安装微软 **2026 年 7 月**及此前 OOB 相关 SharePoint / AD FS 安全更新，验证补丁成功。
2. 按 [CISA SharePoint 加固 Alert](https://www.cisa.gov/news-events/alerts/2026/07/14/cisa-urges-sharepoint-hardening-after-new-exploitations)：监控异常、AMSI、**排查入侵痕迹后再轮换 IIS machine key**、加强日志、避免 SharePoint 直接对公网、限制 Central Admin。
3. AD FS 当 **tier-0**：收紧可达性，优先打 CVE-2026-56155 等 7 月身份相关补丁。
4. 对照 [CISA KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) 核对本表 CVE 是否仍在册及 due date。
5. 公网暴露面：能下线临时下线或仅 VPN/私有接入。

## 处置信度

| 环境 | 信度 | 说明 |
|------|------|------|
| 本地 SharePoint 未打齐 7 月/KEV 相关补丁（尤其可暴露） | **高** | 多 CVE 在野 + 50522 有公开 PoC 后利用报道 |
| 有 AD FS | **高** | 官方在野提权；与文档/身份面绑定 |
| 已补丁且完成 machine key 轮换 + 排查 | **中** | 残余风险在是否漏补/漏扫 |
| 仅云 SharePoint | **低～观察** | 本簇主焦点在 on-prem |
| 总评 | **高（on-prem SharePoint / AD FS）** | 官方路径清晰；KEV 强制联邦节奏可当行业优先级参考 |

## 权威回执

- CISA 加固 Alert（含 KEV 时间线）：https://www.cisa.gov/news-events/alerts/2026/07/14/cisa-urges-sharepoint-hardening-after-new-exploitations
- CISA KEV 目录：https://www.cisa.gov/known-exploited-vulnerabilities-catalog
- MSRC 示例：https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-58644 等
- THN 等对 CVE-2026-50522 / 58644 的公开复述（交叉）

## 缺口与保留

- 利用团伙归因与完整 IOC 未展开。
- 联邦 due date 以 CISA 原文为准（已过期条目仍应视为「必须补」）。
- 50522 与 58644 的认证前提以 MSRC 为准，媒体表述可能打架。
- 不附、不链 exploit 仓库正文。
