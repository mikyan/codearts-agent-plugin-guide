# 在 CodeArts CLI 中使用 database-optimizer

[English](README.en.md)

安装 `database-optimizer` Skill，用已有执行计划和延迟数据提出可验证、可回滚的数据库优化方案。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合仓库内共享；个人级 `~/.codeartsdoer` 适合跨项目。二选一，项目级同名 Skill 优先；任何同名目标或 vendor 冲突都必须停止。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目安装并验证项目级 database-optimizer。固定 https://github.com/sickn33/agentic-awesome-skills.git 的 v17.3.0 / Commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294 / skills/database-optimizer。先运行 codearts --version、git --version、codearts models，让我选择 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-database-optimizer'，$target=Join-Path $root 'skills\database-optimizer'；检查 $source、$target、~/.codeartsdoer/skills/database-optimizer，任一存在就停止。不得修改 package.json、codearts_cli.json、凭据、插件或其他 Skill。
依次执行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/database-optimizer/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；确认 HEAD 精确匹配；Copy-Item -LiteralPath (Join-Path $source 'skills\database-optimizer') -Destination $target -Recurse。
运行 codearts debug skill，唯一 database-optimizer location 必须是 $target/SKILL.md。替换 <model> 后原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name database-optimizer and use no other tool. Do not access files or the network. Analyze only this PostgreSQL evidence: orders has 2,000,000 rows; query SELECT id,total FROM orders WHERE customer_id=$1 AND created_at >= $2 ORDER BY created_at DESC LIMIT 50; p95 is 800 ms; EXPLAIN shows a sequential scan; no relevant index exists; the database time is 650 ms of the 800 ms request. Recommend the first optimization, exact index column order, how to validate with EXPLAIN ANALYZE and before/after p95, rollback, and unknowns. Do not claim an unmeasured improvement.' 只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、无其他工具事件，且答案给出 (customer_id, created_at DESC)、EXPLAIN ANALYZE、前后 p95、DROP INDEX 回滚并拒绝虚构收益时通过。报告 Commit、路径和事件。卸载只删除精确 $target 与 $source，再确认旧路径消失。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 database-optimizer。固定仓库、版本、Commit、源目录为 https://github.com/sickn33/agentic-awesome-skills.git、v17.3.0、69906dde999aaa0f3d173f0e3d5bcdb84c87a294、skills/database-optimizer。运行 codearts --version、git --version、codearts models 并让我选择 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-database-optimizer'，$target=Join-Path $root 'skills\database-optimizer'；确认 $source、$target 和全新 consumer 的同名项目 Skill 不存在，冲突时停止。不得修改用户 package.json、codearts_cli.json、凭据、插件或项目配置。
依次执行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/database-optimizer/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；确认 HEAD 精确匹配；Copy-Item -LiteralPath (Join-Path $source 'skills\database-optimizer') -Destination $target -Recurse。进入无同名项目 Skill 的全新 consumer，运行 codearts debug skill，唯一 database-optimizer location 必须是 $target/SKILL.md。替换 <model> 后原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name database-optimizer and use no other tool. Do not access files or the network. Analyze only this PostgreSQL evidence: orders has 2,000,000 rows; query SELECT id,total FROM orders WHERE customer_id=$1 AND created_at >= $2 ORDER BY created_at DESC LIMIT 50; p95 is 800 ms; EXPLAIN shows a sequential scan; no relevant index exists; the database time is 650 ms of the 800 ms request. Recommend the first optimization, exact index column order, how to validate with EXPLAIN ANALYZE and before/after p95, rollback, and unknowns. Do not claim an unmeasured improvement.' 只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、无其他工具事件，且答案给出 (customer_id, created_at DESC)、EXPLAIN ANALYZE、前后 p95、DROP INDEX 回滚并拒绝虚构收益时通过。报告 Commit、路径和事件。卸载只删除精确 $target、$source 和核对后的 consumer，再确认旧路径消失；不得删除用户根或其他 Skill。
```

## Windows 手动安装

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-database-optimizer'; $target=Join-Path $root 'skills\database-optimizer'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/database-optimizer/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\database-optimizer') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择可用模型。本 Skill 无运行时依赖；不要为了测试连接真实数据库。

## 验证

按提示词运行只读合成分析；要求唯一 completed Skill 事件、精确索引顺序、测量与回滚步骤，并明确实际收益尚未测量。

## 使用

```text
Use database-optimizer. 根据我提供的查询、执行计划和基线，先找首要瓶颈，再给可测量、可回滚的最小优化；不要猜收益。
```

## 更新

更换 Commit 前重新审查内容和许可证，并重做三范围与回滚验证。

## 卸载

只删除 `$root/skills/database-optimizer` 与 `$root/vendor/agentic-awesome-database-optimizer`，随后确认旧路径消失。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | `v17.3.0`；Commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `database-optimizer` / `9FF4B8C8D4726F9A388932B46D74B370FF8D417758704BF343F7DE1AD5F45917` |
| 内容许可证 | AAS 原创非代码内容：CC BY 4.0 |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-09-17 |

## 已知限制

只验证基于合成 PostgreSQL 证据的只读建议；没有连接数据库、创建索引或运行负载测试。命令必须先在测试环境审查。

## 安全

固定目录只有一个 10,534 字节的 `SKILL.md`，无依赖、脚本、二进制、下载器或遥测；三次调用只有目标 Skill 事件。

## 证据与来源

- [中文研究记录](../../research/2026-09-17.md) · [English](../../research/2026-09-17.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/database-optimizer)
- [AAS 内容许可证](https://github.com/sickn33/agentic-awesome-skills/blob/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/LICENSE-CONTENT)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
