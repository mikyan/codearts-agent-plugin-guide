# 在 CodeArts CLI 中使用 resolving-merge-conflicts

[English](README.en.md)

依据两侧提交意图逐 hunk 解决进行中的 Git 冲突，并完成测试与 merge commit。

## 选择安装范围

| 范围 | 目标目录 | 适用场景 |
| --- | --- | --- |
| 项目级 | `<项目>/.codeartsdoer/skills` | 团队共享、随仓库固定版本；推荐默认选择。 |
| 个人级 | `~/.codeartsdoer/skills` | 在多个项目中使用。 |

同名时项目级优先。安装前检查 `resolving-merge-conflicts`；有冲突就停止，不能覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
请在当前项目安装并验证 Matt Pocock resolving-merge-conflicts，固定 Commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76。
1. 只修改当前项目 .codeartsdoer/skills；不得修改 ~/.codeartsdoer、codearts_cli.json、package.json 或凭据。
2. 在项目根目录原样执行：$source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"；git clone --filter=blob:none https://github.com/mattpocock/skills.git $source；git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76；if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }。不运行 npm 或上游脚本。
3. 原样执行：$target = Join-Path (Get-Location) ".codeartsdoer\skills"；New-Item -ItemType Directory -Force -Path $target | Out-Null；$names = @("resolving-merge-conflicts")；if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "同名 Skill 已存在" }；随后逐条执行：Copy-Item -LiteralPath (Join-Path $source "skills\engineering\resolving-merge-conflicts") -Destination (Join-Path $target "resolving-merge-conflicts") -Recurse。源到目标为：skills/engineering/resolving-merge-conflicts -> .codeartsdoer/skills/resolving-merge-conflicts。
4. 在安装范围对应的隔离工作目录，逐行原样执行以下完整夹具；先创建夹具，再运行 CodeArts：
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
Set-Content package.json '{"type":"module"}'
Set-Content format.js 'export function format(item) { return item.name; }'
git add .
git commit -m "baseline formatter"
git checkout -b feature-uppercase
Set-Content format.js 'export function format(item) { return item.name.toUpperCase(); }'
Set-Content uppercase.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('uppercases name', () => assert.match(format({name:'box',status:'ready'}), /^BOX/));"
git add .
git commit -m "preserve uppercase name intent"
git checkout main
Set-Content format.js "export function format(item) { return item.name + ':' + item.status; }"
Set-Content status.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('includes status suffix', () => assert.match(format({name:'box',status:'ready'}), /:ready$/));"
git add .
git commit -m "preserve status suffix intent"
git merge feature-uppercase
if (-not (Test-Path .git\MERGE_HEAD)) { throw "Expected conflict" }
5. 仅在当前 PowerShell 进程设置 $env:CODEARTS_CLI_AK="local-placeholder" 与 $env:CODEARTS_CLI_SK="local-placeholder"，运行 codearts models；若有多个外部 provider/model ID，先让我选择，不把占位值写入文件。
6. 运行 codearts debug skill，确认 resolving-merge-conflicts 的 location 均在当前项目。
7. 原样运行：codearts run --auto -m <外部模型ID> --format json "Explicitly use the resolving-merge-conflicts skill. This isolated repository is already in an in-progress merge conflict. Inspect both commits as primary sources, resolve format.js so it preserves uppercase-name and status-suffix intents, run node --test, stage the resolution, and finish the merge with a non-interactive commit. Never abort. Do not add dependencies."
8. 只有 JSON 出现 completed Skill "resolving-merge-conflicts" 事件，且node --test 两项通过、无未合并文件、MERGE_HEAD 消失、HEAD 为双亲 merge commit 且 format.js 同时保留两侧意图才通过。
9. 报告准确文件和事件。卸载必须删除 .codeartsdoer/skills/resolving-merge-conflicts；若源码 checkout 是本次创建且未共享，也删除精确的 .tmp/mattpocock-skills-6654f6b。不得删除整个 skills 或 .tmp 父目录、其他 Skill、任何用户文件、codearts_cli.json、package.json 或凭据。
```

### 个人级提示词

```text
请为当前 Windows 用户安装并验证个人级 Matt Pocock resolving-merge-conflicts，固定 Commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76。
1. 安装根只能是 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"；不得修改任何项目 .codeartsdoer、$userRoot/package.json、codearts_cli.json 或凭据。
2. 在没有项目级同名 Skill 的空目录原样执行：$source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"；git clone --filter=blob:none https://github.com/mattpocock/skills.git $source；git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76；if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }。不运行 npm 或上游脚本。
3. 原样执行：$target = Join-Path $userRoot "skills"；New-Item -ItemType Directory -Force -Path $target | Out-Null；$names = @("resolving-merge-conflicts")；if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "同名 Skill 已存在" }；随后逐条执行：Copy-Item -LiteralPath (Join-Path $source "skills\engineering\resolving-merge-conflicts") -Destination (Join-Path $target "resolving-merge-conflicts") -Recurse。源到目标为：skills/engineering/resolving-merge-conflicts -> $userRoot/skills/resolving-merge-conflicts。
4. 在上述无项目覆盖的隔离目录逐行原样执行以下完整夹具；先创建夹具，再运行 CodeArts：
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
Set-Content package.json '{"type":"module"}'
Set-Content format.js 'export function format(item) { return item.name; }'
git add .
git commit -m "baseline formatter"
git checkout -b feature-uppercase
Set-Content format.js 'export function format(item) { return item.name.toUpperCase(); }'
Set-Content uppercase.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('uppercases name', () => assert.match(format({name:'box',status:'ready'}), /^BOX/));"
git add .
git commit -m "preserve uppercase name intent"
git checkout main
Set-Content format.js "export function format(item) { return item.name + ':' + item.status; }"
Set-Content status.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('includes status suffix', () => assert.match(format({name:'box',status:'ready'}), /:ready$/));"
git add .
git commit -m "preserve status suffix intent"
git merge feature-uppercase
if (-not (Test-Path .git\MERGE_HEAD)) { throw "Expected conflict" }
5. 原样执行 $env:CODEARTS_CLI_AK="local-placeholder"；$env:CODEARTS_CLI_SK="local-placeholder"；codearts models。若有多个外部 provider/model ID，先让我选择，绝不持久化。
6. 从上述空目录运行 codearts debug skill，确认 location 位于 $userRoot/skills。
7. 原样运行：codearts run --auto -m <外部模型ID> --format json "Explicitly use the resolving-merge-conflicts skill. This isolated repository is already in an in-progress merge conflict. Inspect both commits as primary sources, resolve format.js so it preserves uppercase-name and status-suffix intents, run node --test, stage the resolution, and finish the merge with a non-interactive commit. Never abort. Do not add dependencies."
8. 通过标准：completed Skill "resolving-merge-conflicts"，且node --test 两项通过、无未合并文件、MERGE_HEAD 消失、HEAD 为双亲 merge commit 且 format.js 同时保留两侧意图。
9. 卸载必须删除 $userRoot/skills/resolving-merge-conflicts；若源码 checkout 是本次创建且未共享，也删除隔离目录精确的 .tmp/mattpocock-skills-6654f6b。不得删除 $userRoot/skills 或 .tmp 父目录、package.json、codearts_cli.json、其他 Skill 或凭据。
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
$names = @("resolving-merge-conflicts")
if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "同名 Skill 已存在" }
Copy-Item -LiteralPath (Join-Path $source "skills\engineering\resolving-merge-conflicts") -Destination (Join-Path $target "resolving-merge-conflicts") -Recurse
```

上游是纯内容包；固定版本没有依赖安装或生命周期脚本。

验证前在隔离目录按 Agent 提示词中的完整内容建立夹具：

```powershell
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
Set-Content package.json '{"type":"module"}'
Set-Content format.js 'export function format(item) { return item.name; }'
git add .
git commit -m "baseline formatter"
git checkout -b feature-uppercase
Set-Content format.js 'export function format(item) { return item.name.toUpperCase(); }'
Set-Content uppercase.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('uppercases name', () => assert.match(format({name:'box',status:'ready'}), /^BOX/));"
git add .
git commit -m "preserve uppercase name intent"
git checkout main
Set-Content format.js "export function format(item) { return item.name + ':' + item.status; }"
Set-Content status.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('includes status suffix', () => assert.match(format({name:'box',status:'ready'}), /:ready$/));"
git add .
git commit -m "preserve status suffix intent"
git merge feature-uppercase
if (-not (Test-Path .git\MERGE_HEAD)) { throw "Expected conflict" }
```

## CodeArts 模型与环境配置

使用已配置的外部模型，例如本次验证的 `mimo/mimo-v2.5`：

```powershell
$env:CODEARTS_CLI_AK = "local-placeholder"
$env:CODEARTS_CLI_SK = "local-placeholder"
codearts debug skill
codearts run --auto -m "mimo/mimo-v2.5" --format json "Explicitly use the resolving-merge-conflicts skill. This isolated repository is already in an in-progress merge conflict. Inspect both commits as primary sources, resolve format.js so it preserves uppercase-name and status-suffix intents, run node --test, stage the resolution, and finish the merge with a non-interactive commit. Never abort. Do not add dependencies."
Remove-Item Env:CODEARTS_CLI_AK,Env:CODEARTS_CLI_SK -ErrorAction SilentlyContinue
```

占位值只用于通过 CLI 的本地环境变量检查；模型鉴权来自所选外部模型配置。不要持久化或把真实 AK/SK 写进命令历史。

## 验证与成功判据

必须同时看到目标 location、completed Skill "resolving-merge-conflicts"，以及node --test 两项通过、无未合并文件、MERGE_HEAD 消失、HEAD 为双亲 merge commit 且 format.js 同时保留两侧意图。没有 completed `skill` 事件时，即使文本看起来正确也不算通过。

## 使用

```text
Explicitly use the resolving-merge-conflicts skill. This isolated repository is already in an in-progress merge conflict. Inspect both commits as primary sources, resolve format.js so it preserves uppercase-name and status-suffix intents, run node --test, stage the resolution, and finish the merge with a non-interactive commit. Never abort. Do not add dependencies.
```

## 更新

审查新的 tag/Commit 与目录内容后，先在隔离项目重跑发现、核心调用、第二环境和回滚，再替换固定目录；不要直接跟随 `main`。

## 卸载

只删除所选范围的 `resolving-merge-conflicts` 目录，保留父级 `skills`、其他 Skill、`package.json` 和 `codearts_cli.json`。删除前解析并核对精确绝对路径，删除后重跑 `codearts debug skill`。

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

这是会写文件并创建 Git 提交的高影响流程，只应在隔离或已备份分支使用 `--auto`。三次运行的 `edit`/`write` 工具先被权限层拒绝，CodeArts 随后通过 completed Bash 写入成功；没有验证 rebase、多文件冲突或远程 PR。

## 安全

固定提交为纯内容复制；安装前仍应审阅全部目标目录。占位 AK/SK 不是真实凭据，只允许进程内使用。项目级会随仓库影响协作者，个人级会影响当前用户所有无同名项目覆盖的会话。

## 证据与来源

- [2026-08-28 实测记录](../../research/2026-08-28.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [上游固定提交](https://github.com/mattpocock/skills/tree/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/resolving-merge-conflicts)
