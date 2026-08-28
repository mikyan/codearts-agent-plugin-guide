# 在 CodeArts CLI 中使用 tdd

[English](README.en.md)

强制先观察失败测试，再做最小实现并重构。

## 选择安装范围

| 范围 | 目标目录 | 适用场景 |
| --- | --- | --- |
| 项目级 | `<项目>/.codeartsdoer/skills` | 团队共享、随仓库固定版本；推荐默认选择。 |
| 个人级 | `~/.codeartsdoer/skills` | 在多个项目中使用。 |

同名时项目级优先。安装前检查 `tdd`；有冲突就停止，不能覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
请在当前项目安装并验证 Matt Pocock tdd，固定 Commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76。
1. 只修改当前项目 .codeartsdoer/skills；不得修改 ~/.codeartsdoer、codearts_cli.json、package.json 或凭据。
2. 在项目根目录原样执行：$source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"；git clone --filter=blob:none https://github.com/mattpocock/skills.git $source；git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76；if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }。不运行 npm 或上游脚本。
3. 原样执行：$target = Join-Path (Get-Location) ".codeartsdoer\skills"；New-Item -ItemType Directory -Force -Path $target | Out-Null；$names = @("tdd")；if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "同名 Skill 已存在" }；随后逐条执行：Copy-Item -LiteralPath (Join-Path $source "skills\engineering\tdd") -Destination (Join-Path $target "tdd") -Recurse。源到目标为：skills/engineering/tdd -> .codeartsdoer/skills/tdd。
4. 仅在当前 PowerShell 进程设置 $env:CODEARTS_CLI_AK="local-placeholder" 与 $env:CODEARTS_CLI_SK="local-placeholder"，运行 codearts models；若有多个外部 provider/model ID，先让我选择，不把占位值写入文件。
5. 运行 codearts debug skill，确认 tdd 的 location 均在当前项目。
6. 原样运行：codearts run --auto -m <外部模型ID> --format json "Use the tdd skill. Implement clamp in clamp.js with strict red-green-refactor using node --test. Show the failing test first, then make it pass. Do not add dependencies."
7. 只有 JSON 出现 completed Skill "tdd" 事件，且同一测试先失败后通过才通过。验证前在隔离目录准备 clamp.js 与 clamp.test.js；不得使用真实凭据或生产数据。
8. 报告准确文件和事件。卸载只能删除 .codeartsdoer/skills/tdd；不得删除整个 skills 目录、其他 Skill 或任何用户文件。
```

### 个人级提示词

```text
请为当前 Windows 用户安装并验证个人级 Matt Pocock tdd，固定 Commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76。
1. 安装根只能是 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"；不得修改任何项目 .codeartsdoer、$userRoot/package.json、codearts_cli.json 或凭据。
2. 在没有项目级同名 Skill 的空目录原样执行：$source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"；git clone --filter=blob:none https://github.com/mattpocock/skills.git $source；git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76；if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }。不运行 npm 或上游脚本。
3. 原样执行：$target = Join-Path $userRoot "skills"；New-Item -ItemType Directory -Force -Path $target | Out-Null；$names = @("tdd")；if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "同名 Skill 已存在" }；随后逐条执行：Copy-Item -LiteralPath (Join-Path $source "skills\engineering\tdd") -Destination (Join-Path $target "tdd") -Recurse。源到目标为：skills/engineering/tdd -> $userRoot/skills/tdd。
4. 原样执行 $env:CODEARTS_CLI_AK="local-placeholder"；$env:CODEARTS_CLI_SK="local-placeholder"；codearts models。若有多个外部 provider/model ID，先让我选择，绝不持久化。
5. 从上述空目录运行 codearts debug skill，确认 location 位于 $userRoot/skills。
6. 原样运行：codearts run --auto -m <外部模型ID> --format json "Use the tdd skill. Implement clamp in clamp.js with strict red-green-refactor using node --test. Show the failing test first, then make it pass. Do not add dependencies."
7. 通过标准：completed Skill "tdd"，且同一测试先失败后通过。验证前在隔离目录准备 clamp.js 与 clamp.test.js；不得使用真实凭据或生产数据。
8. 卸载只能删除 $userRoot/skills/tdd；不得删除 $userRoot/skills、package.json、codearts_cli.json 或其他 Skill。
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
$names = @("tdd")
if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "同名 Skill 已存在" }
Copy-Item -LiteralPath (Join-Path $source "skills\engineering\tdd") -Destination (Join-Path $target "tdd") -Recurse
```

上游是纯内容包；固定版本没有依赖安装或生命周期脚本。验证前在隔离目录准备 clamp.js 与 clamp.test.js；不得使用真实凭据或生产数据。

## CodeArts 模型与环境配置

使用已配置的外部模型，例如本次验证的 `mimo/mimo-v2.5`：

```powershell
$env:CODEARTS_CLI_AK = "local-placeholder"
$env:CODEARTS_CLI_SK = "local-placeholder"
codearts debug skill
codearts run --auto -m "mimo/mimo-v2.5" --format json "Use the tdd skill. Implement clamp in clamp.js with strict red-green-refactor using node --test. Show the failing test first, then make it pass. Do not add dependencies."
Remove-Item Env:CODEARTS_CLI_AK,Env:CODEARTS_CLI_SK -ErrorAction SilentlyContinue
```

占位值只用于通过 CLI 的本地环境变量检查；模型鉴权来自所选外部模型配置。不要持久化或把真实 AK/SK 写进命令历史。

## 验证与成功判据

必须同时看到目标 location、completed Skill "tdd"，以及同一测试先失败后通过。没有 completed `skill` 事件时，即使文本看起来正确也不算通过。

## 使用

```text
Use the tdd skill. Implement clamp in clamp.js with strict red-green-refactor using node --test. Show the failing test first, then make it pass. Do not add dependencies.
```

## 更新

审查新的 tag/Commit 与目录内容后，先在隔离项目重跑发现、核心调用、第二环境和回滚，再替换固定目录；不要直接跟随 `main`。

## 卸载

只删除所选范围的 `tdd` 目录，保留父级 `skills`、其他 Skill、`package.json` 和 `codearts_cli.json`。删除前解析并核对精确绝对路径，删除后重跑 `codearts debug skill`。

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

验证覆盖上述代表流程，不代表所有输入与长会话。上游 frontmatter 的宿主专用键不作为断言；使用时应明确要求调用 Skill。写文件或执行测试的流程需要用户审阅并授予相应目录权限。

## 安全

固定提交为纯内容复制；安装前仍应审阅全部目标目录。占位 AK/SK 不是真实凭据，只允许进程内使用。项目级会随仓库影响协作者，个人级会影响当前用户所有无同名项目覆盖的会话。

## 证据与来源

- [2026-08-28 实测记录](../../research/2026-08-28.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [上游固定提交](https://github.com/mattpocock/skills/tree/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/tdd)
