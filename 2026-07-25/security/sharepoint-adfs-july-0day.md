# /intelligence：Microsoft SharePoint / AD FS 7 月在野漏洞簇

| | |
|--|--|
| 类型 | 网络安全情报 |
| 窗口 | 约 2026-07 月中下旬 · 截至 2026-07-25 |
| 模式 | security · 事实信心 高 · 舆论信心 中 |
| 信源 | CISA KEV/警报、微软 MSRC、专业媒体综述 |
| skill | raystyle/intelligence ≥ v1.0.2（不交付 payload；可做 PoC/处置信度） |

## 结论（先读）

2026 年 7 月微软补丁周期前后，本地 SharePoint Server 与 AD FS 多枚漏洞被确认在野利用，并进入/关联 CISA KEV 与加固警报。重点包括反序列化 RCE 与提权。  
有本地 SharePoint / AD FS 的环境应优先打 7 月补丁并按 CISA 清单加固；纯消费级 macOS 桌面默认不直接暴露该面。

## 技术要点（公开编号，不完全列表）

| CVE | 方向 | 备注 |
|-----|------|------|
| CVE-2026-50522 | SharePoint 反序列化 | 可网络侧代码执行；KEV 时间线约 07-22 一带 |
| CVE-2026-58644 | SharePoint 反序列化 RCE | CVSS 约 9.8；Site Owner 等前提下远程注入；补丁前在野 |
| CVE-2026-56164 | SharePoint 提权 | 7 月 Patch Tuesday，微软确认在野 |
| CVE-2026-56155 | AD FS 提权 | 同上；身份基础设施优先 |
| 相关 | CVE-2026-32201、CVE-2026-45659 等 | CISA 同波段 SharePoint 加固提及 |

影响版本（SharePoint 侧公开说明）：Subscription Edition / 2019 / 2016 等本地服务器。  
公开描述的利用后活动：RCE、IIS machine key 窃取、反序列化持久化、落毒等（机理层级，不附 payload）。

## 利用与 PoC 信度

| 项 | 判断 |
|----|------|
| 声称状态 | 在野：微软确认 + CISA KEV/加固警报；公开利用细节多见于厂商/应急，完整「民间 PoC 合集」非本篇重点 |
| 真伪/水分 | **在野叙事信度高**。依据：CISA KEV 与 MSRC 官方路径；非单一自媒体。是否每个 CVE 都有独立公开 PoC 未逐条审计。 |
| 本文边界 | 不交付可复制 exploit；只写状态与处置 |

## 影响面

- 高：互联网或半暴露的本地 SharePoint；AD FS 作为身份关键路径。
- 低：无上述组件的端点/开发机。

## 处置建议（可执行）

1. 安装微软 2026 年 7 月相关安全更新并验证成功。
2. 按 CISA：AMSI 集成、入侵痕迹扫描后再轮换 machine key、加强日志、尽量不直接对公网暴露 SharePoint、限制 Central Admin。
3. 将 AD FS 视为 tier-0 身份设施，收紧可达性。
4. 持续对照 CISA KEV 目录。

## 处置信度

| 环境 | 信度 | 说明 |
|------|------|------|
| 本地 SharePoint 未打 7 月补丁（尤其可暴露） | **高** | KEV + 官方在野确认；应立刻补丁与加固 |
| 有 AD FS 且可达面大 | **高** | 身份面；与 SharePoint 同波段看待 |
| 仅 M365 云 SharePoint、无本地 Server | **低～观察** | 本簇公开焦点在本地 Server；仍跟微软云侧通告 |
| 总评 | **高（对本地 SharePoint/AD FS）** | 官方与 KEV 对齐；迁移/补丁路径清晰 |

## 权威回执

- https://www.cisa.gov/known-exploited-vulnerabilities-catalog
- https://www.cisa.gov/news-events/alerts/2026/07/14/cisa-urges-sharepoint-hardening-after-new-exploitations
- 各 CVE 的 MSRC / NVD 条目

## 缺口与保留

- 具体利用团伙与 IOC 未在本篇展开。
- 联邦修复 deadline 以 CISA 原文为准。
- 未对网上流传的全部 PoC 做真伪逐条拆解。
