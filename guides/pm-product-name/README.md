# 在 CodeArts CLI 中使用 PM 产品命名

[English](README.en.md)

从 PM Skills 安装经过验证的 `product-name` Skill，基于品牌价值、目标用户、语气与命名约束生成五个候选名称及人工验证建议。

## 选择安装范围

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 随仓库固定版本、团队共享或只在一个项目使用。默认推荐。 |
| 个人级 | `~/.codeartsdoer` | 当前用户需要在多个项目中重复使用。 |

CodeArts 官方文档规定同名 Skill 以项目级为优先。不要在两个范围重复安装；安装前发现同名目录必须停止，不能覆盖。

## 让 Agent 帮你安装

### 项目级安装提示词

```text
请在当前项目为 CodeArts CLI 安装并验证 phuryn/pm-skills 的 product-name Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 只修改当前项目的 .codeartsdoer；不得修改 ~/.codeartsdoer、凭据、全局软件或项目其他文件。
2. 在项目根目录运行 codearts --version、git --version 和 codearts models；如果有多个模型，先让我选择 provider/model ID。
3. 检查 .codeartsdoer/vendor/pm-skills-product-name 和 .codeartsdoer/skills/product-name。任一存在就停止并报告，不得覆盖。
4. 在项目根目录的 PowerShell 原样执行：
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-product-name"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\product-name"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-marketing-growth/skills/product-name"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-marketing-growth\skills\product-name") -Destination $target -Recurse
5. 运行 codearts debug skill，确认 product-name 的 location 是当前项目 .codeartsdoer/skills/product-name/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步确认的模型，然后原样执行：
   codearts run --format json --model "<选择的模型>" "Call the skill tool exactly once with name product-name and use no other tool. Do not access the network or write files. Name a privacy-first offline study planner for Hong Kong commuter undergraduates. Brand values: calm, trustworthy, practical, non-surveillant. Tone: concise English names that Cantonese speakers can pronounce. Avoid the words AI, cloud, smart, study, task, and plan. Generate exactly 5 distinct names. For each give rationale, brand fit, memorability, pronunciation note, one risk, and domain/trademark status explicitly marked UNCHECKED because no search is allowed. Rank all five and recommend one for human validation, without claiming availability. Use exact markers CONTEXT, NAME-1, NAME-2, NAME-3, NAME-4, NAME-5, RATIONALE, BRAND-FIT, MEMORABILITY, PRONUNCIATION, RISK, UNCHECKED, RANKING, and RECOMMENDATION."
7. 通过标准：必须出现一次完成的 `product-name` Skill 事件并解析到所选原生目录；结果必须有恰好五个不含禁用词的名称，每个名称都有理由、品牌匹配、记忆度、发音、风险和 `UNCHECKED`，并给出排序与人工验证建议。域名或商标状态不得声称已验证。
8. 报告 Commit、源目录、目标目录、完成事件、来源路径和结果。卸载只能删除 .codeartsdoer/skills/product-name 与 .codeartsdoer/vendor/pm-skills-product-name；不得删除 .codeartsdoer 本身、package.json、codearts_cli.json 或其他 Skill。任一步失败必须停止，不得宣称成功。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装并验证个人级 phuryn/pm-skills product-name Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 在 PowerShell 设置 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"。只允许修改 $userRoot/vendor/pm-skills-product-name 和 $userRoot/skills/product-name；不得修改 $userRoot/package.json、codearts_cli.json、凭据、项目配置或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；如果有多个模型，先让我选择 provider/model ID。
3. 检查上述两个准确目标；任一存在就停止并报告，不得覆盖。
4. 在同一个 PowerShell 会话原样执行：
   $source = Join-Path $userRoot "vendor\pm-skills-product-name"
   $target = Join-Path $userRoot "skills\product-name"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-marketing-growth/skills/product-name"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-marketing-growth\skills\product-name") -Destination $target -Recurse
5. 切换到没有项目级同名 Skill 的干净目录，运行 codearts debug skill，确认 location 是 ~/.codeartsdoer/skills/product-name/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步模型，并在该干净目录原样执行：
   codearts run --format json --model "<选择的模型>" "Call the skill tool exactly once with name product-name and use no other tool. Do not access the network or write files. Name a privacy-first offline study planner for Hong Kong commuter undergraduates. Brand values: calm, trustworthy, practical, non-surveillant. Tone: concise English names that Cantonese speakers can pronounce. Avoid the words AI, cloud, smart, study, task, and plan. Generate exactly 5 distinct names. For each give rationale, brand fit, memorability, pronunciation note, one risk, and domain/trademark status explicitly marked UNCHECKED because no search is allowed. Rank all five and recommend one for human validation, without claiming availability. Use exact markers CONTEXT, NAME-1, NAME-2, NAME-3, NAME-4, NAME-5, RATIONALE, BRAND-FIT, MEMORABILITY, PRONUNCIATION, RISK, UNCHECKED, RANKING, and RECOMMENDATION."
7. 通过标准与项目级相同：一次完成事件、准确个人级来源路径、五个受约束名称、逐项 `UNCHECKED` 和人工验证建议；不得声称域名或商标可用。
8. 报告 Commit、完成事件、来源路径和结果。卸载只能删除 $userRoot/skills/product-name 与 $userRoot/vendor/pm-skills-product-name；不得删除用户根 package.json、codearts_cli.json、凭据或其他 Skill。任一步失败必须停止，不得宣称成功。
```

## Windows 手动安装

先确认工具和模型：

```powershell
codearts --version
git --version
codearts models
```

项目级在项目根目录执行：

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-product-name"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\product-name"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-marketing-growth/skills/product-name"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-marketing-growth\skills\product-name") -Destination $target -Recurse
```

个人级执行：

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-product-name"
$target = Join-Path $userRoot "skills\product-name"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-marketing-growth/skills/product-name"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-marketing-growth\skills\product-name") -Destination $target -Recurse
```

两种范围都只复制所选 Skill，不执行上游 Python 校验器、安装脚本或依赖。

## CodeArts 配置

本 Skill 不需要修改 `codearts_cli.json`。按[官方安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)安装 CodeArts CLI，并用 `codearts models` 选择真实 `provider/model` ID。自定义模型参考[官方配置示例](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html)；密钥只保存在本地配置或环境变量中。

CodeArts CLI 26.8.1 在本次自定义 Provider 测试中要求进程存在 `CODEARTS_CLI_AK` 与 `CODEARTS_CLI_SK`。测试使用未持久化的非秘密占位值通过该前置检查，模型仍由 Provider 自己的密钥鉴权；这是实测现象，不是兼容保证。华为云托管模型应按[官方 AK/SK 说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html)配置有效凭据。

## 验证

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills | Where-Object { $_.name -eq "product-name" } | Select-Object name, location
```

替换模型 ID 后，运行“让 Agent 帮你安装”中的完整 `codearts run` 命令。必须出现一次完成的 `product-name` Skill 事件并解析到所选原生目录；最终结果必须有五个名称、每项六类说明、排序与人工验证建议。只有文本、没有完成事件，或声称未查询的域名/商标已可用，都不算通过。

## 使用

```text
Call the skill tool with name product-name. 根据这些品牌价值、受众、发音与禁用词约束给出五个名称；域名和商标未经检索时必须标记为未检查。
```

## 更新

先审阅新 Commit 与 `pm-marketing-growth/skills/product-name`。只移除当前范围的旧目标，按固定新 Commit 重装并重复完整验证；不要把浮动 `main` 当成已验证版本。

## 卸载

项目级只移除 `.codeartsdoer/skills/product-name` 与 `.codeartsdoer/vendor/pm-skills-product-name`。个人级只移除 `~/.codeartsdoer/skills/product-name` 与 `~/.codeartsdoer/vendor/pm-skills-product-name`。不要删除 `.codeartsdoer` 根目录、用户根 `package.json`、`codearts_cli.json`、凭据或其他 Skills。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Works** |
| 上游项目 | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| 上游版本 | v2.1.0 |
| 固定源码 | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| 已验证 Skill | `product-name` |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目 A、全新项目 B、个人级 |
| 最后验证日期 | 2026-09-08 |

三个范围均完成安装、精确发现、目标路径加载、代表能力、回滚和移除后诊断。用户根 `package.json` 与 `codearts_cli.json` 的 SHA-256 在测试前后保持不变。

## 已知限制

未联网检查域名、商标、语言歧义、粤语用户实际发音或市场接受度；生成名称只是待人工筛选的创意，不是法律或品牌可用性结论。

只验证 Windows 11、CodeArts CLI 26.8.1、`mimo/mimo-v2.5`、固定 Commit 和上述离线流程；未验证 Linux、macOS、其他模型、联网命名检索、Claude Commands 或完整插件编排。

## 安全

- 固定 Commit，只复制 `pm-marketing-growth/skills/product-name`；不运行上游 `validate_plugins.py`、测试脚本或其他插件内容。
- 测试提示词提供完整本地事实并禁止网络与文件写入；不需要 `--auto`。
- Skill 会影响 Agent 行为；敏感项目使用前应审阅 `SKILL.md`。
- 安装与卸载不修改凭据、用户根清单或 CodeArts 模型配置。

## 证据与来源

- [2026-09-08 实测记录](../../research/2026-09-08.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [固定版本 Skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-marketing-growth/skills/product-name/SKILL.md)
- [上游仓库](https://github.com/phuryn/pm-skills)
