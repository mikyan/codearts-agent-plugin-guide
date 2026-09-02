# 在 CodeArts CLI 中使用 PM 机会解决方案树

[English](README.en.md)

从 PM Skills 安装经过验证的 `opportunity-solution-tree` Skill，把单一可测结果连接到客户机会、多个解决方案与验证实验。

## 选择安装范围

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 随仓库固定版本、团队共享或只在一个项目使用。默认推荐。 |
| 个人级 | `~/.codeartsdoer` | 当前用户需要在多个项目中重复使用。 |

CodeArts 官方文档规定同名 Skill 以项目级为优先。不要在两个范围重复安装；安装前发现同名目录必须停止，不能覆盖。

## 让 Agent 帮你安装

### 项目级安装提示词

```text
请在当前项目为 CodeArts CLI 安装并验证 phuryn/pm-skills 的 opportunity-solution-tree Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 只修改当前项目的 .codeartsdoer；不得修改 ~/.codeartsdoer、凭据、全局软件或项目其他文件。
2. 运行 codearts --version、git --version 和 codearts models；如果有多个模型，先让我选择 provider/model ID。
3. 检查 .codeartsdoer/vendor/pm-skills-opportunity-solution-tree 和 .codeartsdoer/skills/opportunity-solution-tree。任一存在就停止并报告，不得覆盖。
4. 在项目根目录的 PowerShell 原样执行：
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-opportunity-solution-tree"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\opportunity-solution-tree"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-product-discovery/skills/opportunity-solution-tree"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\opportunity-solution-tree") -Destination $target -Recurse
5. 运行 codearts debug skill，确认 opportunity-solution-tree 的 location 是当前项目 .codeartsdoer/skills/opportunity-solution-tree/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步确认的模型，然后原样执行：
   codearts run --format json --model "<选择的模型>" "Call the skill tool exactly once with name opportunity-solution-tree and use no other tool. Do not access the network or write files. Build an Opportunity Solution Tree for the outcome increase 7-day activation of an offline-first study planner from 30% to 45%. Research facts supplied locally: students forget to import deadlines, cannot see the next action, and distrust cloud-only tools. Include at least 3 opportunities, 3 solutions per prioritized opportunity, and one experiment with metric and threshold. Use exact ASCII markers OUTCOME, OPPORTUNITIES, SOLUTIONS, and EXPERIMENTS."
7. 通过标准：必须出现一次完成的 `opportunity-solution-tree` Skill 事件并解析到所选原生目录；结果包含单一指标、至少 3 个机会、每个优先机会至少 3 个方案，以及带指标和阈值的实验。
8. 报告 Commit、源目录、目标目录、完成事件、来源路径和结果。卸载只能删除 .codeartsdoer/skills/opportunity-solution-tree 与 .codeartsdoer/vendor/pm-skills-opportunity-solution-tree；不得删除 .codeartsdoer 本身、package.json、codearts_cli.json 或其他 Skill。任一步失败必须停止，不得宣称成功。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装并验证个人级 phuryn/pm-skills opportunity-solution-tree Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 在 PowerShell 设置 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"。只允许修改 $userRoot/vendor/pm-skills-opportunity-solution-tree 和 $userRoot/skills/opportunity-solution-tree；不得修改 $userRoot/package.json、codearts_cli.json、凭据、项目配置或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；如果有多个模型，先让我选择 provider/model ID。
3. 检查上述两个准确目标；任一存在就停止并报告，不得覆盖。
4. 在同一个 PowerShell 会话原样执行：
   $source = Join-Path $userRoot "vendor\pm-skills-opportunity-solution-tree"
   $target = Join-Path $userRoot "skills\opportunity-solution-tree"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-product-discovery/skills/opportunity-solution-tree"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\opportunity-solution-tree") -Destination $target -Recurse
5. 切换到没有项目级同名 Skill 的干净目录，运行 codearts debug skill，确认 location 是 ~/.codeartsdoer/skills/opportunity-solution-tree/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步模型，并在该干净目录原样执行：
   codearts run --format json --model "<选择的模型>" "Call the skill tool exactly once with name opportunity-solution-tree and use no other tool. Do not access the network or write files. Build an Opportunity Solution Tree for the outcome increase 7-day activation of an offline-first study planner from 30% to 45%. Research facts supplied locally: students forget to import deadlines, cannot see the next action, and distrust cloud-only tools. Include at least 3 opportunities, 3 solutions per prioritized opportunity, and one experiment with metric and threshold. Use exact ASCII markers OUTCOME, OPPORTUNITIES, SOLUTIONS, and EXPERIMENTS."
7. 通过标准：必须出现一次完成的 `opportunity-solution-tree` Skill 事件并解析到所选原生目录；结果包含单一指标、至少 3 个机会、每个优先机会至少 3 个方案，以及带指标和阈值的实验。
8. 报告 Commit、完成事件、来源路径和结果。卸载只能删除 $userRoot/skills/opportunity-solution-tree 与 $userRoot/vendor/pm-skills-opportunity-solution-tree；不得删除用户根 package.json、codearts_cli.json、凭据或其他 Skill。任一步失败必须停止，不得宣称成功。
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
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-opportunity-solution-tree"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\opportunity-solution-tree"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-product-discovery/skills/opportunity-solution-tree"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\opportunity-solution-tree") -Destination $target -Recurse
```

个人级执行：

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-opportunity-solution-tree"
$target = Join-Path $userRoot "skills\opportunity-solution-tree"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-product-discovery/skills/opportunity-solution-tree"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\opportunity-solution-tree") -Destination $target -Recurse
```

两种范围都只复制所选 Skill，不执行上游 Python 校验器、安装脚本或依赖。

## CodeArts 配置

本 Skill 不需要修改 `codearts_cli.json`。按[官方安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)安装 CodeArts CLI，并用 `codearts models` 选择真实 `provider/model` ID。自定义模型参考[官方配置示例](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html)；密钥只保存在本地配置或环境变量中。

CodeArts CLI 26.8.1 在本次自定义 Provider 测试中要求进程存在 `CODEARTS_CLI_AK` 与 `CODEARTS_CLI_SK`。测试使用未持久化的非秘密占位值通过该前置检查，模型仍由 Provider 自己的密钥鉴权；这是实测现象，不是兼容保证。华为云托管模型应按[官方 AK/SK 说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html)配置有效凭据。

## 验证

先检查来源路径：

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills | Where-Object { $_.name -eq "opportunity-solution-tree" } | Select-Object name, location
```

再替换模型 ID 并真实调用：

```powershell
codearts run --format json --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name opportunity-solution-tree and use no other tool. Do not access the network or write files. Build an Opportunity Solution Tree for the outcome increase 7-day activation of an offline-first study planner from 30% to 45%. Research facts supplied locally: students forget to import deadlines, cannot see the next action, and distrust cloud-only tools. Include at least 3 opportunities, 3 solutions per prioritized opportunity, and one experiment with metric and threshold. Use exact ASCII markers OUTCOME, OPPORTUNITIES, SOLUTIONS, and EXPERIMENTS."
```

必须出现一次完成的 `opportunity-solution-tree` Skill 事件并解析到所选原生目录；结果包含单一指标、至少 3 个机会、每个优先机会至少 3 个方案，以及带指标和阈值的实验。 只有最终文本而没有完成的目标 `skill` 事件，不能视为通过。

## 使用

```text
Call the skill tool with name opportunity-solution-tree. 根据这份访谈摘要为“7 日激活率”构建机会解决方案树。
```

## 更新

先审阅新 Commit 与所选 Skill 目录。然后只移除当前范围的旧目标，按固定新 Commit 重装并重复完整验证。不要把浮动 `main` 当成已验证版本。

## 卸载

项目级只移除：

- `.codeartsdoer/skills/opportunity-solution-tree`
- `.codeartsdoer/vendor/pm-skills-opportunity-solution-tree`

个人级只移除：

- `~/.codeartsdoer/skills/opportunity-solution-tree`
- `~/.codeartsdoer/vendor/pm-skills-opportunity-solution-tree`

不要删除 `.codeartsdoer` 根目录、用户根 `package.json`、`codearts_cli.json`、凭据或其他 Skills。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Works** |
| 上游项目 | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| 上游版本 | v2.1.0 |
| 固定源码 | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| 已验证 Skill | `opportunity-solution-tree` |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目 A、全新项目 B、个人级 |
| 最后验证日期 | 2026-09-02 |

三个范围均完成安装、精确发现、目标路径加载、代表能力、回滚和移除后诊断。用户根 `package.json` 与 `codearts_cli.json` 的 SHA-256 在测试前后保持不变。

## 已知限制

只验证了纯文本树；未验证外部研究检索、图形化工具或把结果写入文件。

只验证 Windows 11、CodeArts CLI 26.8.1、`mimo/mimo-v2.5`、固定 Commit 和上述代表流程；未验证 Linux、macOS、其他模型、Claude `/commands` 或完整 PM 插件编排。

## 安全

- 固定 Commit，只复制 `pm-product-discovery/skills/opportunity-solution-tree`；不运行上游 `validate_plugins.py`、测试脚本或其他插件内容。
- 测试提示词提供完整本地事实并禁止网络与文件写入；不需要 `--auto`。
- Skill 会影响 Agent 行为；敏感项目使用前应审阅 `SKILL.md`。
- 安装与卸载不修改凭据、用户根清单或 CodeArts 模型配置。

## 证据与来源

- [2026-09-02 实测记录](../../research/2026-09-02.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
- [固定版本 Skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-product-discovery/skills/opportunity-solution-tree/SKILL.md)
- [上游仓库](https://github.com/phuryn/pm-skills)