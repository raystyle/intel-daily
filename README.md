# intel-daily

技术与网络安全情报日更产物（由 `/intelligence` 技能写入）。

| | |
|--|--|
| 本地 | `~/intel-daily` |
| 远程 | https://github.com/raystyle/intel-daily |
| 技能 | `raystyle/intelligence` · `/intelligence` |

## 目录约定

```text
YYYY-MM-DD/
  security/     # 漏洞、在野、厂商通告类简报
  tech/         # 产品/模型/技术动态
  hybrid/       # 安全+技术混合
  index.md      # 当日索引（可选）
```

文件命名建议：

```text
{简短slug}.md
# 例: cve-2026-16723-fastjson.md
# 例: sharepoint-kev-july.md
# 例: kimi-k3-launch.md
```

## 与其它仓库

| 仓库 | 用途 |
|------|------|
| intelligence | 技能源码与契约 |
| intel-daily | 每日情报**产物** |
| mac-note | 本机运维与部署日志 |

## 同步

```bash
cd ~/intel-daily
git add -A && git status
git commit -m "intel: YYYY-MM-DD ..."
git push
```
