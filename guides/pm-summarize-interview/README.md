# 在 CodeArts CLI 中使用 PM 访谈摘要

[English](README.en.md)

从 PM Skills 安装经过验证的 `summarize-interview` Skill，把客户访谈逐字稿整理为 JTBD、满意度信号、关键洞察和行动项明确的 Markdown 摘要。

## 选择安装范围

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 随研究仓库固定版本、团队共享或只在一个项目使用。默认推荐。 |
| 个人级 | `~/.codeartsdoer` | 当前用户需要跨多个项目重复整理访谈。 |

同名 Skill 按 CodeArts 官方规则以项目级为优先。不要同时安装；发现同名 Skill 或 vendor 目录时必须停止，不能覆盖。

## 让 Agent 帮你安装

验证会生成 `interview-summary-mei-2026-09-09.md`。它属于用户产物，卸载 Skill 时不得删除。

### 项目级安装提示词

```text
请在当前项目为 CodeArts CLI 安装并验证 phuryn/pm-skills 的 summarize-interview Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 只安装到当前项目 .codeartsdoer/vendor/pm-skills-summarize-interview 与 .codeartsdoer/skills/summarize-interview。不得修改 ~/.codeartsdoer、凭据、任何根 package.json 或 codearts_cli.json。
2. 在项目根运行 codearts --version、git --version 和 codearts models；如果有多个模型，先让我选择准确 provider/model ID。
3. 检查两个安装目标和项目根 interview-summary-mei-2026-09-09.md；任一存在就停止并报告，不得覆盖。
4. 在项目根的 PowerShell 原样执行：
   $projectRoot = (Get-Location).Path
   $source = Join-Path $projectRoot ".codeartsdoer\vendor\pm-skills-summarize-interview"
   $target = Join-Path $projectRoot ".codeartsdoer\skills\summarize-interview"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-product-discovery/skills/summarize-interview"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\summarize-interview") -Destination $target -Recurse
5. 运行 codearts debug skill，确认 summarize-interview 的 location 是当前项目 .codeartsdoer/skills/summarize-interview/SKILL.md。
6. 为非交互写入建立一次性权限目录，不改用户持久 permission/global.json：
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-summarize-interview-project-verify"
   if (Test-Path -LiteralPath $verifyRoot) { throw "Verification directory already exists; inspect it instead of overwriting." }
   $kernelData = Join-Path $verifyRoot "kernel-data"
   $permissionDir = Join-Path $kernelData "storage\permission"
   New-Item -ItemType Directory -Path $permissionDir -Force | Out-Null
   @'
   [
     { "permission": "edit", "pattern": "*", "action": "allow" },
     { "permission": "write", "pattern": "*", "action": "allow" },
     { "permission": "bash", "pattern": "*", "action": "deny" },
     { "permission": "read", "pattern": "*", "action": "deny" },
     { "permission": "webfetch", "pattern": "*", "action": "deny" },
     { "permission": "websearch", "pattern": "*", "action": "deny" },
     { "permission": "external_directory_write", "pattern": "*", "action": "deny" },
     { "permission": "external_directory_read", "pattern": "*", "action": "deny" },
     { "permission": "dotfile", "pattern": "*", "action": "deny" }
   ]
   '@ | Set-Content -LiteralPath (Join-Path $permissionDir "global.json") -Encoding utf8
7. 把 <选择的模型> 替换为第 2 步模型，在项目根用真实 CodeArts 26.8.1 可执行文件运行：
   $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
   $codeartsExe = Join-Path $userRoot "installers\bin\codearts.exe"
   $env:SCENARIO = "codeartsdoer"
   $env:KERNEL_DATA_DIR = $kernelData
   $env:KERNEL_CONFIG_DIR = $userRoot
   $env:OPENCODE_CHANNEL = "latest"
   $env:OPENCODE_CONFIG = Join-Path $userRoot "codearts_cli.json"
   $env:OPENCODE_CONFIG_FILE = "codearts_cli.json,codearts_cli.jsonc"
   $env:OPENCODE_MODE = "tui"
   $env:PLUGIN_ENV = "hc"
   $env:NODE_TLS_REJECT_UNAUTHORIZED = "0"
   $env:OPENCODE_DISABLE_MODELS_FETCH = "1"
   $env:OPENCODE_DISABLE_AUTOUPDATE = "true"
   $env:OPENCODE_ALWAYS_NOTIFY_UPDATE = "false"
   $env:OMO_SEND_ANONYMOUS_TELEMETRY = "0"
   $prompt = "Call the skill tool exactly once with name summarize-interview, then use the write tool exactly once to create interview-summary-mei-2026-09-09.md in the current workspace. Use no other tool, do not access the network, and do not read other files. Summarize this complete synthetic transcript only: Interview 2026-09-09 10:00 HKT. Participant Mei, commuter undergraduate; interviewer product researcher. Mei plans with paper on the MTR because it opens instantly and needs no login. She likes crossing items off. She often misses deadlines when tutors change dates in chat. She tried a cloud planner but stopped because setup took 30 minutes and she worried about uploading course notes. She wants tomorrow's urgent items in under 10 seconds, offline, with clear keyboard labels. Satisfaction with paper: mixed. Quote: I need the next thing, not another system to maintain. Unknown: willingness to pay, laptop use, sync need. Action: researcher to test a paper prototype by 2026-09-16. Use the skill template, retain the quote exactly, use - for unavailable fields, separate evidence from inference, and do not invent facts. The saved file must include exact markers **Date**, **Participants**, **Background**, **Current Solution**, **What They Like About Current Solution**, **Problems With Current Solution**, **Key Insights**, **Action Items**, JTBD, mixed, I need the next thing, not another system to maintain., UNKNOWN, and 2026-09-16. Finish by stating only that interview-summary-mei-2026-09-09.md was created."
   & $codeartsExe run -m "<选择的模型>" --auto --format json $prompt
8. 通过标准：恰好一次完成的 summarize-interview Skill 和一次完成的 write；Skill 来源是第 4 步目标；文件包含模板全部字段、JTBD、mixed、原样引语、三个 UNKNOWN 与 2026-09-16；不得新增人物、事实、付费/同步结论或行动。只有文件或只有文本均不算通过。
9. 记录证据后只删除 $verifyRoot，不删除生成的摘要。报告 Commit、源目录、目标目录、完成事件、来源路径和事实忠实度。任一步失败必须停止，不得宣称成功。
10. 卸载只能删除 .codeartsdoer/skills/summarize-interview 与 .codeartsdoer/vendor/pm-skills-summarize-interview；不得删除 .codeartsdoer 本身、配置、凭据、其他 Skill、逐字稿或摘要。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装并验证个人级 phuryn/pm-skills summarize-interview Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 设置 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"。只安装到 $userRoot/vendor/pm-skills-summarize-interview 与 $userRoot/skills/summarize-interview；不得修改 package.json、codearts_cli.json、持久 permission/global.json、凭据、项目配置或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；如有多个模型，先让我选择准确 provider/model ID。
3. 检查两个安装目标；任一存在就停止，不得覆盖。
4. 在 PowerShell 原样执行：
   $source = Join-Path $userRoot "vendor\pm-skills-summarize-interview"
   $target = Join-Path $userRoot "skills\summarize-interview"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-product-discovery/skills/summarize-interview"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\summarize-interview") -Destination $target -Recurse
5. 创建无项目覆盖的隔离 workspace 和完整临时权限：
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-summarize-interview-user-verify"
   if (Test-Path -LiteralPath $verifyRoot) { throw "Verification directory already exists; inspect it instead of overwriting." }
   $workspace = Join-Path $verifyRoot "workspace"
   $kernelData = Join-Path $verifyRoot "kernel-data"
   $permissionDir = Join-Path $kernelData "storage\permission"
   New-Item -ItemType Directory -Path $workspace,$permissionDir -Force | Out-Null
   @'
   [
     { "permission": "edit", "pattern": "*", "action": "allow" },
     { "permission": "write", "pattern": "*", "action": "allow" },
     { "permission": "bash", "pattern": "*", "action": "deny" },
     { "permission": "read", "pattern": "*", "action": "deny" },
     { "permission": "webfetch", "pattern": "*", "action": "deny" },
     { "permission": "websearch", "pattern": "*", "action": "deny" },
     { "permission": "external_directory_write", "pattern": "*", "action": "deny" },
     { "permission": "external_directory_read", "pattern": "*", "action": "deny" },
     { "permission": "dotfile", "pattern": "*", "action": "deny" }
   ]
   '@ | Set-Content -LiteralPath (Join-Path $permissionDir "global.json") -Encoding utf8
   if (Test-Path -LiteralPath (Join-Path $workspace ".codeartsdoer\skills\summarize-interview")) { throw "Project override exists." }
6. 切换到 $workspace，运行 codearts debug skill，确认 location 是 $userRoot/skills/summarize-interview/SKILL.md。
7. 在该 workspace，把 <选择的模型> 替换为第 2 步模型并原样执行：
   $codeartsExe = Join-Path $userRoot "installers\bin\codearts.exe"
   $env:SCENARIO = "codeartsdoer"
   $env:KERNEL_DATA_DIR = $kernelData
   $env:KERNEL_CONFIG_DIR = $userRoot
   $env:OPENCODE_CHANNEL = "latest"
   $env:OPENCODE_CONFIG = Join-Path $userRoot "codearts_cli.json"
   $env:OPENCODE_CONFIG_FILE = "codearts_cli.json,codearts_cli.jsonc"
   $env:OPENCODE_MODE = "tui"
   $env:PLUGIN_ENV = "hc"
   $env:NODE_TLS_REJECT_UNAUTHORIZED = "0"
   $env:OPENCODE_DISABLE_MODELS_FETCH = "1"
   $env:OPENCODE_DISABLE_AUTOUPDATE = "true"
   $env:OPENCODE_ALWAYS_NOTIFY_UPDATE = "false"
   $env:OMO_SEND_ANONYMOUS_TELEMETRY = "0"
   $prompt = "Call the skill tool exactly once with name summarize-interview, then use the write tool exactly once to create interview-summary-mei-2026-09-09.md in the current workspace. Use no other tool, do not access the network, and do not read other files. Summarize this complete synthetic transcript only: Interview 2026-09-09 10:00 HKT. Participant Mei, commuter undergraduate; interviewer product researcher. Mei plans with paper on the MTR because it opens instantly and needs no login. She likes crossing items off. She often misses deadlines when tutors change dates in chat. She tried a cloud planner but stopped because setup took 30 minutes and she worried about uploading course notes. She wants tomorrow's urgent items in under 10 seconds, offline, with clear keyboard labels. Satisfaction with paper: mixed. Quote: I need the next thing, not another system to maintain. Unknown: willingness to pay, laptop use, sync need. Action: researcher to test a paper prototype by 2026-09-16. Use the skill template, retain the quote exactly, use - for unavailable fields, separate evidence from inference, and do not invent facts. The saved file must include exact markers **Date**, **Participants**, **Background**, **Current Solution**, **What They Like About Current Solution**, **Problems With Current Solution**, **Key Insights**, **Action Items**, JTBD, mixed, I need the next thing, not another system to maintain., UNKNOWN, and 2026-09-16. Finish by stating only that interview-summary-mei-2026-09-09.md was created."
   & $codeartsExe run -m "<选择的模型>" --auto --format json $prompt
8. 通过标准与项目级相同：一次完成的目标 Skill、一次完成的 write、准确个人级来源路径、模板字段和全部忠实度断言。记录证据后只删除 $verifyRoot，不删除其他临时目录或用户文件。
9. 报告 Commit、源目录、目标目录、完成事件、来源路径和结果。任一步失败必须停止，不得宣称成功。
10. 卸载只能删除 $userRoot/skills/summarize-interview 与 $userRoot/vendor/pm-skills-summarize-interview；不得删除用户配置、持久权限、凭据、其他 Skill、逐字稿或摘要。
```

## Windows 手动安装

```powershell
codearts --version
git --version
codearts models
```

项目级在项目根执行：

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-summarize-interview"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\summarize-interview"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-product-discovery/skills/summarize-interview"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\summarize-interview") -Destination $target -Recurse
```

个人级执行：

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-summarize-interview"
$target = Join-Path $userRoot "skills\summarize-interview"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-product-discovery/skills/summarize-interview"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\summarize-interview") -Destination $target -Recurse
```

只复制该 Skill，不运行上游脚本或依赖。

## CodeArts 配置

Skill 本身不改 `codearts_cli.json`。按[官方安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)安装 CLI，用 `codearts models` 选择真实模型，凭据保留在本地配置或环境变量。

写文件时，交互式会话可按[官方权限说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0006.html)批准。CLI 26.8.1 的非交互 `ask` 会自动拒绝，即使带 `--auto`；提示词因此用可删除的隔离 `KERNEL_DATA_DIR` 与临时权限 JSON，并直接调用 `codearts.exe` 复用官方启动器环境。`NODE_TLS_REJECT_UNAUTHORIZED=0` 是该版本启动器现状，不应扩散到其他工具。

## 验证

用 `codearts debug skill` 确认准确来源，再运行完整 smoke test。成功必须同时有完成 Skill 事件、完成 Write 事件、磁盘文件、模板字段和事实忠实度。测试真实访谈前先用合成逐字稿；真实内容需要组织授权与数据处理边界。

## 使用

```text
Call the skill tool with name summarize-interview. 读取我提供的完整访谈逐字稿，按模板总结 Current Solution、JTBD、满意度、Problems、Key Insights 和 Action Items。直接引语必须逐字保留；缺失信息写 -；把推断单独标出，不得补造事实。
```

## 更新

先审阅新 Commit 和 `pm-product-discovery/skills/summarize-interview`，只移除当前范围旧目标，再按固定 Commit 重装并重复完整验证。不要把浮动 `main` 当成已验证版本。

## 卸载

项目级只移除 `.codeartsdoer/skills/summarize-interview` 与 `.codeartsdoer/vendor/pm-skills-summarize-interview`；个人级只移除 `~/.codeartsdoer/skills/summarize-interview` 与 `~/.codeartsdoer/vendor/pm-skills-summarize-interview`。不要删除逐字稿、摘要、配置或其他 Skills。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Works** |
| 上游项目 | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| 上游版本 | v2.1.0 |
| 固定源码 | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| 已验证 Skill | `summarize-interview` |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目 A、全新项目 B、个人级 |
| 最后验证日期 | 2026-09-09 |

三个范围都完成准确发现与目标路径加载，只发生一次完成的 Skill 和一次完成的 Write；三份摘要均保留原话、未知项、JTBD、满意度与行动日期，没有新增人物或结论。安装、产物和临时权限均精确回滚，用户配置与持久权限哈希不变。

## 已知限制

只验证短篇英文合成逐字稿与 Markdown 输出；未验证 PDF、音频转写、长访谈、多人对话、中文/粤语逐字稿、附件读取、跨访谈综合或其他模型。摘要可能遗漏语气与上下文，不能替代原始记录或研究者复核。

## 安全

- 固定 Commit，只复制 `pm-product-discovery/skills/summarize-interview`；无依赖、脚本、二进制、遥测或运行时联网要求。
- 真实逐字稿可能含个人资料、商业秘密或敏感研究数据；必须先获授权、最小化内容并遵守保存期限。
- 隔离权限只允许测试 workspace 的 Write/Edit，拒绝 Shell、读取、浏览和外部目录访问，完成后精确删除。
- 直接引语、日期、姓名和行动必须回看原文；推断要明确标注。

## 证据与来源

- [2026-09-09 实测记录](../../research/2026-09-09.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI 权限](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0006.html)
- [固定版本 Skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-product-discovery/skills/summarize-interview/SKILL.md)
- [上游仓库](https://github.com/phuryn/pm-skills)
