# 在 CodeArts CLI 中使用 deep-research-framework

[English](README.en.md)

安装 `deep-research-framework` Skill，让 CodeArts 先定义问题和来源层级，再把事实、推断与不确定性分开写成结论优先的研究提纲。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 仅影响当前仓库；个人级 `~/.codeartsdoer` 供当前用户跨项目使用。请二选一；项目级同名 Skill 优先，任何同名冲突都应停止。本指南验证了两个新项目和无项目覆盖的个人级 consumer。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目安装并验证项目级 deep-research-framework Skill。固定 https://github.com/sickn33/agentic-awesome-skills.git 的 v19.7.0、Commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；完整复制 skills/deep-research-framework（仅 SKILL.md）到 <项目>/.codeartsdoer/skills/deep-research-framework，vendor 为 <项目>/.codeartsdoer/vendor/agentic-awesome-deep-research-framework。不得修改 package.json、codearts_cli.json、权限、凭据、插件或其他 Skill。
先运行 codearts --version、git --version、node --version、codearts models 并让我选择 <model>。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-deep-research-framework'，$target=Join-Path $root 'skills\deep-research-framework'，$userTarget=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer\skills\deep-research-framework'；任一存在就停止。确认路径在 $root 内，运行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；执行 git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/deep-research-framework；要求 HEAD 精确等于 a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；运行 Copy-Item -LiteralPath (Join-Path $source 'skills\deep-research-framework') -Destination $target -Recurse；要求 $target/SKILL.md SHA-256 为 EB11BD0C5CBEF0BDE6AF1FB71FE449250AA6001C34A70A11372BB47A9F12BB2F。
运行 codearts debug skill，唯一同名 location 必须是 $target/SKILL.md。用所选模型原样运行：codearts run -m "<model>" --format json 'Call the skill tool exactly once with name deep-research-framework and use no other tool. Using only these supplied facts, produce a concise conclusion-first research outline: Source A is the vendor release note dated 2026-10-01 and says feature X shipped; Source B is an independent benchmark dated 2026-10-03 and measured X at 120 ms on one Windows laptop; no Linux data and no sample size beyond one are available. Separate the research question, source tiers, verified facts, inference, and uncertainty. Do not browse, invent citations, generalize the benchmark, or claim Linux support. End with SOURCE_TIERS_EXPLICIT and UNCERTAINTY_EXPLICIT.'
只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、无其他工具事件、包含结论/问题/来源层级/事实/推断/不确定性、把 120 ms 限定为单机 n=1、没有 Linux 或泛化声明并包含两个标记才通过。报告 Commit、路径和 Skill call ID。之后只删除 $target 与 $source，重新诊断确认旧路径消失；禁止删除整个 .codeartsdoer、用户配置或其他 Skill。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 deep-research-framework Skill。固定 https://github.com/sickn33/agentic-awesome-skills.git 的 v19.7.0、Commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；完整复制 skills/deep-research-framework（仅 SKILL.md）到 ~/.codeartsdoer/skills/deep-research-framework，vendor 为 ~/.codeartsdoer/vendor/agentic-awesome-deep-research-framework。不得修改用户根 package.json、codearts_cli.json、权限、凭据、插件或其他 Skill。
先运行 codearts --version、git --version、node --version 和 codearts models，让我选择 <model>。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-deep-research-framework'，$target=Join-Path $root 'skills\deep-research-framework'，$consumer=Join-Path (Get-Location) 'codearts-deep-research-framework-consumer'，$projectTarget=Join-Path $consumer '.codeartsdoer\skills\deep-research-framework'；任一存在就停止。创建 consumer。运行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；执行 git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/deep-research-framework；要求 HEAD 为 a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；运行 Copy-Item -LiteralPath (Join-Path $source 'skills\deep-research-framework') -Destination $target -Recurse；要求 $target/SKILL.md SHA-256 为 EB11BD0C5CBEF0BDE6AF1FB71FE449250AA6001C34A70A11372BB47A9F12BB2F。
进入 consumer 运行 codearts debug skill，唯一 name=deep-research-framework 的 location 必须为 $target/SKILL.md。原样运行：codearts run -m "<model>" --format json 'Call the skill tool exactly once with name deep-research-framework and use no other tool. Using only these supplied facts, produce a concise conclusion-first research outline: Source A is the vendor release note dated 2026-10-01 and says feature X shipped; Source B is an independent benchmark dated 2026-10-03 and measured X at 120 ms on one Windows laptop; no Linux data and no sample size beyond one are available. Separate the research question, source tiers, verified facts, inference, and uncertainty. Do not browse, invent citations, generalize the benchmark, or claim Linux support. End with SOURCE_TIERS_EXPLICIT and UNCERTAINTY_EXPLICIT.' 只有退出码 0、一个来自 $target 的 completed Skill 事件、无其他工具事件、显式结论/问题/来源层级/事实/推断/不确定性、120 ms 限定为 Windows 单机 n=1、无 Linux 或泛化声明且两个标记存在才通过。报告 call ID。验证后只删除 $target、$source 和新建 consumer，从无项目覆盖目录确认旧路径消失。禁止删除用户根、整个 .codeartsdoer、根配置、其他 Skill 或既有项目。
```

## Windows 手动安装

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer'
# 个人级：$root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-deep-research-framework'
$target=Join-Path $root 'skills\deep-research-framework'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/deep-research-framework
if((git -C $source rev-parse HEAD).Trim() -ne 'a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\deep-research-framework') -Destination $target -Recurse
if((Get-FileHash -Algorithm SHA256 -LiteralPath (Join-Path $target 'SKILL.md')).Hash -ne 'EB11BD0C5CBEF0BDE6AF1FB71FE449250AA6001C34A70A11372BB47A9F12BB2F'){throw 'Skill hash mismatch'}
```

## CodeArts 配置

用 `codearts models` 选择模型。Skill 只有 Markdown，不需要依赖、插件、凭据或配置变更；本指南的可复现 smoke test 故意禁止网络，所以无需 `--auto`。实际联网研究仍需用户单独授权并逐条核对来源。

## 验证

先诊断绝对路径，再运行 Agent 提示词中的命令。成功必须有唯一 completed Skill 事件和清晰的事实/推断/不确定性边界；只把给定事实复述成段落不够。

## 使用

```text
Use deep-research-framework. First define the research question and source tiers. Cross-check primary claims where evidence permits, label every inference, and finish with explicit unknowns and invalidation conditions. Never invent a citation.
```

## 更新

固定新 release/Commit 后重新审查并重复三范围验证和回滚；不要跟随浮动 `main`。

## 卸载

先用 `[IO.Path]::GetFullPath()` 确认两个路径都在 `$root` 下，再精确运行 `Remove-Item -LiteralPath (Join-Path $root 'skills\deep-research-framework') -Recurse -Force` 和 `Remove-Item -LiteralPath (Join-Path $root 'vendor\agentic-awesome-deep-research-framework') -Recurse -Force`，最后运行 `codearts debug skill` 确认旧路径消失。不得删除整个 `.codeartsdoer`、根配置、其他 Skill、项目或用户文件。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | Agentic Awesome `v19.7.0`；Commit `a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1` |
| 文件 / SHA-256 | `SKILL.md` / `EB11BD0C5CBEF0BDE6AF1FB71FE449250AA6001C34A70A11372BB47A9F12BB2F` |
| 许可证 | MIT；来源 `alapha888/agent-skills-en` |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；PowerShell 7.6.6；Git 2.53.0.windows.2；Node 24.14.1；npm 11.11.0；`mimo/mimo-v2.5` |
| 范围 | 项目 A、全新项目 B、无项目覆盖个人级 consumer；2026-10-16 |
| 核心 Skill calls | `call_f830a512792d47199e9bac1c`、`call_66fe7dcb9f2245f09b31d015`、`call_97762e6bd1764e8d953523ff` |

## 已知限制

验证使用两条给定事实且禁止联网，只证明研究结构、来源分层和不确定性纪律；没有验证真实网页抓取、引用可达性、付费数据库、长报告或领域专家结论。Skill 不能保证模型自动找到第二独立来源。

## 安全

固定目录只有 `SKILL.md`；三次调用无其他工具、网络、文件写入、凭据或遥测。回滚后用户根基线哈希未变。

## 证据与来源

- [中文研究记录](../../research/2026-10-16.md) · [English](../../research/2026-10-16.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1/skills/deep-research-framework)
- [原始仓库](https://github.com/alapha888/agent-skills-en)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
