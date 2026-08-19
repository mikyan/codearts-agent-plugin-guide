# 在 CodeArts CLI 中使用 i-have-adhd

[English](README.en.md)

安装 `i-have-adhd` Skill，让 Agent 以行动优先、分步且少干扰的方式组织回复。

## 选择安装范围

先且只选一种范围：

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 团队需要固定版本，或只在一个项目使用。默认推荐。 |
| 个人级 | `~/.codeartsdoer` | 当前 Windows 用户希望在多个项目中使用。 |

CodeArts 官方 CLI 文档写明同名 Skill 应由项目级覆盖个人级；但 CodeArts CLI 26.8.1 的本机实测在两个范围同时存在 `i-have-adhd` 时解析到了个人级路径。为避免版本差异，本指南要求发现任一范围已有同名 Skill 就停止，不要重复安装。

## 让 Agent 帮你安装

### 项目级安装提示词

```text
请在当前项目根目录为 CodeArts CLI 安装并验证项目级 ayghri/i-have-adhd Skill，固定到 Commit e7555fcaf612dfa1739dc86610ea926a906db614（上游 package.json 版本 0.2.0）。

严格执行：
1. 只安装以下两个目标：<项目根目录>/.codeartsdoer/vendor/i-have-adhd（固定源码）和 <项目根目录>/.codeartsdoer/skills/i-have-adhd/SKILL.md（CodeArts 原生 Skill）。不要修改 ~/.codeartsdoer、任何 package.json、codearts_cli.json、凭据或全局软件。
2. 在项目根目录运行 codearts --version、git --version 和 codearts models。让我选择一个实际可用的 provider/model ID；如果 CodeArts 报告缺少凭据，停止并让我按官方文档配置，不要读取、打印或写入秘密。
3. 检查项目级源码目录、项目级 Skill 目录以及 ~/.codeartsdoer/skills/i-have-adhd。任一存在就停止并报告同名冲突，不得覆盖。
4. 说明本次没有 npm 依赖、install/postinstall、二进制或网络运行时；只执行 Git clone/checkout，并只复制上游 skills/i-have-adhd/SKILL.md，不复制 hooks、extensions、插件清单或 agents 子目录。
5. 在项目根目录的 PowerShell 原样执行：
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\i-have-adhd"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\i-have-adhd"
   $userSkill = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer\skills\i-have-adhd"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target) -or (Test-Path -LiteralPath $userSkill)) { throw "An i-have-adhd source or Skill already exists; inspect the conflict instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/ayghri/i-have-adhd.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/i-have-adhd"
   git -C $source checkout --detach "e7555fcaf612dfa1739dc86610ea926a906db614"
   if ((git -C $source rev-parse HEAD).Trim() -ne "e7555fcaf612dfa1739dc86610ea926a906db614") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\i-have-adhd\SKILL.md") -Destination (Join-Path $target "SKILL.md")
6. 运行 codearts debug skill，确认只有一个 i-have-adhd，location 必须是当前项目 .codeartsdoer/skills/i-have-adhd/SKILL.md。
7. 把 <选择的模型> 替换为第 2 步确认的 ID，然后原样执行：
   codearts run --format json --sandbox --model "<选择的模型>" "Verification contract: call the skill tool exactly once with name i-have-adhd. The skill tool is the only allowed tool. After its completed event, immediately produce the final answer; a bash, file, time, task, or any second tool call means failure. Answer in Chinese using only the prompt and loaded skill: 给出三步阅读一段 npm 错误文本的清单。第一行是打开错误文本；正好三步编号；最后一个动作是在两分钟内圈出第一个 Error 行。不要查询时间，不要修改文件，不要执行命令。"
8. 通过标准：退出状态为 0；JSON 中只有一次 tool=skill、name=i-have-adhd、status=completed 的事件；Base directory 指向当前项目目标目录；没有其他工具事件；最终结果正好三条编号动作，第一条打开错误文本，第三条要求两分钟内圈出第一个 Error 行。
9. 报告 Commit、目标文件、CodeArts 来源路径、工具事件、最终结果和卸载清单。卸载只能移除 <项目根目录>/.codeartsdoer/skills/i-have-adhd 与 <项目根目录>/.codeartsdoer/vendor/i-have-adhd；禁止删除整个 .codeartsdoer、package.json、ProjectSkillStatus.txt 或其他 Skill。任一步失败就如实停止，不得宣称成功。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装并验证个人级 ayghri/i-have-adhd Skill，固定到 Commit e7555fcaf612dfa1739dc86610ea926a906db614（上游 package.json 版本 0.2.0）。

严格执行：
1. 在 PowerShell 定义 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"。只安装 $userRoot/vendor/i-have-adhd 和 $userRoot/skills/i-have-adhd/SKILL.md；不要修改 $userRoot/package.json、codearts_cli.json、凭据、现有插件、项目配置或全局软件。
2. 运行 codearts --version、git --version 和 codearts models。让我选择一个实际可用的 provider/model ID；凭据不足时停止并让我按官方文档配置，不要读取、打印或写入秘密。
3. 检查 $userRoot/vendor/i-have-adhd 和 $userRoot/skills/i-have-adhd。任一存在就停止并报告，不得覆盖。还要在用于验证的目录确认不存在 .codeartsdoer/skills/i-have-adhd。
4. 说明本次没有 npm 依赖、install/postinstall、二进制或网络运行时；只执行 Git clone/checkout，并只复制上游 skills/i-have-adhd/SKILL.md，不复制 hooks、extensions、插件清单或 agents 子目录。
5. 在同一个 PowerShell 会话原样执行：
   $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
   $source = Join-Path $userRoot "vendor\i-have-adhd"
   $target = Join-Path $userRoot "skills\i-have-adhd"
   $consumer = Join-Path ([System.IO.Path]::GetTempPath()) "codearts-i-have-adhd-e7555fc-smoke"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target) -or (Test-Path -LiteralPath $consumer)) { throw "Source, target, or clean smoke directory already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   New-Item -ItemType Directory -Path $consumer | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/ayghri/i-have-adhd.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/i-have-adhd"
   git -C $source checkout --detach "e7555fcaf612dfa1739dc86610ea926a906db614"
   if ((git -C $source rev-parse HEAD).Trim() -ne "e7555fcaf612dfa1739dc86610ea926a906db614") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\i-have-adhd\SKILL.md") -Destination (Join-Path $target "SKILL.md")
   Set-Location -LiteralPath $consumer
6. 在 $consumer 运行 codearts debug skill，确认只有一个 i-have-adhd，location 必须是 $userRoot/skills/i-have-adhd/SKILL.md，且 $consumer 下没有项目级同名目录。
7. 把 <选择的模型> 替换为第 2 步确认的 ID，然后原样执行：
   codearts run --format json --sandbox --model "<选择的模型>" "Verification contract: call the skill tool exactly once with name i-have-adhd. The skill tool is the only allowed tool. After its completed event, immediately produce the final answer; a bash, file, time, task, or any second tool call means failure. Answer in Chinese using only the prompt and loaded skill: 给出三步阅读一段 npm 错误文本的清单。第一行是打开错误文本；正好三步编号；最后一个动作是在两分钟内圈出第一个 Error 行。不要查询时间，不要修改文件，不要执行命令。"
8. 通过标准：退出状态为 0；JSON 中只有一次成功的 i-have-adhd Skill 事件；Base directory 指向个人级目标；没有其他工具事件；最终结果正好三条编号动作，第一条打开错误文本，第三条要求两分钟内圈出第一个 Error 行。
9. 报告 Commit、目标文件、CodeArts 来源路径、工具事件、最终结果和卸载清单。卸载只能移除 $userRoot/skills/i-have-adhd、$userRoot/vendor/i-have-adhd，以及确认精确路径后可选移除 $consumer；禁止删除整个 $userRoot、$userRoot/package.json、codearts_cli.json、凭据或其他 Skill。任一步失败就如实停止，不得宣称成功。
```

## Windows 手动安装

### 前置条件

安装 CodeArts CLI 和 Git，并确认模型可用：

```powershell
codearts --version
git --version
codearts models
```

以下步骤不需要 npm。安装前先确认项目级与个人级都没有同名 Skill。

### 项目级

在目标项目根目录执行：

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\i-have-adhd"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\i-have-adhd"
$userSkill = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer\skills\i-have-adhd"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target) -or (Test-Path -LiteralPath $userSkill)) { throw "An i-have-adhd source or Skill already exists." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path $target -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/ayghri/i-have-adhd.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/i-have-adhd"
git -C $source checkout --detach "e7555fcaf612dfa1739dc86610ea926a906db614"
if ((git -C $source rev-parse HEAD).Trim() -ne "e7555fcaf612dfa1739dc86610ea926a906db614") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\i-have-adhd\SKILL.md") -Destination (Join-Path $target "SKILL.md")
```

### 个人级

在 PowerShell 执行：

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\i-have-adhd"
$target = Join-Path $userRoot "skills\i-have-adhd"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "An i-have-adhd source or Skill already exists." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path $target -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/ayghri/i-have-adhd.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/i-have-adhd"
git -C $source checkout --detach "e7555fcaf612dfa1739dc86610ea926a906db614"
if ((git -C $source rev-parse HEAD).Trim() -ne "e7555fcaf612dfa1739dc86610ea926a906db614") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\i-have-adhd\SKILL.md") -Destination (Join-Path $target "SKILL.md")
```

## CodeArts 配置

用 `codearts models` 选择实际可用的 `provider/model` ID，并在下方验证命令中替换模型。使用华为云模型时按[官方 AK/SK 指引](https://support.huaweicloud.com/codeartsagent_faq/codeartsagent_faq_0054.html)配置；使用自定义 Provider 时按本机配置提供凭据。不要把 API Key、AK 或 SK 写入仓库或命令输出。

本次 MiMo 自定义 Provider 验证中，CodeArts CLI 26.8.1 的前置检查仍要求当前进程存在 `CODEARTS_CLI_AK` 和 `CODEARTS_CLI_SK`；非秘密占位值只用于通过该本机前置检查，真实模型鉴权仍来自既有 Provider 配置。这是环境现象，不是通用配置建议。

## 验证

先运行 `codearts debug skill`，确认唯一匹配项的 `location` 属于所选范围。个人级验证必须在没有项目级同名 Skill 的目录进行。

再替换模型 ID 并执行：

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Verification contract: call the skill tool exactly once with name i-have-adhd. The skill tool is the only allowed tool. After its completed event, immediately produce the final answer; a bash, file, time, task, or any second tool call means failure. Answer in Chinese using only the prompt and loaded skill: 给出三步阅读一段 npm 错误文本的清单。第一行是打开错误文本；正好三步编号；最后一个动作是在两分钟内圈出第一个 Error 行。不要查询时间，不要修改文件，不要执行命令。"
```

成功判据是一次且仅一次 `skill` 工具事件，输入名为 `i-have-adhd`、状态为 `completed`，Base directory 指向安装目录；最终文本正好是三条行动，且没有任何其他工具事件。

## 使用

显式调用最可靠：

```text
Use i-have-adhd for this task. 把下面的排障方案改成第一步可立即执行、最多五项、每项一个动作，并以唯一的下一步结束：<粘贴方案>
```

本轮只验证了单次显式调用。不要把上游描述中的会话持续模式或 always-on Hook 当作已验证功能。

## 更新

先检查当前固定版本：

```powershell
git -C <所选范围>\.codeartsdoer\vendor\i-have-adhd rev-parse HEAD
```

升级到新 Commit 前重新审查许可证、`SKILL.md`、清单、Hook、脚本与安全差异。精确卸载旧的 Skill 和专用 vendor 后，替换本文中的 Commit 并从零重装、复现、回滚；在完成真实 CodeArts 验证前，不要沿用本文的 `Works` 结论。

## 卸载

项目级在项目根目录执行：

```powershell
$targets = @(
  (Join-Path (Get-Location) ".codeartsdoer\skills\i-have-adhd"),
  (Join-Path (Get-Location) ".codeartsdoer\vendor\i-have-adhd")
)
foreach ($target in $targets) {
  if (Test-Path -LiteralPath $target) {
    $resolved = (Resolve-Path -LiteralPath $target).Path
    if ($resolved -ne [System.IO.Path]::GetFullPath($target)) { throw "Unexpected path: $resolved" }
    Remove-Item -LiteralPath $resolved -Recurse -Force
  }
}
```

个人级执行：

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$targets = @(
  (Join-Path $userRoot "skills\i-have-adhd"),
  (Join-Path $userRoot "vendor\i-have-adhd")
)
foreach ($target in $targets) {
  if (Test-Path -LiteralPath $target) {
    $resolved = (Resolve-Path -LiteralPath $target).Path
    if ($resolved -ne [System.IO.Path]::GetFullPath($target)) { throw "Unexpected path: $resolved" }
    Remove-Item -LiteralPath $resolved -Recurse -Force
  }
}
```

不要删除整个 `.codeartsdoer`、根 `package.json`、`codearts_cli.json`、凭据、`ProjectSkillStatus.txt` 或其他 Skill。卸载后用 `codearts debug skill` 确认没有 `i-have-adhd`；本次真实回滚还确认了个人根 `package.json` 的 SHA-256 与安装前一致。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Works** |
| 上游版本 | `0.2.0`，无 GitHub Release |
| 固定 Commit | `e7555fcaf612dfa1739dc86610ea926a906db614` |
| 已验证 Skill | `i-have-adhd` |
| Skill SHA-256 | `13969C0AA5A69127828EF10FB4A53AA2B56D08C184B9F4354D0EA34695C358BB` |
| 许可证 | MIT |
| CodeArts | Windows 11 Build 26200 上的 CLI 26.8.1 |
| 模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目级、第二个全新项目、个人级 |
| 最后验证日期 | 2026-08-19 |

`Works` 只适用于把固定 Commit 的单个内容型 `SKILL.md` 放入 CodeArts 原生 Skills 目录。没有验证上游其他客户端的插件、Hook 或扩展。

## 已知限制

- CodeArts CLI 26.8.1 的同名解析实测与当前官方 CLI 文档相反：两个本地范围同时安装时，`codearts debug skill` 返回个人级路径。只安装一个范围，并以诊断输出为准。
- 只验证显式 `skill` 调用和单轮输出；未验证上游的持续会话开关、`stop adhd mode`、always-on Hook 或 OpenCode/Claude/Codex 插件。
- 上游 Frontmatter 含 `disable-model-invocation: true`，但 CodeArts 运行时 `skill` 工具返回正文时不包含 Frontmatter；本指南不声称自动调用行为。
- 第一次个人级 smoke test 中，模型违背“不得调用其他工具”并额外读取系统时间；更严格且不依赖外部信息的提示重跑后只调用 Skill 并通过。工具约束仍受模型遵循度影响。
- 未验证 CodeArts IDE、Linux、其他模型、辅助 `agents` 元数据或无障碍/医学效果。

## 安全

固定 Commit 为 MIT。安装目标只有 6,953 字节的 `SKILL.md`，不运行 npm、生命周期脚本、Hook、扩展或二进制，也不需要运行时网络、凭据或遥测。上游仓库确实包含其他客户端的 Hook、PowerShell/Shell/Node 脚本和 TypeScript 扩展；它们经过静态审查但没有安装或执行。Skill 会改变输出风格并声明持续模式，安装前应自行阅读正文；系统与用户安全要求始终高于该 Skill。

## 证据与来源

- [2026-08-19 中文验证记录](../../research/2026-08-19.md) · [English](../../research/2026-08-19.en.md)
- [固定 Commit](https://github.com/ayghri/i-have-adhd/tree/e7555fcaf612dfa1739dc86610ea926a906db614)
- [已复制的 SKILL.md](https://github.com/ayghri/i-have-adhd/blob/e7555fcaf612dfa1739dc86610ea926a906db614/skills/i-have-adhd/SKILL.md)
- [MIT License](https://github.com/ayghri/i-have-adhd/blob/e7555fcaf612dfa1739dc86610ea926a906db614/LICENSE)
- [CodeArts CLI Skills 官方文档](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI 命令官方文档](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0034.html)
