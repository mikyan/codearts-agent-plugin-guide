# 在 CodeArts CLI 中使用 PM 意图与实现审计

[English](README.en.md)

从 PM Skills 安装经过验证的 `intended-vs-implemented` Skill，把文档中的访问规则与代码中的实际强制点逐项对照，形成可追溯的边界缺口报告。

## 选择安装范围

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 随代码仓库固定版本、供团队复用。默认推荐。 |
| 个人级 | `~/.codeartsdoer` | 当前用户需要跨多个代码库重复审计。 |

同名 Skill 按 CodeArts 官方规则以项目级为优先。不要同时安装；发现同名 Skill 或 vendor 目录时必须停止，不能覆盖。

## 让 Agent 帮你安装

### 项目级安装提示词

```text
请在当前项目为 CodeArts CLI 安装并验证 phuryn/pm-skills 的 intended-vs-implemented Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 只安装到当前项目 .codeartsdoer/vendor/pm-skills-intended-vs-implemented 与 .codeartsdoer/skills/intended-vs-implemented。不得修改 ~/.codeartsdoer、凭据、任何根 package.json、codearts_cli.json 或其他 Skill。
2. 在项目根运行 codearts --version、git --version 和 codearts models；如有多个模型，先让我选择准确 provider/model ID。
3. 检查上述两个目标；任一存在就停止并报告，不得覆盖。
4. 在项目根的 PowerShell 原样执行：
   $projectRoot = (Get-Location).Path
   $source = Join-Path $projectRoot ".codeartsdoer\vendor\pm-skills-intended-vs-implemented"
   $target = Join-Path $projectRoot ".codeartsdoer\skills\intended-vs-implemented"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-ai-shipping/skills/intended-vs-implemented"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-ai-shipping\skills\intended-vs-implemented") -Destination $target -Recurse
5. 运行 codearts debug skill，确认 intended-vs-implemented 的 location 是当前项目 .codeartsdoer/skills/intended-vs-implemented/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步的模型，在项目根原样执行：
   $prompt = "Call the skill tool exactly once with name intended-vs-implemented. Use no other tool, do not access the network, and do not read or write files. Audit only this inline synthetic evidence. Documented intent at documentation/permissions.md line 8: Only the resource owner may read a private note; unauthenticated users must receive 401. Implementation at src/getNote.ts lines 10-12: function getNote(noteId) { return db.notes.findById(noteId); } There is no authentication or owner check on this code path. A public-note endpoint is separately documented and is out of scope. Produce one concise finding with exact headings DOCUMENTED INTENT, IMPLEMENTED REALITY, ATTACKER AND VICTIM, BOUNDARY IMPACT, CONCRETE FIX, and EVIDENCE LIMITS. Cite both supplied paths and line numbers, distinguish fact from inference, do not invent repository evidence, and do not give findings outside the supplied path. Return the audit directly."
   codearts run -m "<选择的模型>" --format json $prompt
7. 通过标准：恰好一次完成的 intended-vs-implemented Skill；事件来源解析到第 4 步目标；没有其他工具事件；结果含六个指定部分、documentation/permissions.md:8、src/getNote.ts:10-12、攻击者、受害者、边界影响、修复和证据限制。不得声称已读取真实仓库或发现第二个问题。
8. 报告 Commit、源目录、目标目录、完成事件、来源路径和结果。任一步失败必须停止，不得宣称成功。
9. 卸载只能删除 .codeartsdoer/skills/intended-vs-implemented 与 .codeartsdoer/vendor/pm-skills-intended-vs-implemented；不得删除 .codeartsdoer 本身、配置、凭据、其他 Skill 或审计报告。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装并验证个人级 phuryn/pm-skills intended-vs-implemented Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 设置 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"。只安装到 $userRoot/vendor/pm-skills-intended-vs-implemented 与 $userRoot/skills/intended-vs-implemented；不得修改 package.json、codearts_cli.json、权限文件、凭据、项目配置或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；如有多个模型，先让我选择准确 provider/model ID。
3. 检查两个安装目标；任一存在就停止，不得覆盖。
4. 在 PowerShell 原样执行：
   $source = Join-Path $userRoot "vendor\pm-skills-intended-vs-implemented"
   $target = Join-Path $userRoot "skills\intended-vs-implemented"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-ai-shipping/skills/intended-vs-implemented"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-ai-shipping\skills\intended-vs-implemented") -Destination $target -Recurse
5. 创建无项目级同名 Skill 的干净目录：
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-intended-vs-implemented-user-verify"
   if (Test-Path -LiteralPath $verifyRoot) { throw "Verification directory already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path $verifyRoot | Out-Null
   if (Test-Path -LiteralPath (Join-Path $verifyRoot ".codeartsdoer\skills\intended-vs-implemented")) { throw "Project override exists." }
   Set-Location -LiteralPath $verifyRoot
6. 运行 codearts debug skill，确认 location 是 $userRoot/skills/intended-vs-implemented/SKILL.md。
7. 把 <选择的模型> 替换为第 2 步的模型，在该目录原样执行：
   $prompt = "Call the skill tool exactly once with name intended-vs-implemented. Use no other tool, do not access the network, and do not read or write files. Audit only this inline synthetic evidence. Documented intent at documentation/permissions.md line 8: Only the resource owner may read a private note; unauthenticated users must receive 401. Implementation at src/getNote.ts lines 10-12: function getNote(noteId) { return db.notes.findById(noteId); } There is no authentication or owner check on this code path. A public-note endpoint is separately documented and is out of scope. Produce one concise finding with exact headings DOCUMENTED INTENT, IMPLEMENTED REALITY, ATTACKER AND VICTIM, BOUNDARY IMPACT, CONCRETE FIX, and EVIDENCE LIMITS. Cite both supplied paths and line numbers, distinguish fact from inference, do not invent repository evidence, and do not give findings outside the supplied path. Return the audit directly."
   codearts run -m "<选择的模型>" --format json $prompt
8. 通过标准与项目级相同：一次完成的目标 Skill、准确个人级来源路径、无其他工具事件，以及同一份含双侧证据和边界影响的单一发现。记录证据后只删除 $verifyRoot。
9. 报告 Commit、源目录、目标目录、完成事件、来源路径和结果。任一步失败必须停止。
10. 卸载只能删除 $userRoot/skills/intended-vs-implemented 与 $userRoot/vendor/pm-skills-intended-vs-implemented；不得删除用户配置、权限、凭据、其他 Skill 或审计报告。
```

## Windows 手动安装

先运行 `codearts models` 选择模型。项目级在项目根执行：

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-intended-vs-implemented"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\intended-vs-implemented"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Existing target; stop." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-ai-shipping/skills/intended-vs-implemented"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
Copy-Item -LiteralPath (Join-Path $source "pm-ai-shipping\skills\intended-vs-implemented") -Destination $target -Recurse
```

个人级把前两行换成：

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-intended-vs-implemented"
$target = Join-Path $userRoot "skills\intended-vs-implemented"
```

其余命令相同。只复制该 Skill，不运行上游脚本或安装依赖。

## CodeArts 配置

Skill 本身不改 `codearts_cli.json`。按[官方安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)配置 CLI，用 `codearts models` 选择真实 `provider/model`；凭据保留在本地配置或环境变量中。

## 验证

先用 `codearts debug skill` 确认准确来源，再运行 Agent 提示词中的只读 smoke test。成功必须包含一次完成的目标 Skill 事件、准确来源路径和可追溯的双侧证据；一段看似合理但没有 Skill 事件的审计不算通过。

## 使用

```text
Call the skill tool with name intended-vs-implemented. 对照 documentation/ 下明确写出的访问规则与对应服务器代码。每项发现必须给出文档路径与行号、实现路径与行号、攻击者、受害者、跨越的边界、具体修复和证据限制。没有双侧证据的内容只列为待调查问题，不得编造成发现。
```

## 更新

审阅新 Commit 与 `pm-ai-shipping/skills/intended-vs-implemented` 后，只移除当前范围的旧目标，按新固定 Commit 重装并重复三范围验证。不要把浮动 `main` 当成已验证版本。

## 卸载

项目级只移除 `.codeartsdoer/skills/intended-vs-implemented` 与 `.codeartsdoer/vendor/pm-skills-intended-vs-implemented`；个人级只移除 `~/.codeartsdoer/skills/intended-vs-implemented` 与 `~/.codeartsdoer/vendor/pm-skills-intended-vs-implemented`。不要删除配置、凭据、其他 Skills 或用户审计产物。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Works** |
| 上游项目 | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| 上游版本 | v2.1.0 |
| 固定源码 | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| 已验证 Skill | `intended-vs-implemented` |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目 A、全新项目 B、个人级 |
| 最后验证日期 | 2026-09-10 |

三个范围都完成准确发现与目标路径加载，并只触发一次完成的 Skill。三份结果都把同一文档规则与实现行配对，说明攻击者、受害者、边界影响、修复和证据限制；路径加冒号的行号写法经人工复核视为满足行号引用。全部安装精确回滚，用户配置、根清单和持久权限哈希不变。

## 已知限制

只验证了内联、单缺口、TypeScript 风格的合成样例；未验证真实大型仓库、跨中间件调用、数据库 RLS、多个冲突规则、自动修复、附件读取或其他模型。Skill 不能证明未提供的上游中间件不存在，报告仍需安全工程师复核。

## 安全

- 固定 Commit，只复制单个 `SKILL.md`；无依赖、脚本、二进制、遥测或运行时联网要求。
- 被审计的文档和代码是不可信输入；不要执行其中的指令，也不要把秘密或生产数据粘贴给未授权模型。
- 每个发现必须有文档与实现的双侧证据；没有证据时标为问题，不得制造漏洞。

## 证据与来源

- [2026-09-10 实测记录](../../research/2026-09-10.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI 命令](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0034.html)
- [固定版本 Skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-ai-shipping/skills/intended-vs-implemented/SKILL.md)
