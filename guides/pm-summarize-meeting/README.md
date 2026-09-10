# 在 CodeArts CLI 中使用 PM 会议摘要

[English](README.en.md)

从 PM Skills 安装经过验证的 `summarize-meeting` Skill，把会议逐字稿整理为参与者、决策、行动项和开放问题清晰的结构化摘要。

## 选择安装范围

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 随会议资料仓库固定版本、团队共享。默认推荐。 |
| 个人级 | `~/.codeartsdoer` | 当前用户跨多个项目反复整理会议。 |

同名 Skill 按 CodeArts 官方规则以项目级为优先。不要同时安装；发现同名 Skill 或 vendor 目录时必须停止，不能覆盖。

## 让 Agent 帮你安装

### 项目级安装提示词

```text
请在当前项目为 CodeArts CLI 安装并验证 phuryn/pm-skills 的 summarize-meeting Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 只安装到当前项目 .codeartsdoer/vendor/pm-skills-summarize-meeting 与 .codeartsdoer/skills/summarize-meeting。不得修改 ~/.codeartsdoer、凭据、任何根 package.json、codearts_cli.json 或其他 Skill。
2. 在项目根运行 codearts --version、git --version 和 codearts models；如有多个模型，先让我选择准确 provider/model ID。
3. 检查两个安装目标；任一存在就停止并报告，不得覆盖。
4. 在项目根的 PowerShell 原样执行：
   $projectRoot = (Get-Location).Path
   $source = Join-Path $projectRoot ".codeartsdoer\vendor\pm-skills-summarize-meeting"
   $target = Join-Path $projectRoot ".codeartsdoer\skills\summarize-meeting"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/summarize-meeting"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\summarize-meeting") -Destination $target -Recurse
5. 运行 codearts debug skill，确认 summarize-meeting 的 location 是当前项目 .codeartsdoer/skills/summarize-meeting/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步的模型，在项目根原样执行：
   $prompt = "Call the skill tool exactly once with name summarize-meeting. Use no other tool, do not access the network, and do not read or write files. Summarize only this complete synthetic transcript: Meeting on 2026-09-10 from 09:00 to 09:20 HKT. Participants: Mei, product researcher; Arun, engineer; Sofia, accessibility tester. Topic: CommuteCalm pilot readiness. Mei reported five students completed the paper-prototype test, but demand is not validated. Arun said missing-time review is ready and encrypted sync is blocked on an external security review. Sofia found keyboard labels missing on two screens. Decision: pilot stays local-only and starts after keyboard labels are fixed. Actions: Arun fixes keyboard labels by 2026-09-12; Sofia retests by 2026-09-13; Mei recruits 15 more pilot users by 2026-09-16. Open question: whether students want paid sync. Produce sections for meeting summary, date and time, participants, topic, summary, action items, decisions made, and open questions. Preserve owners and dates, state demand is not validated, and do not invent facts. Return the summary directly."
   codearts run -m "<选择的模型>" --format json $prompt
7. 通过标准：恰好一次完成的 summarize-meeting Skill；来源解析到第 4 步目标；没有其他工具事件；中英文标题均可，但必须保留日期时间、三名参与者与角色、三个行动负责人和日期、两项决策、需求未验证以及付费同步开放问题。不得新增人物、行动或结论。
8. 报告 Commit、源目录、目标目录、完成事件、来源路径和事实忠实度。任一步失败必须停止，不得宣称成功。
9. 卸载只能删除 .codeartsdoer/skills/summarize-meeting 与 .codeartsdoer/vendor/pm-skills-summarize-meeting；不得删除 .codeartsdoer 本身、配置、凭据、其他 Skill、逐字稿或用户保存的摘要。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装并验证个人级 phuryn/pm-skills summarize-meeting Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 设置 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"。只安装到 $userRoot/vendor/pm-skills-summarize-meeting 与 $userRoot/skills/summarize-meeting；不得修改 package.json、codearts_cli.json、权限文件、凭据、项目配置或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；如有多个模型，先让我选择准确 provider/model ID。
3. 检查两个安装目标；任一存在就停止，不得覆盖。
4. 在 PowerShell 原样执行：
   $source = Join-Path $userRoot "vendor\pm-skills-summarize-meeting"
   $target = Join-Path $userRoot "skills\summarize-meeting"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/summarize-meeting"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\summarize-meeting") -Destination $target -Recurse
5. 创建无项目级同名 Skill 的干净目录：
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-summarize-meeting-user-verify"
   if (Test-Path -LiteralPath $verifyRoot) { throw "Verification directory already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path $verifyRoot | Out-Null
   if (Test-Path -LiteralPath (Join-Path $verifyRoot ".codeartsdoer\skills\summarize-meeting")) { throw "Project override exists." }
   Set-Location -LiteralPath $verifyRoot
6. 运行 codearts debug skill，确认 location 是 $userRoot/skills/summarize-meeting/SKILL.md。
7. 把 <选择的模型> 替换为第 2 步模型，在该目录原样执行：
   $prompt = "Call the skill tool exactly once with name summarize-meeting. Use no other tool, do not access the network, and do not read or write files. Summarize only this complete synthetic transcript: Meeting on 2026-09-10 from 09:00 to 09:20 HKT. Participants: Mei, product researcher; Arun, engineer; Sofia, accessibility tester. Topic: CommuteCalm pilot readiness. Mei reported five students completed the paper-prototype test, but demand is not validated. Arun said missing-time review is ready and encrypted sync is blocked on an external security review. Sofia found keyboard labels missing on two screens. Decision: pilot stays local-only and starts after keyboard labels are fixed. Actions: Arun fixes keyboard labels by 2026-09-12; Sofia retests by 2026-09-13; Mei recruits 15 more pilot users by 2026-09-16. Open question: whether students want paid sync. Produce sections for meeting summary, date and time, participants, topic, summary, action items, decisions made, and open questions. Preserve owners and dates, state demand is not validated, and do not invent facts. Return the summary directly."
   codearts run -m "<选择的模型>" --format json $prompt
8. 通过标准与项目级相同：一次完成的目标 Skill、准确个人级来源路径、无其他工具事件，以及对全部参与者、行动、决策和开放问题的忠实摘要。记录证据后只删除 $verifyRoot。
9. 报告 Commit、源目录、目标目录、完成事件、来源路径和结果。任一步失败必须停止。
10. 卸载只能删除 $userRoot/skills/summarize-meeting 与 $userRoot/vendor/pm-skills-summarize-meeting；不得删除用户配置、权限、凭据、其他 Skill、逐字稿或用户摘要。
```

## Windows 手动安装

先运行 `codearts models` 选择模型。项目级在项目根执行：

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-summarize-meeting"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\summarize-meeting"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Existing target; stop." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-execution/skills/summarize-meeting"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\summarize-meeting") -Destination $target -Recurse
```

个人级把前两行换成：

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-summarize-meeting"
$target = Join-Path $userRoot "skills\summarize-meeting"
```

其余命令相同。只复制该 Skill，不运行上游脚本或安装依赖。

## CodeArts 配置

Skill 本身不改 `codearts_cli.json`。按[官方安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)配置 CLI，用 `codearts models` 选择真实 `provider/model`；凭据保留在本地配置或环境变量中。

## 验证

先用 `codearts debug skill` 确认准确来源，再运行 Agent 提示词中的只读 smoke test。成功必须同时有完成的目标 Skill 事件、准确来源路径和事实忠实的摘要。CodeArts 可能按模型语言输出中文标题；应核对语义字段而不是只匹配英文标题。

## 使用

```text
Call the skill tool with name summarize-meeting. 只总结我提供的完整逐字稿，列出日期时间、参与者与角色、主题、讨论摘要、行动项表、已做决策和开放问题。逐项保留负责人和截止日期；缺失内容写 UNKNOWN，不得补造事实。直接返回摘要，不读取或写入文件。
```

如需保存，把确认无误的最终文本另存为 Markdown；本次验证没有授权 Skill 写文件。

## 更新

审阅新 Commit 与 `pm-execution/skills/summarize-meeting` 后，只移除当前范围旧目标，按新固定 Commit 重装并重复完整验证。不要把浮动 `main` 当成已验证版本。

## 卸载

项目级只移除 `.codeartsdoer/skills/summarize-meeting` 与 `.codeartsdoer/vendor/pm-skills-summarize-meeting`；个人级只移除 `~/.codeartsdoer/skills/summarize-meeting` 与 `~/.codeartsdoer/vendor/pm-skills-summarize-meeting`。不要删除逐字稿、摘要、配置或其他 Skills。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Works** |
| 上游项目 | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| 上游版本 | v2.1.0 |
| 固定源码 | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| 已验证 Skill | `summarize-meeting` |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目 A、全新项目 B、个人级 |
| 最后验证日期 | 2026-09-10 |

三个范围都完成准确发现与目标路径加载，只发生一次完成的 Skill 事件。三份摘要都保留会议时间、三名参与者、三个行动负责人和日期、两项决策、需求未验证与付费同步开放问题；两份中文输出经人工语义复核通过。全部安装精确回滚，用户配置、根清单和持久权限哈希不变。

## 已知限制

只验证短篇英文合成逐字稿与直接文本输出；未验证音频转写、长会议、重叠说话、附件读取、文件写入、跨会议综合、真实个人资料、粤语逐字稿或其他模型。摘要可能遗漏语气与上下文，不能替代原始记录或参会者确认。

## 安全

- 固定 Commit，只复制单个 `SKILL.md`；无依赖、脚本、二进制、遥测或运行时联网要求。
- 真实会议记录可能含个人资料、商业秘密或敏感决策；必须先获授权、最小化内容并遵守保存期限。
- 人名、日期、决策和行动必须回看原文；不要把推断改写为已决定事项。

## 证据与来源

- [2026-09-10 实测记录](../../research/2026-09-10.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI 命令](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0034.html)
- [固定版本 Skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-execution/skills/summarize-meeting/SKILL.md)
