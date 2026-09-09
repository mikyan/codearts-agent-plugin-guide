# 在 CodeArts CLI 中使用 PM 虚拟数据集生成

[English](README.en.md)

从 PM Skills 安装经过验证的 `dummy-dataset` Skill，按列、格式与业务约束生成可直接使用的合成测试数据。

## 选择安装范围

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 随仓库固定版本、团队共享或只在一个项目使用。默认推荐。 |
| 个人级 | `~/.codeartsdoer` | 当前用户需要在多个项目重复生成测试数据。 |

CodeArts 官方文档规定同名 Skill 以项目级为优先。不要在两个范围重复安装；安装前发现同名 Skill 或 vendor 目录必须停止，不能覆盖。

## 让 Agent 帮你安装

以下提示词包含已验证的安装、隔离写权限、真实调用和回滚边界。验证会生成 `dummy-commute-tasks.csv`；卸载 Skill 时不要顺带删除用户生成的数据文件。

### 项目级安装提示词

```text
请在当前项目为 CodeArts CLI 安装并验证 phuryn/pm-skills 的 dummy-dataset Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 只安装到当前项目 .codeartsdoer/vendor/pm-skills-dummy-dataset 与 .codeartsdoer/skills/dummy-dataset。不得修改 ~/.codeartsdoer、凭据、项目根 package.json、用户根 package.json 或 codearts_cli.json。
2. 在项目根目录运行 codearts --version、git --version 和 codearts models；如果有多个模型，先让我选择准确的 provider/model ID。
3. 检查上述两个安装目标以及项目根 dummy-commute-tasks.csv；任一存在就停止并报告，不得覆盖。
4. 在项目根目录的 PowerShell 原样执行：
   $projectRoot = (Get-Location).Path
   $source = Join-Path $projectRoot ".codeartsdoer\vendor\pm-skills-dummy-dataset"
   $target = Join-Path $projectRoot ".codeartsdoer\skills\dummy-dataset"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/dummy-dataset"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\dummy-dataset") -Destination $target -Recurse
5. 运行 codearts debug skill，确认 dummy-dataset 的 location 是当前项目 .codeartsdoer/skills/dummy-dataset/SKILL.md。
6. 为非交互写入建立一次性权限目录；不要改用户持久 permission/global.json：
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-dummy-dataset-project-verify"
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
7. 把 <选择的模型> 替换为第 2 步确认的模型。仍在项目根目录，用已安装 CodeArts 26.8.1 的真实可执行文件运行：
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
   $prompt = "Call the skill tool exactly once with name dummy-dataset, then use the write tool exactly once to create dummy-commute-tasks.csv in the current workspace. Use no other tool, do not access the network, and do not read or execute files. Generate exactly 6 synthetic task records plus one CSV header. Columns must be task_id,title,commute_minutes,due_in_hours,needs_review,device. Use IDs T001 through T006 exactly once. commute_minutes must be integers from 5 through 60. due_in_hours must be an integer from 0 through 48 or blank. needs_review must be true exactly when due_in_hours is blank. device must be phone or laptop. Include at least one blank due_in_hours, at least one phone, and at least one laptop. Do not use real names, emails, or personal data. Finish by stating only that dummy-commute-tasks.csv was created."
   & $codeartsExe run -m "<选择的模型>" --auto --format json $prompt
8. 通过标准：必须恰好出现一次完成的 dummy-dataset Skill 事件和一次完成的 write 事件；Skill 来源必须是第 4 步目标；CSV 必须有一个表头与六条 T001-T006 记录，并满足全部类型、范围、联动和无真实个人数据约束。只有文件或只有最终文本都不算通过。
9. 记录事件与结果后，只删除 $verifyRoot；不得删除项目根 dummy-commute-tasks.csv。报告 Commit、源目录、目标目录、完成事件、来源路径与约束检查。任一步失败必须停止，不得宣称成功。
10. 卸载只能删除 .codeartsdoer/skills/dummy-dataset 与 .codeartsdoer/vendor/pm-skills-dummy-dataset；不得删除 .codeartsdoer 本身、任何 package.json、codearts_cli.json、其他 Skill 或已生成的数据文件。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装并验证个人级 phuryn/pm-skills dummy-dataset Skill，固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 设置 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"。只安装到 $userRoot/vendor/pm-skills-dummy-dataset 与 $userRoot/skills/dummy-dataset；不得修改用户根 package.json、codearts_cli.json、持久 permission/global.json、凭据、项目配置或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；如果有多个模型，先让我选择准确的 provider/model ID。
3. 检查两个安装目标；任一存在就停止并报告，不得覆盖。
4. 在 PowerShell 原样执行：
   $source = Join-Path $userRoot "vendor\pm-skills-dummy-dataset"
   $target = Join-Path $userRoot "skills\dummy-dataset"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/dummy-dataset"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\dummy-dataset") -Destination $target -Recurse
5. 创建无项目级覆盖的隔离 workspace 和完整临时权限：
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-dummy-dataset-user-verify"
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
   if (Test-Path -LiteralPath (Join-Path $workspace ".codeartsdoer\skills\dummy-dataset")) { throw "Project override exists." }
6. 切换到 $workspace，运行 codearts debug skill，确认 dummy-dataset 的 location 是 $userRoot/skills/dummy-dataset/SKILL.md，而不是项目路径。
7. 在该 workspace，把 <选择的模型> 替换为第 2 步模型，并原样执行：
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
   $prompt = "Call the skill tool exactly once with name dummy-dataset, then use the write tool exactly once to create dummy-commute-tasks.csv in the current workspace. Use no other tool, do not access the network, and do not read or execute files. Generate exactly 6 synthetic task records plus one CSV header. Columns must be task_id,title,commute_minutes,due_in_hours,needs_review,device. Use IDs T001 through T006 exactly once. commute_minutes must be integers from 5 through 60. due_in_hours must be an integer from 0 through 48 or blank. needs_review must be true exactly when due_in_hours is blank. device must be phone or laptop. Include at least one blank due_in_hours, at least one phone, and at least one laptop. Do not use real names, emails, or personal data. Finish by stating only that dummy-commute-tasks.csv was created."
   & $codeartsExe run -m "<选择的模型>" --auto --format json $prompt
8. 通过标准与项目级相同：一次完成的目标 Skill、一次完成的 write、准确个人级来源路径、一个表头加六条记录，并满足全部 CSV 约束。记录证据后只删除 $verifyRoot；不要删除其他临时目录或用户文件。
9. 报告 Commit、源目录、目标目录、完成事件、来源路径和约束检查。任一步失败必须停止，不得宣称成功。
10. 卸载只能删除 $userRoot/skills/dummy-dataset 与 $userRoot/vendor/pm-skills-dummy-dataset；不得删除用户根 package.json、codearts_cli.json、持久权限、凭据、其他 Skill 或用户生成的数据文件。
```

## Windows 手动安装

先确认工具与模型：

```powershell
codearts --version
git --version
codearts models
```

项目级在项目根目录执行：

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-dummy-dataset"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\dummy-dataset"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-execution/skills/dummy-dataset"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\dummy-dataset") -Destination $target -Recurse
```

个人级执行：

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-dummy-dataset"
$target = Join-Path $userRoot "skills\dummy-dataset"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-execution/skills/dummy-dataset"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\dummy-dataset") -Destination $target -Recurse
```

两种范围都只复制这个 Skill，不运行上游脚本或依赖。

## CodeArts 配置

Skill 本身不需要修改 `codearts_cli.json`。按[官方安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)安装 CLI，用 `codearts models` 选择真实 `provider/model`，凭据只保存在本地配置或环境变量。

该 Skill 会写文件。交互式使用可按[官方权限说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0006.html)逐次批准；非交互验证中，CLI 26.8.1 会自动拒绝 `ask`，即使带 `--auto`。上面的提示词因此使用可删除的 `KERNEL_DATA_DIR` 和完整临时 `global.json`，没有修改用户持久权限。测试时直接调用 `codearts.exe`，并复用了官方 `codearts.cmd` 的进程环境；`NODE_TLS_REJECT_UNAUTHORIZED=0` 是该版本官方启动器的现状，不应复制到其他工具。

## 验证

先用 `codearts debug skill` 检查准确 location，再运行 Agent 提示词中的完整写入 smoke test。成功必须同时具备完成的 Skill 事件、完成的 Write 事件、准确来源路径和磁盘 CSV 约束。执行结果后可用 `Import-Csv .\dummy-commute-tasks.csv` 检查六行。

## 使用

```text
Call the skill tool with name dummy-dataset. 为订单导入测试生成 50 行 CSV：order_id 唯一，amount 为 1.00-999.99，currency 只能是 HKD/USD，refunded=true 时 refund_reason 不得为空。只使用合成值，不要包含真实个人数据。
```

生成脚本、SQL 或较大数据集时，先审阅输出再执行或导入；不要把“看起来真实”误当成生产数据。

## 更新

先审阅新 Commit 和 `pm-execution/skills/dummy-dataset`。只移除当前范围的旧目标，按固定新 Commit 重装并重复完整验证；不要把浮动 `main` 当成已验证版本。

## 卸载

项目级只移除 `.codeartsdoer/skills/dummy-dataset` 与 `.codeartsdoer/vendor/pm-skills-dummy-dataset`；个人级只移除 `~/.codeartsdoer/skills/dummy-dataset` 与 `~/.codeartsdoer/vendor/pm-skills-dummy-dataset`。不要删除 `.codeartsdoer` 根目录、用户根配置、其他 Skills 或任何已生成数据文件。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Works** |
| 上游项目 | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| 上游版本 | v2.1.0 |
| 固定源码 | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| 已验证 Skill | `dummy-dataset` |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目 A、全新项目 B、个人级 |
| 最后验证日期 | 2026-09-09 |

三个范围都精确发现并从预期目录加载 Skill，只发生一次完成的 Skill 与一次完成的 Write，生成的 CSV 通过行数、字段、范围、联动和无个人数据审计；安装、产物与临时权限均精确回滚。用户根 `package.json`、`codearts_cli.json` 和持久权限文件哈希保持不变。

## 已知限制

只验证了 6 行直接 CSV；未验证 JSON、SQL、Python 生成器、大数据量、关系完整性、统计分布、执行生成脚本或导入数据库。合成数据仍可能带偏见、不满足业务边界或意外类似真实身份，投入测试前必须审阅。

只覆盖 Windows 11、CLI 26.8.1、`mimo/mimo-v2.5`、固定 Commit 和上述隔离写入流程；未验证 Linux、macOS、其他模型、Claude Commands 或完整插件编排。

## 安全

- 固定 Commit，只复制 `pm-execution/skills/dummy-dataset`；无 npm/Python 依赖、生命周期脚本、二进制或遥测。
- 临时权限允许当前测试 workspace 的 Write/Edit，同时明确拒绝 Shell、读取、浏览和外部目录访问；完成后删除精确临时目录。
- 不执行模型生成的 Python、SQL 或 Shell；先人工审阅。
- 不向提示词提供真实个人数据、凭据或生产记录。

## 证据与来源

- [2026-09-09 实测记录](../../research/2026-09-09.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI 权限](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0006.html)
- [固定版本 Skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-execution/skills/dummy-dataset/SKILL.md)
- [上游仓库](https://github.com/phuryn/pm-skills)
