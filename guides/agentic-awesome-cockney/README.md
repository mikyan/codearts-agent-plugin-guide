# 在 CodeArts CLI 中使用 cockney

[English](README.en.md)

安装 `cockney` Skill，让 CodeArts Agent 用简短的伦敦东区口吻完成最小正确改动，并在安全或破坏性风险上自动改回清晰的普通英语。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 只影响当前仓库；个人级 `~/.codeartsdoer` 可供当前用户的所有项目使用。请二选一。CodeArts 官方规则是项目级同名 Skill 优先；若项目级、个人级、目标 Skill 或 vendor 中已有同名目录，立即停止，不要覆盖。本指南同时验证了两个全新项目与一个没有项目级同名 Skill 的个人级 consumer。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目中安装并验证项目级 cockney Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v19.7.0，Commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；复制源是仓库内完整目录 skills/cockney（仅 SKILL.md），vendor 目标是 <项目>/.codeartsdoer/vendor/agentic-awesome-cockney，CodeArts 原生目标是 <项目>/.codeartsdoer/skills/cockney。不得修改 package.json、codearts_cli.json、权限、凭据、插件或其他 Skill。
先在项目根运行 codearts --version、git --version、node --version 和 codearts models，让我从真实列表选择 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-cockney'，$target=Join-Path $root 'skills\cockney'，$userTarget=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer\skills\cockney'。若 $source、$target 或 $userTarget 任一存在，立即停止并报告冲突，不得覆盖。确认 $source 与 $target 的绝对路径均在 $root 内，然后在项目根执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/cockney；确认 (git -C $source rev-parse HEAD).Trim() 精确等于 a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；Copy-Item -LiteralPath (Join-Path $source 'skills\cockney') -Destination $target -Recurse；确认 $target/SKILL.md 存在且 SHA-256 为 22093F5D83E02E9F69F8E98E94A91332F62140DAEF22A5A04E924A2F57872C91。
在项目根运行 codearts debug skill；唯一 name=cockney 的 location 必须精确是 $target/SKILL.md。把 <model> 替换为刚选择的 provider/model ID，原样运行：codearts run -m "<model>" --format json 'Call the skill tool exactly once with name cockney and use no other tool. In full mode, diagnose this supplied synthetic fact in at most four sentences: greeting in email.py uses user.name without a null fallback, so new users see Hello None; the minimal fix is Hello there and the email tests pass. Preserve the identifiers email.py, user.name, Hello None, and Hello there exactly. Then add one separate plain-English security warning sentence: deploy.sh drops the users table and requires backup confirmation. Do not invent edits or test commands. End with COCKNEY_MODE_PROVED and RISK_WARNING_PLAIN.'
只有命令退出码为 0、恰好一个来自 $target 的 completed Skill 事件、没有其他工具事件、答案保留四个准确标识符、工程说明使用 Cockney 口吻、安全警告使用普通英语且包含两个结尾标记时才通过。报告 Commit、目标绝对路径、Skill call ID 与最终判据。验证后再次核对 $target 和 $source 都在 $root 内，只删除这两个精确目录；重新运行 codearts debug skill，确认旧绝对路径消失。禁止删除整个 .codeartsdoer、项目根、用户根、根配置或其他文件。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 cockney Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v19.7.0，Commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；复制源是仓库内完整目录 skills/cockney（仅 SKILL.md），vendor 目标是 ~/.codeartsdoer/vendor/agentic-awesome-cockney，CodeArts 原生目标是 ~/.codeartsdoer/skills/cockney。不得修改用户 package.json、codearts_cli.json、权限、凭据、插件或其他 Skill。
先运行 codearts --version、git --version、node --version 和 codearts models，让我从真实列表选择 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-cockney'，$target=Join-Path $root 'skills\cockney'，$consumer=Join-Path (Get-Location) 'codearts-cockney-consumer'。若 $source、$target 或 $consumer 任一存在，立即停止并报告冲突。创建全新的 $consumer，并确认其中不存在 .codeartsdoer/skills/cockney；不得在 consumer 中创建项目级同名 Skill。确认 $source 与 $target 的绝对路径均在 $root 内，然后执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent),$consumer -Force | Out-Null；git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/cockney；确认 (git -C $source rev-parse HEAD).Trim() 精确等于 a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；Copy-Item -LiteralPath (Join-Path $source 'skills\cockney') -Destination $target -Recurse；确认 $target/SKILL.md 存在且 SHA-256 为 22093F5D83E02E9F69F8E98E94A91332F62140DAEF22A5A04E924A2F57872C91。
进入 $consumer 运行 codearts debug skill；唯一 name=cockney 的 location 必须精确是 $target/SKILL.md。把 <model> 替换为刚选择的 provider/model ID，原样运行：codearts run -m "<model>" --format json 'Call the skill tool exactly once with name cockney and use no other tool. In full mode, diagnose this supplied synthetic fact in at most four sentences: greeting in email.py uses user.name without a null fallback, so new users see Hello None; the minimal fix is Hello there and the email tests pass. Preserve the identifiers email.py, user.name, Hello None, and Hello there exactly. Then add one separate plain-English security warning sentence: deploy.sh drops the users table and requires backup confirmation. Do not invent edits or test commands. End with COCKNEY_MODE_PROVED and RISK_WARNING_PLAIN.'
只有命令退出码为 0、恰好一个来自 $target 的 completed Skill 事件、没有其他工具事件、答案保留四个准确标识符、工程说明使用 Cockney 口吻、安全警告使用普通英语且包含两个结尾标记时才通过。报告 Commit、目标绝对路径、Skill call ID 与最终判据。验证后核对绝对路径，只删除 $target、$source 和本提示词新建的精确 $consumer；从无项目级覆盖的目录重新运行 codearts debug skill，确认旧路径消失。禁止删除用户根、整个 .codeartsdoer、根配置、其他 Skill 或任何既有项目。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version`、`node --version` 和 `codearts models`。以下默认为项目级；个人级把 `$root` 换成注释中的用户目录。若任一目标存在就停止。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer'
# 个人级：$root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-cockney'
$target=Join-Path $root 'skills\cockney'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/cockney
if((git -C $source rev-parse HEAD).Trim() -ne 'a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\cockney') -Destination $target -Recurse
if((Get-FileHash -Algorithm SHA256 -LiteralPath (Join-Path $target 'SKILL.md')).Hash -ne '22093F5D83E02E9F69F8E98E94A91332F62140DAEF22A5A04E924A2F57872C91'){throw 'Skill hash mismatch'}
```

个人级验证必须从没有 `.codeartsdoer/skills/cockney` 的独立 consumer 目录启动，避免项目级同名 Skill 抢占。

## CodeArts 配置

用 `codearts models` 选择真实可用的 `provider/model`。本 Skill 只有一份 Markdown，不需要 npm、Python、MCP、Hook、插件、凭据或网络，也不需要修改 `package.json`、`codearts_cli.json` 或权限文件。无需 `--auto`。

## 验证

先运行 `codearts debug skill`，核对唯一 `cockney` 的绝对 `location`。随后使用所选模型运行对应 Agent 提示词中的 `codearts run` 命令。成功判据是：退出码 0；恰好一个来自预期路径的 completed `skill` 事件；无其他工具事件；准确保留 `email.py`、`user.name`、`Hello None`、`Hello there`；普通工程说明使用 Cockney 口吻；`deploy.sh`/`users` 风险警告回到普通英语；最终包含 `COCKNEY_MODE_PROVED` 与 `RISK_WARNING_PLAIN`。

## 使用

```text
Use cockney in full mode. Review this small bug and recommend the minimum correct fix. Keep identifiers, paths, commands, errors, and numbers exact. Switch to plain English for any security, destructive, irreversible, ordered, or clarifying content. Do not make changes unless I explicitly ask.
```

停止该风格时明确说 `speak proper`、`stop cockney`、`normal mode` 或 `/cockney off`。由于本安装只有 Skill 文本，没有上游 Claude Hook，跨会话持久化未验证；新会话最好重新说明模式。

## 更新

更新 release 或 Commit 前，重新审查 `skills/cockney/SKILL.md` 与许可证来源，并重新执行两个全新项目、无项目覆盖的个人级 consumer、代表调用和精确回滚。不要用浮动 `main` 替换固定版本。

## 卸载

项目级或个人级都只删除所选 `$root/skills/cockney` 与 `$root/vendor/agentic-awesome-cockney`。只有确认 consumer 是本指南新建的精确目录时才能删除它。之后运行 `codearts debug skill`，确认旧绝对路径消失。不要删除整个 `.codeartsdoer`、用户根、根配置或其他 Skill。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | Agentic Awesome `v19.7.0`；Commit `a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1` |
| 文件 / SHA-256 | `SKILL.md` / `22093F5D83E02E9F69F8E98E94A91332F62140DAEF22A5A04E924A2F57872C91` |
| 许可证 | Skill frontmatter：MIT；来源 `Kadajett/cockney` |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；PowerShell 7.6.6；Git 2.53.0.windows.2；Node 24.14.1；npm 11.11.0；`mimo/mimo-v2.5` |
| 范围 | 项目 A、全新项目 B、无项目覆盖的个人级 consumer；2026-10-15 |
| 核心 Skill calls | `call_e840a82f253b4b6195059777`、`call_46f03d37038e430b9a524b96`、`call_ad2a52cadd894e609ea07e98` |

## 已知限制

验证只覆盖文本风格、最小修复说明、准确标识符保留和风险警告切换，没有让 Agent 实际修改代码。Skill 自身不能保证模型在长会话中持续保持口吻；上游 Claude Code Hook 的跨轮次状态没有安装或验证。Cockney 表达可能降低非英语用户或辅助技术用户的可读性，团队文档、用户可见文案和正式事故沟通应谨慎使用。

## 安全

固定目录只有 `SKILL.md`，没有 manifest、锁文件、安装脚本、二进制、下载器、运行时网络、凭据或遥测。三个代表调用都只产生一个 completed Skill 事件，没有执行命令或改写文件。风险句都按 Skill 要求使用普通英语。卸载后项目 A、项目 B 与个人级共 6 个 Skill/vendor 目标均不存在，用户根 `package.json`、`codearts_cli.json` 与持久权限文件 SHA-256 保持基线。

## 证据与来源

- [中文研究记录](../../research/2026-10-15.md) · [English](../../research/2026-10-15.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1/skills/cockney)
- [Agentic Awesome v19.7.0](https://github.com/sickn33/agentic-awesome-skills/releases/tag/v19.7.0)
- [原始仓库](https://github.com/Kadajett/cockney)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
