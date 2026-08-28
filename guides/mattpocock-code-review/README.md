# 在 CodeArts CLI 中使用 code-review

[English](README.en.md)

把 Git 差异分成“仓库标准”和“需求规格”两个互不遮蔽的审查轴。

## 选择安装范围

| 范围 | 目标目录 | 适用场景 |
| --- | --- | --- |
| 项目级 | `<项目>/.codeartsdoer/skills` | 团队共享、随仓库固定版本；推荐默认选择。 |
| 个人级 | `~/.codeartsdoer/skills` | 在多个项目中使用。 |

同名时项目级优先。安装前检查 `code-review`；有冲突就停止，不能覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
请在当前项目安装并验证 Matt Pocock code-review，固定 Commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76。
1. 只修改当前项目 .codeartsdoer/skills；不得修改 ~/.codeartsdoer、codearts_cli.json、package.json 或凭据。
2. 在项目根目录原样执行：$source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"；git clone --filter=blob:none https://github.com/mattpocock/skills.git $source；git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76；if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }。不运行 npm 或上游脚本。
3. 原样执行：$target = Join-Path (Get-Location) ".codeartsdoer\skills"；New-Item -ItemType Directory -Force -Path $target | Out-Null；$names = @("code-review")；if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "同名 Skill 已存在" }；随后逐条执行：Copy-Item -LiteralPath (Join-Path $source "skills\engineering\code-review") -Destination (Join-Path $target "code-review") -Recurse。源到目标为：skills/engineering/code-review -> .codeartsdoer/skills/code-review。
4. 在安装范围对应的隔离工作目录，逐行原样执行以下完整夹具；先创建夹具，再运行 CodeArts：
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
@'
# Standards

Production code must not call console.log.
'@ | Set-Content CONTRIBUTING.md
@'
# Greeting spec

The greet(name) function returns exactly Hello, <name> and produces no logging side effect.
'@ | Set-Content SPEC.md
Set-Content greet.js 'export function greet(name) { return "Hello, " + name; }'
git add .
git commit -m "baseline greeting"
Set-Content greet.js 'export function greet(name) { console.log(name); return "Hi, " + name; }'
git add greet.js
git commit -m "change greeting output"
5. 仅在当前 PowerShell 进程设置 $env:CODEARTS_CLI_AK="local-placeholder" 与 $env:CODEARTS_CLI_SK="local-placeholder"，运行 codearts models；若有多个外部 provider/model ID，先让我选择，不把占位值写入文件。
6. 运行 codearts debug skill，确认 code-review 的 location 均在当前项目。
7. 原样运行：codearts run -m <外部模型ID> --format json "Explicitly use the code-review skill to review HEAD against fixed point HEAD~1. The spec source is SPEC.md and the standards source is CONTRIBUTING.md. Run the required git diff and git log checks. Report the Standards and Spec axes separately, including the documented console.log violation and the greeting mismatch. Do not modify files."
8. 只有 JSON 出现 completed Skill "code-review" 事件，且至少两个 completed task 事件，且双轴报告同时指出 console.log 和 greeting 回归才通过。
9. 报告准确文件和事件。卸载必须删除 .codeartsdoer/skills/code-review；若源码 checkout 是本次创建且未共享，也删除精确的 .tmp/mattpocock-skills-6654f6b。不得删除整个 skills 或 .tmp 父目录、其他 Skill、任何用户文件、codearts_cli.json、package.json 或凭据。
```

### 个人级提示词

```text
请为当前 Windows 用户安装并验证个人级 Matt Pocock code-review，固定 Commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76。
1. 安装根只能是 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"；不得修改任何项目 .codeartsdoer、$userRoot/package.json、codearts_cli.json 或凭据。
2. 在没有项目级同名 Skill 的空目录原样执行：$source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"；git clone --filter=blob:none https://github.com/mattpocock/skills.git $source；git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76；if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }。不运行 npm 或上游脚本。
3. 原样执行：$target = Join-Path $userRoot "skills"；New-Item -ItemType Directory -Force -Path $target | Out-Null；$names = @("code-review")；if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "同名 Skill 已存在" }；随后逐条执行：Copy-Item -LiteralPath (Join-Path $source "skills\engineering\code-review") -Destination (Join-Path $target "code-review") -Recurse。源到目标为：skills/engineering/code-review -> $userRoot/skills/code-review。
4. 在上述无项目覆盖的隔离目录逐行原样执行以下完整夹具；先创建夹具，再运行 CodeArts：
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
@'
# Standards

Production code must not call console.log.
'@ | Set-Content CONTRIBUTING.md
@'
# Greeting spec

The greet(name) function returns exactly Hello, <name> and produces no logging side effect.
'@ | Set-Content SPEC.md
Set-Content greet.js 'export function greet(name) { return "Hello, " + name; }'
git add .
git commit -m "baseline greeting"
Set-Content greet.js 'export function greet(name) { console.log(name); return "Hi, " + name; }'
git add greet.js
git commit -m "change greeting output"
5. 原样执行 $env:CODEARTS_CLI_AK="local-placeholder"；$env:CODEARTS_CLI_SK="local-placeholder"；codearts models。若有多个外部 provider/model ID，先让我选择，绝不持久化。
6. 从上述空目录运行 codearts debug skill，确认 location 位于 $userRoot/skills。
7. 原样运行：codearts run -m <外部模型ID> --format json "Explicitly use the code-review skill to review HEAD against fixed point HEAD~1. The spec source is SPEC.md and the standards source is CONTRIBUTING.md. Run the required git diff and git log checks. Report the Standards and Spec axes separately, including the documented console.log violation and the greeting mismatch. Do not modify files."
8. 通过标准：completed Skill "code-review"，且至少两个 completed task 事件，且双轴报告同时指出 console.log 和 greeting 回归。
9. 卸载必须删除 $userRoot/skills/code-review；若源码 checkout 是本次创建且未共享，也删除隔离目录精确的 .tmp/mattpocock-skills-6654f6b。不得删除 $userRoot/skills 或 .tmp 父目录、package.json、codearts_cli.json、其他 Skill 或凭据。
```

## 手动安装

在所选范围的干净工作目录执行：

```powershell
$source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"
git clone --filter=blob:none https://github.com/mattpocock/skills.git $source
git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76
if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }
$projectTarget = Join-Path (Get-Location) ".codeartsdoer\skills"
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$userTarget = Join-Path $userRoot "skills"
$target = $projectTarget # 项目级；选择个人级时本行改为 $target = $userTarget
New-Item -ItemType Directory -Force -Path $target | Out-Null
$names = @("code-review")
if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "同名 Skill 已存在" }
Copy-Item -LiteralPath (Join-Path $source "skills\engineering\code-review") -Destination (Join-Path $target "code-review") -Recurse
```

上游是纯内容包；固定版本没有依赖安装或生命周期脚本。

验证前在隔离目录按 Agent 提示词中的完整内容建立夹具：

```powershell
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
@'
# Standards

Production code must not call console.log.
'@ | Set-Content CONTRIBUTING.md
@'
# Greeting spec

The greet(name) function returns exactly Hello, <name> and produces no logging side effect.
'@ | Set-Content SPEC.md
Set-Content greet.js 'export function greet(name) { return "Hello, " + name; }'
git add .
git commit -m "baseline greeting"
Set-Content greet.js 'export function greet(name) { console.log(name); return "Hi, " + name; }'
git add greet.js
git commit -m "change greeting output"
```

## CodeArts 模型与环境配置

使用已配置的外部模型，例如本次验证的 `mimo/mimo-v2.5`：

```powershell
$env:CODEARTS_CLI_AK = "local-placeholder"
$env:CODEARTS_CLI_SK = "local-placeholder"
codearts debug skill
codearts run -m "mimo/mimo-v2.5" --format json "Explicitly use the code-review skill to review HEAD against fixed point HEAD~1. The spec source is SPEC.md and the standards source is CONTRIBUTING.md. Run the required git diff and git log checks. Report the Standards and Spec axes separately, including the documented console.log violation and the greeting mismatch. Do not modify files."
Remove-Item Env:CODEARTS_CLI_AK,Env:CODEARTS_CLI_SK -ErrorAction SilentlyContinue
```

占位值只用于通过 CLI 的本地环境变量检查；模型鉴权来自所选外部模型配置。不要持久化或把真实 AK/SK 写进命令历史。

## 验证与成功判据

必须同时看到目标 location、completed Skill "code-review"，以及至少两个 completed task 事件，且双轴报告同时指出 console.log 和 greeting 回归。没有 completed `skill` 事件时，即使文本看起来正确也不算通过。

## 使用

```text
Explicitly use the code-review skill to review HEAD against fixed point HEAD~1. The spec source is SPEC.md and the standards source is CONTRIBUTING.md. Run the required git diff and git log checks. Report the Standards and Spec axes separately, including the documented console.log violation and the greeting mismatch. Do not modify files.
```

## 更新

审查新的 tag/Commit 与目录内容后，先在隔离项目重跑发现、核心调用、第二环境和回滚，再替换固定目录；不要直接跟随 `main`。

## 卸载

只删除所选范围的 `code-review` 目录，保留父级 `skills`、其他 Skill、`package.json` 和 `codearts_cli.json`。删除前解析并核对精确绝对路径，删除后重跑 `codearts debug skill`。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 状态 | **Works** |
| 上游 | [mattpocock/skills](https://github.com/mattpocock/skills) |
| Release / Commit | v1.2.3 后的 `6654f6b60cd9d5be8b54c6fafe44346dabeb3b76` |
| 许可证 | MIT |
| CodeArts / 系统 | CLI 26.8.1 / Windows 11 |
| 模型 | `mimo/mimo-v2.5` |
| 范围 | 项目级（两个隔离项目）与个人级 |
| 日期 | 2026-08-28 |

## 已知限制

需要 Git 仓库、可解析的固定点及明确的标准/规格来源。三次运行各出现 2–4 个 completed `task` 子代理事件，因此比单模型审查更慢；没有验证远程 PR 或真实 issue tracker。

## 安全

固定提交为纯内容复制；安装前仍应审阅全部目标目录。占位 AK/SK 不是真实凭据，只允许进程内使用。项目级会随仓库影响协作者，个人级会影响当前用户所有无同名项目覆盖的会话。

## 证据与来源

- [2026-08-28 实测记录](../../research/2026-08-28.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [上游固定提交](https://github.com/mattpocock/skills/tree/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/code-review)
