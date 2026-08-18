# 在 CodeArts CLI 中使用 PM Prioritization Frameworks

[English](README.en.md)

从 PM Skills 安装经过验证的 `prioritization-frameworks` Skill，用 RICE、ICE、Kano 等框架进行产品优先级分析。

## 选择安装范围

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 随仓库固定版本、团队共享或只在一个项目使用。默认推荐。 |
| 个人级 | `~/.codeartsdoer` | 当前用户需要在多个项目中反复使用。 |

CodeArts 同名 Skill 以项目级为优先。不要在两个范围重复安装同一版本，除非有意让项目覆盖个人配置。

## 让 Agent 帮你安装

### 项目级安装提示词

```text
请在当前项目中为 CodeArts CLI 安装并验证 phuryn/pm-skills 的 prioritization-frameworks Skill，源码固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 只修改当前项目的 .codeartsdoer；不要修改 ~/.codeartsdoer、凭据或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；模型不明确时先询问我选择 provider/model ID。
3. 检查 .codeartsdoer/vendor/pm-skills 和 .codeartsdoer/skills/prioritization-frameworks。任一存在就停止并报告，不得覆盖。
4. 在项目根目录的 PowerShell 原样执行：
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\prioritization-frameworks"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/prioritization-frameworks"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\prioritization-frameworks") -Destination $target -Recurse
5. 运行 codearts debug skill，确认 prioritization-frameworks 的 location 位于当前项目 .codeartsdoer/skills/prioritization-frameworks/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步确认的模型，然后原样执行：
   codearts run --format json --sandbox --model "<选择的模型>" "Call the skill tool exactly once with name prioritization-frameworks. Do not use any other tool. Calculate the RICE score for Reach 100, Impact 2, Confidence 0.5, Effort 4. Output only the numeric result."
7. 通过标准：JSON 中必须只有一次成功的 `prioritization-frameworks` Skill 调用，最终结果必须为 `25`。
8. 报告 Commit、修改路径、工具事件、最终结果和卸载清单。任一步失败就如实停止，不得宣称成功。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装并验证个人级 phuryn/pm-skills prioritization-frameworks Skill，源码固定到 Commit 18468a95b427e70e258b51389796367c6f684e7d。

严格执行：
1. 在 PowerShell 执行 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"。只修改该目录下 vendor/pm-skills 和 pm-execution/skills/prioritization-frameworks；不要修改 $userRoot/package.json、codearts_cli.json、凭据、项目配置或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；模型不明确时先询问我选择 provider/model ID。
3. 检查 $userRoot/vendor/pm-skills 和 $userRoot/pm-execution/skills/prioritization-frameworks。任一存在就停止并报告，不得覆盖。
4. 在同一个 PowerShell 会话原样执行：
   $source = Join-Path $userRoot "vendor\pm-skills"
   $target = Join-Path $userRoot "skills\prioritization-frameworks"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/prioritization-frameworks"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\prioritization-frameworks") -Destination $target -Recurse
5. 在没有项目级同名 Skill 的目录运行 codearts debug skill，确认 prioritization-frameworks 的 location 位于当前用户 .codeartsdoer/skills/prioritization-frameworks/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步确认的模型，在同一目录原样执行：
   codearts run --format json --sandbox --model "<选择的模型>" "Call the skill tool exactly once with name prioritization-frameworks. Do not use any other tool. Calculate the RICE score for Reach 100, Impact 2, Confidence 0.5, Effort 4. Output only the numeric result."
7. 通过标准：JSON 中必须只有一次成功的 `prioritization-frameworks` Skill 调用，最终结果必须为 `25`。
8. 卸载只能移除 $userRoot/pm-execution/skills/prioritization-frameworks 和 $userRoot/vendor/pm-skills。报告工具事件与结果；任一步失败不得宣称成功。
```

## Windows 手动安装

### 前置条件

安装 CodeArts CLI 和 Git，并确认模型可用：

```powershell
codearts --version
git --version
codearts models
```

### 项目级

在目标项目根目录执行：

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\prioritization-frameworks"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-execution/skills/prioritization-frameworks"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\prioritization-frameworks") -Destination $target -Recurse
```

### 个人级

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills"
$target = Join-Path $userRoot "skills\prioritization-frameworks"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-execution/skills/prioritization-frameworks"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\prioritization-frameworks") -Destination $target -Recurse
```

两种范围都不修改 CodeArts 模型配置，也不执行第三方安装脚本。

## CodeArts 配置

本 Skill 不需要修改 `codearts_cli.json`。如果尚未安装 CodeArts CLI，先按[官方安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)完成安装；然后列出当前可用模型：

```powershell
codearts models
```

在后续命令中把 `mimo/mimo-v2.5` 替换为列表里的实际 `provider/model` ID。需要自定义模型时，参考[官方配置示例](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html)；API Key 只保留在本地配置或环境变量中，不要写入本项目。

CodeArts CLI 26.8.1 在本次自定义 Provider 测试中仍要求当前进程存在 `CODEARTS_CLI_AK` 和 `CODEARTS_CLI_SK`。本次使用非秘密占位值即可通过前置检查，实际模型鉴权使用 Provider 自己的 API Key；这是实测现象，不是兼容保证。使用华为云托管模型时应按[官方 AK/SK 说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html)配置有效凭据。

## 验证

先检查来源路径：

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills | Where-Object { $_.name -eq "prioritization-frameworks" } | Select-Object name, location
```

再进行真实调用；替换模型 ID：

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name prioritization-frameworks. Do not use any other tool. Calculate the RICE score for Reach 100, Impact 2, Confidence 0.5, Effort 4. Output only the numeric result."
```

JSON 中必须只有一次成功的 `prioritization-frameworks` Skill 调用，最终结果必须为 `25`。 只有最终文本但没有成功的 `skill` 工具事件，不能视为通过。

## 使用

```text
Call the skill tool with name prioritization-frameworks, choose the best framework for these product decisions, and show the calculation.
```

## 更新与卸载

更新时先审查新 Commit，再移除所选范围内的旧 Skill 和专用 `vendor/pm-skills`，按相同步骤重新安装并复测。不要复用浮动的 `main` 作为已验证版本。

项目级卸载只移除：

- `.codeartsdoer/skills/prioritization-frameworks`
- `.codeartsdoer/vendor/pm-skills`

个人级卸载只移除：

- `~/.codeartsdoer/skills/prioritization-frameworks`
- `~/.codeartsdoer/vendor/pm-skills`

不要删除 CodeArts 用户根 `package.json`、`codearts_cli.json` 或其他 Skills。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Works** |
| 上游项目 | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| 固定源码 | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| 已验证 Skill | `prioritization-frameworks` |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目级、个人级 |
| 最后验证日期 | 2026-08-19 |

该安装在两个全新项目中复现，随后又从没有项目配置的目录验证个人级加载。真实 CodeArts 会话成功调用 `prioritization-frameworks` 并完成上述代表任务；三个范围均完成回滚，回滚后 Skill 不再被发现。

只验证了 `prioritization-frameworks` 和 RICE 计算；外部 Google 模板链接、PM Skills 的其他插件与 Skills 未验证。

## 安全说明

- 只复制固定 Commit 下的所选 Skill 目录；不运行仓库中的其他代码。
- 安装前审阅 `SKILL.md` 及随目录复制的资源。
- Skill 指令会影响 Agent 行为；敏感仓库中使用前应先审阅。
- 本流程不需要 `--auto`，也不读取或写入凭据。

## 证据与来源

- [2026-08-19 批量实测记录](../../research/2026-08-19.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [固定版本 Skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-execution/skills/prioritization-frameworks/SKILL.md)
- [上游仓库](https://github.com/phuryn/pm-skills)
