# /intelligence：Microsoft SharePoint / AD FS 7 月在野漏洞簇

**类型：** 网络安全情报  
**窗口：** 约 2026-07 月中下旬 · 截至 2026-07-25  
**模式：** security · 事实信心 高 · 舆论信心 中  
**信源：** CISA KEV/警报、微软 MSRC、专业媒体综述

## 结论（先读）

2026 年 **7 月**微软补丁周期前后，**本地 SharePoint Server** 与 **AD FS** 多枚漏洞被确认**在野利用**并进入/关联 CISA KEV 与加固警报。重点包括反序列化 RCE 与提权类问题。  
**有本地 SharePoint / AD FS 的环境应优先打 7 月补丁并按 CISA 清单加固**；纯消费级 macOS 桌面默认不直接暴露该面。

## 技术要点（公开编号，不完全列表）

| CVE | 方向 | 备注 |
|-----|------|------|
| CVE-2026-50522 | SharePoint 反序列化 | 可网络侧代码执行；KEV 时间线约 07-22 一带 |
| CVE-2026-58644 | SharePoint 反序列化 RCE | CVSS 约 9.8；Site Owner 等前提下远程注入；补丁前在野 |
| CVE-2026-56164 | SharePoint 提权 | 7 月 Patch Tuesday，微软确认在野 |
| CVE-2026-56155 | AD FS 提权 | 同上；身份基础设施优先 |
| 相关 | CVE-2026-32201、CVE-2026-45659 等 | CISA 同波段 SharePoint 加固提及 |

影响版本（SharePoint 侧公开说明）：Subscription Edition / 2019 / 2016 等**本地**服务器。

利用后活动公开描述含：RCE、**IIS machine key 窃取**、反序列化持久化、落毒等。

## 影响面

- **高：** 互联网或半暴露的本地 SharePoint；AD FS 作为身份关键路径  
- **低：** 无上述组件的端点/开发机  

## 处置建议（可执行）

1. 安装微软 **2026 年 7 月**相关安全更新并验证成功。  
2. 按 CISA：AMSI 集成、入侵痕迹扫描后再轮换 machine key、加强日志、尽量不直接对公网暴露 SharePoint、限制 Central Admin。  
3. 将 AD FS 视为 tier-0 身份设施，收紧可达性。  
4. 持续对照 [CISA KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)。  

## 权威回执

- https://www.cisa.gov/known-exploited-vulnerabilities-catalog  
- https://www.cisa.gov/news-events/alerts/2026/07/14/cisa-urges-sharepoint-hardening-after-new-exploitations  
- 各 CVE 的 MSRC / NVD 条目  

## 缺口与保留

- 具体利用团伙与 IOC 未在本篇展开。  
- 联邦修复 deadline 以 CISA 原文为准。  
