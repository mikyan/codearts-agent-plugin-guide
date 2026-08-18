# 在 CodeArts CLI 中使用 GitHub Commit Message Storyteller

[English](README.en.md)

从 GitHub Awesome Copilot 安装经过验证的 `commit-message-storyteller` Skill，生成说明变更原因的 Conventional Commit 信息。

## 选择安装范围

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 随仓库固定版本、团队共享或只在一个项目使用。默认推荐。 |
| 个人级 | `~/.codeartsdoer` | 当前用户需要在多个项目中反复使用。 |

CodeArts 同名 Skill 以项目级为优先。不要在两个范围重复安装同一版本，除非有意让项目覆盖个人配置。

## 让 Agent 帮你安装

### 项目级安装提示词

```text
请在当前项目中为 CodeArts CLI 安装并验证 github/awesome-copilot 的 commit-message-storyteller Skill，源码固定到 Commit 318066d2213b510e89b500ed0d53506c54093ddc。

严格执行：
1. 只修改当前项目的 .codeartsdoer；不要修改 ~/.codeartsdoer、凭据或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；模型不明确时先询问我选择 provider/model ID。
3. 检查 .codeartsdoer/vendor/github-awesome-copilot 和 .codeartsdoer/skills/commit-message-storyteller。任一存在就停止并报告，不得覆盖。
4. 在项目根目录的 PowerShell 原样执行：
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\github-awesome-copilot"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\commit-message-storyteller"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/github/awesome-copilot.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/commit-message-storyteller"
   git -C $source checkout --detach "318066d2213b510e89b500ed0d53506c54093ddc"
   if ((git -C $source rev-parse HEAD).Trim() -ne "318066d2213b510e89b500ed0d53506c54093ddc") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\commit-message-storyteller") -Destination $target -Recurse
5. 运行 codearts debug skill，确认 commit-message-storyteller 的 location 位于当前项目 .codeartsdoer/skills/commit-message-storyteller/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步确认的模型，然后原样执行：
   codearts run --format json --sandbox --model "<选择的模型>" "Call the skill tool exactly once with name commit-message-storyteller. Do not use any other tool. Write one Conventional Commit message for this change: a request cache was recreated for every request, causing repeated work and latency; the cache is now kept at module scope and reused. Output only the commit message."
7. 通过标准：JSON 中必须只有一次成功的 `commit-message-storyteller` Skill 调用；最终提交标题使用 `perf` 或 `fix` 类型，正文同时解释缓存、重复工作或延迟以及复用原因。允许用代码块包裹提交信息。
8. 报告 Commit、修改路径、工具事件、最终结果和卸载清单。任一步失败就如实停止，不得宣称成功。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装并验证个人级 github/awesome-copilot commit-message-storyteller Skill，源码固定到 Commit 318066d2213b510e89b500ed0d53506c54093ddc。

严格执行：
1. 在 PowerShell 执行 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"。只修改该目录下 vendor/github-awesome-copilot 和 skills/commit-message-storyteller；不要修改 $userRoot/package.json、codearts_cli.json、凭据、项目配置或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；模型不明确时先询问我选择 provider/model ID。
3. 检查 $userRoot/vendor/github-awesome-copilot 和 $userRoot/skills/commit-message-storyteller。任一存在就停止并报告，不得覆盖。
4. 在同一个 PowerShell 会话原样执行：
   $source = Join-Path $userRoot "vendor\github-awesome-copilot"
   $target = Join-Path $userRoot "skills\commit-message-storyteller"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/github/awesome-copilot.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/commit-message-storyteller"
   git -C $source checkout --detach "318066d2213b510e89b500ed0d53506c54093ddc"
   if ((git -C $source rev-parse HEAD).Trim() -ne "318066d2213b510e89b500ed0d53506c54093ddc") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\commit-message-storyteller") -Destination $target -Recurse
5. 在没有项目级同名 Skill 的目录运行 codearts debug skill，确认 commit-message-storyteller 的 location 位于当前用户 .codeartsdoer/skills/commit-message-storyteller/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步确认的模型，在同一目录原样执行：
   codearts run --format json --sandbox --model "<选择的模型>" "Call the skill tool exactly once with name commit-message-storyteller. Do not use any other tool. Write one Conventional Commit message for this change: a request cache was recreated for every request, causing repeated work and latency; the cache is now kept at module scope and reused. Output only the commit message."
7. 通过标准：JSON 中必须只有一次成功的 `commit-message-storyteller` Skill 调用；最终提交标题使用 `perf` 或 `fix` 类型，正文同时解释缓存、重复工作或延迟以及复用原因。允许用代码块包裹提交信息。
8. 卸载只能移除 $userRoot/skills/commit-message-storyteller 和 $userRoot/vendor/github-awesome-copilot。报告工具事件与结果；任一步失败不得宣称成功。
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
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\github-awesome-copilot"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\commit-message-storyteller"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/github/awesome-copilot.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/commit-message-storyteller"
git -C $source checkout --detach "318066d2213b510e89b500ed0d53506c54093ddc"
if ((git -C $source rev-parse HEAD).Trim() -ne "318066d2213b510e89b500ed0d53506c54093ddc") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\commit-message-storyteller") -Destination $target -Recurse
```

### 个人级

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\github-awesome-copilot"
$target = Join-Path $userRoot "skills\commit-message-storyteller"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/github/awesome-copilot.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/commit-message-storyteller"
git -C $source checkout --detach "318066d2213b510e89b500ed0d53506c54093ddc"
if ((git -C $source rev-parse HEAD).Trim() -ne "318066d2213b510e89b500ed0d53506c54093ddc") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\commit-message-storyteller") -Destination $target -Recurse
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
$skills | Where-Object { $_.name -eq "commit-message-storyteller" } | Select-Object name, location
```

再进行真实调用；替换模型 ID：

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name commit-message-storyteller. Do not use any other tool. Write one Conventional Commit message for this change: a request cache was recreated for every request, causing repeated work and latency; the cache is now kept at module scope and reused. Output only the commit message."
```

JSON 中必须只有一次成功的 `commit-message-storyteller` Skill 调用；最终提交标题使用 `perf` 或 `fix` 类型，正文同时解释缓存、重复工作或延迟以及复用原因。允许用代码块包裹提交信息。 只有最终文本但没有成功的 `skill` 工具事件，不能视为通过。

## 使用

```text
Call the skill tool with name commit-message-storyteller, inspect the staged diff, and propose a Conventional Commit message without committing.
```

## 更新与卸载

更新时先审查新 Commit，再移除所选范围内的旧 Skill 和专用 `vendor/github-awesome-copilot`，按相同步骤重新安装并复测。不要复用浮动的 `main` 作为已验证版本。

项目级卸载只移除：

- `.codeartsdoer/skills/commit-message-storyteller`
- `.codeartsdoer/vendor/github-awesome-copilot`

个人级卸载只移除：

- `~/.codeartsdoer/skills/commit-message-storyteller`
- `~/.codeartsdoer/vendor/github-awesome-copilot`

不要删除 CodeArts 用户根 `package.json`、`codearts_cli.json` 或其他 Skills。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Works** |
| 上游项目 | [github/awesome-copilot](https://github.com/github/awesome-copilot) |
| 固定源码 | [318066d](https://github.com/github/awesome-copilot/commit/318066d2213b510e89b500ed0d53506c54093ddc) |
| 已验证 Skill | `commit-message-storyteller` |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目级、个人级 |
| 最后验证日期 | 2026-08-19 |

该安装在两个全新项目中复现，随后又从没有项目配置的目录验证个人级加载。真实 CodeArts 会话成功调用 `commit-message-storyteller` 并完成上述代表任务；三个范围均完成回滚，回滚后 Skill 不再被发现。

只验证了 `commit-message-storyteller`；Awesome Copilot 仓库中的其余 400 多个 Skills、Agents、Hooks、MCP 和扩展未验证。

## 安全说明

- 只复制固定 Commit 下的所选 Skill 目录；不运行仓库中的其他代码。
- 安装前审阅 `SKILL.md` 及随目录复制的资源。
- Skill 指令会影响 Agent 行为；敏感仓库中使用前应先审阅。
- 本流程不需要 `--auto`，也不读取或写入凭据。

## 证据与来源

- [2026-08-19 批量实测记录](../../research/2026-08-19.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [固定版本 Skill](https://github.com/github/awesome-copilot/blob/318066d2213b510e89b500ed0d53506c54093ddc/skills/commit-message-storyteller/SKILL.md)
- [上游仓库](https://github.com/github/awesome-copilot)
