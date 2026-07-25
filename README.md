# intel-daily

intelligence skill 每日记录产物（本远程仓库）。

| | |
|--|--|
| 远程 | https://github.com/raystyle/intel-daily |
| 技能 | `raystyle/intelligence` · `/intelligence` |
| 本机目录示例 | `~/Documents/intel-daily` — **维护者本机**把本仓 clone 到此；他人可放任意路径 |

`~/Documents/intel-daily` **不是** skill 或 GitHub 的全局固定安装路径，只是本机工作区的本地 clone 约定。

## 目录约定

```text
YYYY-MM-DD/
  security/     # 网络安全情报
  tech/         # 前沿技术情报
  hybrid/       # 两线交织
  index.md      # 当日索引（完整简报必更新）
```

文件命名：

```text
{简短slug}.md
# 例: cve-2026-16723-fastjson.md
# 例: sharepoint-kev-july.md
# 例: kimi-k3-launch.md
```

## 写入与同步（由 /intelligence 执行）

**有结论的一次情报交互结束 → 必产物 + 必同步。**

| 步骤 | 说明 |
|------|------|
| 生成 | 落盘到当日目录 + 更新 `index.md` |
| 同步 | 同一轮内 `commit` + `push`（不必等用户再说同步） |
| 例外 | 仅追问无结论，或用户明确免落盘/免 push |

```bash
# 在本机 clone 目录内执行（示例路径）
cd ~/Documents/intel-daily   # 或 $INTEL_DAILY_ROOT
git add -A && git status
git commit -m "intel: YYYY-MM-DD ..."
git push origin main
```

## 与其它仓库

| 仓库 | 用途 |
|------|------|
| intelligence | 技能源码与契约 |
| intel-daily | intelligence skill 每日记录**产物**（本仓库） |
