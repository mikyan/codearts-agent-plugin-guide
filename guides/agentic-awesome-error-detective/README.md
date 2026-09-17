# 在 CodeArts CLI 中使用 error-detective

[English](README.en.md)

安装 `error-detective` Skill，把跨服务日志整理成时间线、关联链、可证伪的根因假设与复发检测方法。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合单仓库；个人级 `~/.codeartsdoer` 适合跨项目。二选一，项目级同名 Skill 优先；同名 Skill 或 vendor 存在时停止。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目安装并验证项目级 error-detective。固定 https://github.com/sickn33/agentic-awesome-skills.git 的 v17.3.0 / Commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294 / skills/error-detective。先运行 codearts --version、git --version、codearts models，让我选择 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-error-detective'，$target=Join-Path $root 'skills\error-detective'；检查 $source、$target、~/.codeartsdoer/skills/error-detective，任一存在就停止。不得修改 package.json、codearts_cli.json、凭据、插件或其他 Skill。
依次执行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/error-detective/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；确认 HEAD 精确匹配；Copy-Item -LiteralPath (Join-Path $source 'skills\error-detective') -Destination $target -Recurse。
运行 codearts debug skill，唯一 error-detective location 必须是 $target/SKILL.md。替换 <model> 后原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name error-detective and use no other tool. Do not access files or the network. Analyze only these logs: 10:00 deploy api v7 completed; 10:02 api request r-17 returned 500 with db timeout; 10:02 payments request r-17 returned 502 after api failure. Produce a timeline, correlation, root-cause hypothesis explicitly labeled as a hypothesis, counterfactual unknowns, one regex or monitoring query for recurrence, and immediate verification steps. Do not claim the deployment caused the timeout.' 只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、无其他工具事件，且答案保留“部署与超时只有相关性”、列出数据库状态和部署差异等未知项时通过。报告 Commit、路径和事件。卸载只删除精确 $target 与 $source，再确认旧路径消失。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 error-detective。固定仓库、版本、Commit、源目录为 https://github.com/sickn33/agentic-awesome-skills.git、v17.3.0、69906dde999aaa0f3d173f0e3d5bcdb84c87a294、skills/error-detective。运行 codearts --version、git --version、codearts models 并让我选择 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-error-detective'，$target=Join-Path $root 'skills\error-detective'；确认 $source、$target 和全新 consumer 的同名项目 Skill 都不存在，冲突时停止。不得修改用户 package.json、codearts_cli.json、凭据、插件或项目配置。
依次执行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/error-detective/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；确认 HEAD 精确匹配；Copy-Item -LiteralPath (Join-Path $source 'skills\error-detective') -Destination $target -Recurse。进入无同名项目 Skill 的 consumer，运行 codearts debug skill，唯一 error-detective location 必须是 $target/SKILL.md。替换 <model> 后原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name error-detective and use no other tool. Do not access files or the network. Analyze only these logs: 10:00 deploy api v7 completed; 10:02 api request r-17 returned 500 with db timeout; 10:02 payments request r-17 returned 502 after api failure. Produce a timeline, correlation, root-cause hypothesis explicitly labeled as a hypothesis, counterfactual unknowns, one regex or monitoring query for recurrence, and immediate verification steps. Do not claim the deployment caused the timeout.' 只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、无其他工具事件，且答案保留“部署与超时只有相关性”、列出数据库状态和部署差异等未知项时通过。报告 Commit、路径和事件。卸载只删除精确 $target、$source 和核对后的 consumer，再确认旧路径消失；不得删除用户根或其他 Skill。
```

## Windows 手动安装

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-error-detective'; $target=Join-Path $root 'skills\error-detective'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/error-detective/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\error-detective') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择可用模型。本 Skill 无依赖；真实日志可能含敏感信息，输入前先脱敏。

## 验证

按提示词运行合成日志分析；要求唯一 completed Skill 事件，时间线和请求链准确，并把根因保持为假设而非事实。

## 使用

```text
Use error-detective. 只根据这些脱敏日志建立时间线、关联链和可证伪根因假设；列出反事实未知与复发查询。
```

## 更新

更换 Commit 前重新审查内容和许可证，并重做三范围与回滚验证。

## 卸载

只删除 `$root/skills/error-detective` 与 `$root/vendor/agentic-awesome-error-detective`，随后确认旧路径消失。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | `v17.3.0`；Commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `error-detective` / `6BB4EBB968B61ED5CC9F44951F99350411D79ACBBDAF0F4BF9F124D9BFB2A3A0` |
| 内容许可证 | AAS 原创非代码内容：CC BY 4.0 |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-09-17 |

## 已知限制

只验证三行合成日志的只读分析；没有读取真实日志系统、代码库或监控后端。生成的正则和查询必须按实际日志 schema 调整。

## 安全

固定目录只有一个 2,166 字节的 `SKILL.md`，无依赖、脚本、二进制、下载器或遥测；三次调用只有目标 Skill 事件。

## 证据与来源

- [中文研究记录](../../research/2026-09-17.md) · [English](../../research/2026-09-17.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/error-detective)
- [AAS 内容许可证](https://github.com/sickn33/agentic-awesome-skills/blob/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/LICENSE-CONTENT)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
