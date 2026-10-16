# 在 CodeArts CLI 中使用 git-commit-message

[English](README.en.md)

安装 `git-commit-message` Skill，让 CodeArts 从真实暂存差异生成简洁、说明原因的 Conventional Commit 文案，而不替你暂存、提交或推送。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 只影响当前仓库；个人级 `~/.codeartsdoer` 可供当前用户的所有项目使用。请二选一。CodeArts 官方规则是项目级同名 Skill 优先；项目级、个人级、目标 Skill 或 vendor 中任何同名目录已存在时都应停止，不要覆盖。本指南验证了两个全新项目和一个没有项目级同名 Skill 的个人级 consumer。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目安装并验证项目级 git-commit-message Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v19.7.0，Commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；完整复制源是 skills/git-commit-message（仅 SKILL.md），vendor 目标是 <项目>/.codeartsdoer/vendor/agentic-awesome-git-commit-message，原生目标是 <项目>/.codeartsdoer/skills/git-commit-message。不得修改 package.json、codearts_cli.json、权限、凭据、插件、其他 Skill、工作树或暂存区。
先在项目根运行 codearts --version、git --version、node --version 和 codearts models，让我从真实列表选择 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-git-commit-message'，$target=Join-Path $root 'skills\git-commit-message'，$userTarget=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer\skills\git-commit-message'。若 $source、$target 或 $userTarget 任一存在，立即停止并报告冲突。确认 $source 和 $target 的绝对路径均在 $root 内；运行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；执行 git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source；执行 git -C $source sparse-checkout set skills/git-commit-message；要求 (git -C $source rev-parse HEAD).Trim() 精确等于 a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；运行 Copy-Item -LiteralPath (Join-Path $source 'skills\git-commit-message') -Destination $target -Recurse；要求 $target/SKILL.md 的 SHA-256 精确为 6261A9FDD3CF103F279CD936BABF4C0CF607A0CF02E287573B0F37563632AC51。
确认当前仓库已有用户明确要审阅的暂存差异；若 git diff --cached 为空就停止，不得自行暂存。运行 codearts debug skill，唯一 name=git-commit-message 的 location 必须精确为 $target/SKILL.md。把 <model> 替换为所选 provider/model ID，原样运行：codearts run -m "<model>" --format json --auto 'Call the skill tool exactly once with name git-commit-message. Inspect the real staged change by running only git status --short, git diff --cached --stat, and git diff --cached. Do not stage, modify, commit, push, or access the network. Return one conventional commit message with a subject of at most 50 characters and a 1-3 line body explaining why. End with COMMIT_MESSAGE_FROM_STAGED_DIFF and NO_GIT_MUTATION.'
只有退出码为 0、恰好一个来自 $target 的 completed Skill 事件、恰好三个 completed Bash 事件对应上述只读 Git 命令、提交主题格式正确、正文说明原因、两个结尾标记存在，且调用前后 git diff --cached 与 HEAD 完全相同时才通过。报告 Commit、绝对路径、Skill call ID 和判据。之后再次核对路径，只删除 $target 和 $source；重新运行 codearts debug skill，确认旧路径消失。禁止删除整个 .codeartsdoer、项目根、用户根、根配置、其他 Skill 或任何用户文件。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 git-commit-message Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v19.7.0，Commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；完整复制源是 skills/git-commit-message（仅 SKILL.md），vendor 目标是 ~/.codeartsdoer/vendor/agentic-awesome-git-commit-message，原生目标是 ~/.codeartsdoer/skills/git-commit-message。不得修改用户 package.json、codearts_cli.json、权限、凭据、插件或其他 Skill。
先运行 codearts --version、git --version、node --version 和 codearts models，让我选择 <model>。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-git-commit-message'，$target=Join-Path $root 'skills\git-commit-message'，$consumer=Join-Path (Get-Location) 'codearts-git-commit-message-consumer'，$projectTarget=Join-Path $consumer '.codeartsdoer\skills\git-commit-message'。若 $source、$target、$consumer 或 $projectTarget 任一存在就停止。创建 $consumer；在其中运行 git init、git config user.name 'CodeArts Verification'、git config user.email 'codearts-verification@example.invalid'；创建内容精确为 `def greet(display_name):` 换行四空格 `return f"Hello {display_name}"` 的 greeting.py，运行 git add greeting.py 和 git commit -m baseline；再把 return 行精确改成 `return f"Hello {display_name or 'there'}"` 并只运行 git add greeting.py；不得推送。运行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；执行 git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/git-commit-message；要求 (git -C $source rev-parse HEAD).Trim() 等于 a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；运行 Copy-Item -LiteralPath (Join-Path $source 'skills\git-commit-message') -Destination $target -Recurse；要求 $target/SKILL.md 的 SHA-256 为 6261A9FDD3CF103F279CD936BABF4C0CF607A0CF02E287573B0F37563632AC51。
进入 $consumer，运行 codearts debug skill，唯一 name=git-commit-message 的 location 必须是 $target/SKILL.md。把 <model> 换成所选 ID 并原样运行：codearts run -m "<model>" --format json --auto 'Call the skill tool exactly once with name git-commit-message. Inspect the real staged change by running only git status --short, git diff --cached --stat, and git diff --cached. Do not stage, modify, commit, push, or access the network. Return one conventional commit message with a subject of at most 50 characters and a 1-3 line body explaining why. End with COMMIT_MESSAGE_FROM_STAGED_DIFF and NO_GIT_MUTATION.' 只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、恰好三个对应只读 Git 命令的 completed Bash 事件、主题和正文合规、两个标记存在，且调用前后 HEAD 与暂存差异不变才通过。报告 Skill call ID。验证后只删除 $target、$source 和本提示词新建的精确 $consumer；从无项目覆盖目录重新诊断并确认旧路径消失。禁止删除用户根、整个 .codeartsdoer、根配置、其他 Skill 或既有项目。
```

## Windows 手动安装

先运行 `codearts models` 选择真实可用的模型。以下默认为项目级；个人级将 `$root` 换为注释中的用户目录。目标存在时停止。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer'
# 个人级：$root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-git-commit-message'
$target=Join-Path $root 'skills\git-commit-message'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/git-commit-message
if((git -C $source rev-parse HEAD).Trim() -ne 'a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\git-commit-message') -Destination $target -Recurse
if((Get-FileHash -Algorithm SHA256 -LiteralPath (Join-Path $target 'SKILL.md')).Hash -ne '6261A9FDD3CF103F279CD936BABF4C0CF607A0CF02E287573B0F37563632AC51'){throw 'Skill hash mismatch'}
```

## CodeArts 配置

用 `codearts models` 选择真实 `provider/model`。Skill 只有 Markdown，不需要 npm、Python、MCP、Hook、插件、凭据或网络，也不要修改根配置。验证需要让 Agent 执行三个只读 Git 命令，因此使用 `--auto` 前应确认当前目录与暂存差异；正常生成文案也可在交互模式逐次批准。

## 验证

运行 `codearts debug skill` 核对唯一同名 Skill 的绝对位置，再执行 Agent 提示词中的 `codearts run`。成功需要 completed Skill 事件、三个只读 Git 事件、正确提交文案、两个标记和完全未变的 HEAD/暂存差异；仅输出看起来合理的 message 不够。

## 使用

```text
Use git-commit-message. Inspect my staged diff but do not change it. Propose one Conventional Commit subject (50 characters or fewer) and an optional 1-3 line body explaining why. Do not commit or push.
```

## 更新

更新 release 或 Commit 前重新审查 `skills/git-commit-message/SKILL.md` 和许可证，再重复两个新项目、无覆盖个人级 consumer、核心调用与回滚。不要改用浮动 `main`。

## 卸载

先用 `[IO.Path]::GetFullPath()` 确认两个路径都在 `$root` 下，再精确运行 `Remove-Item -LiteralPath (Join-Path $root 'skills\git-commit-message') -Recurse -Force` 和 `Remove-Item -LiteralPath (Join-Path $root 'vendor\agentic-awesome-git-commit-message') -Recurse -Force`。随后运行 `codearts debug skill`，确认旧绝对路径消失。不得删除整个 `.codeartsdoer`、根配置、其他 Skill、Git 工作区或用户文件。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | Agentic Awesome `v19.7.0`；Commit `a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1` |
| 文件 / SHA-256 | `SKILL.md` / `6261A9FDD3CF103F279CD936BABF4C0CF607A0CF02E287573B0F37563632AC51` |
| 许可证 | MIT；来源 `alapha888/agent-skills-en` |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；PowerShell 7.6.6；Git 2.53.0.windows.2；Node 24.14.1；npm 11.11.0；`mimo/mimo-v2.5` |
| 范围 | 项目 A、全新项目 B、无项目覆盖个人级 consumer；2026-10-16 |
| 核心 Skill calls | `call_e2c70223aa6c4d43b050afe0`、`call_db63b94e268c4f0ebce3aad7`、`call_9f7646ef15a7402497ee8a39` |

## 已知限制

只验证了单一暂存修复；混合多类变更、空暂存区、合并提交和非英语主题未覆盖。Skill 生成文案，不会替你判断是否应拆分提交；最终 message 仍需人工核对。

## 安全

固定目录只有 `SKILL.md`，没有安装脚本、二进制、下载器、运行时网络、凭据或遥测。三次调用只执行 `git status` 与两条 `git diff --cached`，没有写文件、提交或推送。回滚后用户根三个基线文件哈希未变。

## 证据与来源

- [中文研究记录](../../research/2026-10-16.md) · [English](../../research/2026-10-16.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1/skills/git-commit-message)
- [原始仓库](https://github.com/alapha888/agent-skills-en)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
