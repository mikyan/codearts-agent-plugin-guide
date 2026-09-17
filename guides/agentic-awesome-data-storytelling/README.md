# 在 CodeArts CLI 中使用 data-storytelling

[English](README.en.md)

安装 `data-storytelling` Skill，把给定数据组织成面向决策者的叙事，同时明确因果、预测和预算等未知边界。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合固定仓库；个人级 `~/.codeartsdoer` 适合跨项目。二选一；项目级同名 Skill 优先，任何同名 Skill 或 vendor 冲突都必须停止。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目安装并验证项目级 data-storytelling。固定 https://github.com/sickn33/agentic-awesome-skills.git 的 v17.3.0 / Commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294 / skills/data-storytelling。先运行 codearts --version、git --version、codearts models，让我选择 <model>，不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-data-storytelling'，$target=Join-Path $root 'skills\data-storytelling'；检查 $source、$target、~/.codeartsdoer/skills/data-storytelling，任一存在就停止。不得修改 package.json、codearts_cli.json、凭据、插件或其他 Skill。
依次执行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/data-storytelling/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；确认 HEAD 精确匹配后 Copy-Item -LiteralPath (Join-Path $source 'skills\data-storytelling') -Destination $target -Recurse。
运行 codearts debug skill，唯一 data-storytelling location 必须是 $target/SKILL.md。替换 <model> 后原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name data-storytelling and use no other tool. Do not access files or the network. Turn only these facts into a concise executive data story: quarterly signups Q1=100, Q2=120, Q3=90; no causal data and no budget information were supplied. Include a headline, context, key insight with correct arithmetic, one evidence-bounded next action, and an uncertainty note. Do not invent causes, benchmarks, money, confidence intervals, or forecasts.' 只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、无其他工具事件，且答案给出 Q2 +20%、Q3 -25%、不虚构原因时通过。报告 Commit、路径和事件。卸载只删除精确 $target 与 $source，再确认旧路径消失；禁止删除整个 .codeartsdoer 或根配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 data-storytelling。固定仓库、版本、Commit 与源目录分别为 https://github.com/sickn33/agentic-awesome-skills.git、v17.3.0、69906dde999aaa0f3d173f0e3d5bcdb84c87a294、skills/data-storytelling。运行 codearts --version、git --version、codearts models 并让我选择 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-data-storytelling'，$target=Join-Path $root 'skills\data-storytelling'；确认 $source、$target 和全新 consumer 的项目同名 Skill 都不存在，冲突时停止。不得修改用户 package.json、codearts_cli.json、凭据、插件或项目配置。
依次执行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/data-storytelling/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；确认 HEAD 精确匹配后 Copy-Item -LiteralPath (Join-Path $source 'skills\data-storytelling') -Destination $target -Recurse。进入无同名项目 Skill 的全新 consumer，运行 codearts debug skill，唯一 data-storytelling location 必须是 $target/SKILL.md。替换 <model> 后原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name data-storytelling and use no other tool. Do not access files or the network. Turn only these facts into a concise executive data story: quarterly signups Q1=100, Q2=120, Q3=90; no causal data and no budget information were supplied. Include a headline, context, key insight with correct arithmetic, one evidence-bounded next action, and an uncertainty note. Do not invent causes, benchmarks, money, confidence intervals, or forecasts.' 只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、无其他工具事件，且答案给出 Q2 +20%、Q3 -25%、不虚构原因时通过。报告 Commit、路径和事件。卸载只删除精确 $target、$source 和核对后的 consumer，再用 codearts debug skill 确认旧路径消失；不得删除用户根或其他 Skill。
```

## Windows 手动安装

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-data-storytelling'; $target=Join-Path $root 'skills\data-storytelling'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/data-storytelling/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\data-storytelling') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择可用模型；本 Skill 不需要依赖或配置合并。

## 验证

运行提示词中的 debug 与 smoke test；要求唯一 completed Skill 事件来自目标路径，算术正确且没有补造原因、预测或预算。

## 使用

```text
Use data-storytelling. 只用我给的数据写一段管理层叙事；区分观察、解释和未知，不补造因果。
```

## 更新

更换 Commit 前重新审查全部内容与许可证，并重做三范围验证和回滚。

## 卸载

只删除 `$root/skills/data-storytelling` 与 `$root/vendor/agentic-awesome-data-storytelling`，随后确认旧路径消失。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | `v17.3.0`；Commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `data-storytelling` / `25B3D4B6538627B85FDE94A8DDAE5FDB6ECE78500FB8DFA8D3F026D0B6104488` |
| 内容许可证 | AAS 原创非代码内容：CC BY 4.0 |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-09-17 |

## 已知限制

只验证三点合成数据的只读叙事；未验证绘图、演示文稿生成、外部基准或统计显著性分析。

## 安全

固定目录只有一个 13,586 字节的 `SKILL.md`，没有脚本、依赖、二进制、下载器或遥测；三次调用只有目标 Skill 事件。

## 证据与来源

- [中文研究记录](../../research/2026-09-17.md) · [English](../../research/2026-09-17.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/data-storytelling)
- [AAS 内容许可证](https://github.com/sickn33/agentic-awesome-skills/blob/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/LICENSE-CONTENT)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
