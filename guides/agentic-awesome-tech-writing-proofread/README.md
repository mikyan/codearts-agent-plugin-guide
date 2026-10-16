# 在 CodeArts CLI 中使用 tech-writing-proofread

[English](README.en.md)

安装 `tech-writing-proofread` Skill，让 CodeArts 逐项指出英文技术文档的问题、建议和原因，而不是重写整篇文档。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 只影响当前仓库；个人级 `~/.codeartsdoer` 可供当前用户的所有项目使用。请二选一。项目级同名 Skill 按官方规则优先；任何同名项目/个人 Skill、目标或 vendor 冲突都应停止。本指南验证了两个全新项目和无项目覆盖的个人级 consumer。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目安装并验证项目级 tech-writing-proofread Skill。固定 https://github.com/sickn33/agentic-awesome-skills.git 的 v19.7.0、Commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；复制完整 skills/tech-writing-proofread（仅 SKILL.md）到 <项目>/.codeartsdoer/skills/tech-writing-proofread，vendor 为 <项目>/.codeartsdoer/vendor/agentic-awesome-tech-writing-proofread。不得修改 package.json、codearts_cli.json、权限、凭据、插件或其他 Skill。
先运行 codearts --version、git --version、node --version、codearts models 并让我选择 <model>。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-tech-writing-proofread'，$target=Join-Path $root 'skills\tech-writing-proofread'，$userTarget=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer\skills\tech-writing-proofread'；任一存在就停止。确认路径在 $root 内，运行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；执行 git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/tech-writing-proofread；要求 rev-parse HEAD 精确等于 a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；运行 Copy-Item -LiteralPath (Join-Path $source 'skills\tech-writing-proofread') -Destination $target -Recurse；要求 $target/SKILL.md SHA-256 为 830B485BE9EE9F505DCC42B7F96456EC916A38DA55A266E13C1FD5094EFD02D2。
运行 codearts debug skill，唯一同名 location 必须是 $target/SKILL.md。用所选模型原样运行：codearts run -m "<model>" --format json 'Call the skill tool exactly once with name tech-writing-proofread and use no other tool. Proofread this exact synthetic sentence without rewriting the document: "The API return an unique request IDs, and retries is safe for every POST." Return only an itemized Original -> Suggestion -> Reason list. Correct subject agreement, the article before unique, plural consistency, and the unsupported absolute retry claim by changing it to conditional language; do not invent protocol facts. End with FOUR_ISSUES_ITEMIZED and NO_DOCUMENT_REWRITE.'
只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、无其他工具事件、四类问题逐项覆盖、不重写全文、不声称所有 POST 可安全重试、包含两个标记才通过。报告 Commit、绝对路径和 Skill call ID。之后只删除 $target 与 $source，重新诊断确认旧路径消失；禁止删除整个 .codeartsdoer、根目录、用户配置或其他 Skill。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 tech-writing-proofread Skill。固定 https://github.com/sickn33/agentic-awesome-skills.git 的 v19.7.0、Commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；完整复制 skills/tech-writing-proofread（仅 SKILL.md）到 ~/.codeartsdoer/skills/tech-writing-proofread，vendor 为 ~/.codeartsdoer/vendor/agentic-awesome-tech-writing-proofread。不得修改用户 package.json、codearts_cli.json、权限、凭据、插件或其他 Skill。
先运行 codearts --version、git --version、node --version 和 codearts models，让我选择 <model>。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-tech-writing-proofread'，$target=Join-Path $root 'skills\tech-writing-proofread'，$consumer=Join-Path (Get-Location) 'codearts-tech-writing-proofread-consumer'，$projectTarget=Join-Path $consumer '.codeartsdoer\skills\tech-writing-proofread'；任一存在就停止。创建 consumer。运行 New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；执行 git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/tech-writing-proofread；要求 HEAD 为 a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1；运行 Copy-Item -LiteralPath (Join-Path $source 'skills\tech-writing-proofread') -Destination $target -Recurse；要求 $target/SKILL.md SHA-256 为 830B485BE9EE9F505DCC42B7F96456EC916A38DA55A266E13C1FD5094EFD02D2。
进入 consumer 运行 codearts debug skill，唯一 name=tech-writing-proofread 的 location 必须为 $target/SKILL.md。原样运行：codearts run -m "<model>" --format json 'Call the skill tool exactly once with name tech-writing-proofread and use no other tool. Proofread this exact synthetic sentence without rewriting the document: "The API return an unique request IDs, and retries is safe for every POST." Return only an itemized Original -> Suggestion -> Reason list. Correct subject agreement, the article before unique, plural consistency, and the unsupported absolute retry claim by changing it to conditional language; do not invent protocol facts. End with FOUR_ISSUES_ITEMIZED and NO_DOCUMENT_REWRITE.' 只有退出码 0、一个来自 $target 的 completed Skill 事件、无其他工具事件、四类问题逐项覆盖、无全文重写或绝对重试断言且两个标记存在才通过。报告 call ID。核对绝对路径后只删除 $target、$source 和新建 consumer，从无项目覆盖目录确认旧位置消失。不得删除用户根、整个 .codeartsdoer、根配置或既有项目。
```

## Windows 手动安装

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer'
# 个人级：$root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-tech-writing-proofread'
$target=Join-Path $root 'skills\tech-writing-proofread'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/tech-writing-proofread
if((git -C $source rev-parse HEAD).Trim() -ne 'a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\tech-writing-proofread') -Destination $target -Recurse
if((Get-FileHash -Algorithm SHA256 -LiteralPath (Join-Path $target 'SKILL.md')).Hash -ne '830B485BE9EE9F505DCC42B7F96456EC916A38DA55A266E13C1FD5094EFD02D2'){throw 'Skill hash mismatch'}
```

## CodeArts 配置

用 `codearts models` 选择 `provider/model`。该 Skill 只有 Markdown，不需要依赖、插件、网络、凭据或配置变更；验证不需要 `--auto`。

## 验证

先用 `codearts debug skill` 核对绝对位置，再运行 Agent 提示词中的命令。成功必须有唯一 completed Skill 事件、四类独立纠错、条件化的 POST 重试措辞和两个结束标记。

## 使用

```text
Use tech-writing-proofread on the English text below. Return an itemized Original -> Suggestion -> Reason list. Preserve the author's voice; do not rewrite the whole document or invent technical facts.
```

## 更新

固定新版本后重新安全审查并重复三范围调用与回滚；不要跟随浮动 `main`。

## 卸载

先用 `[IO.Path]::GetFullPath()` 确认两个路径都在 `$root` 下，再精确运行 `Remove-Item -LiteralPath (Join-Path $root 'skills\tech-writing-proofread') -Recurse -Force` 和 `Remove-Item -LiteralPath (Join-Path $root 'vendor\agentic-awesome-tech-writing-proofread') -Recurse -Force`，最后运行 `codearts debug skill` 确认旧路径消失。不得删除整个 `.codeartsdoer`、根配置、其他 Skill、项目或用户文件。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | Agentic Awesome `v19.7.0`；Commit `a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1` |
| 文件 / SHA-256 | `SKILL.md` / `830B485BE9EE9F505DCC42B7F96456EC916A38DA55A266E13C1FD5094EFD02D2` |
| 许可证 | MIT；来源 `alapha888/agent-skills-en` |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；PowerShell 7.6.6；Git 2.53.0.windows.2；Node 24.14.1；npm 11.11.0；`mimo/mimo-v2.5` |
| 范围 | 项目 A、全新项目 B、无项目覆盖个人级 consumer；2026-10-16 |
| 核心 Skill calls | `call_31ccce46b8e141cfa090f493`、`call_13e05a1332db4068b432ac5b`、`call_02b89ac003b94dcb821cc48f` |

## 已知限制

只验证了一句英文技术文本；长文档、术语表、多文件一致性、中文校对和自动改写均未覆盖。建议仍需作者审核，尤其是产品行为与协议语义。

## 安全

固定目录只有 `SKILL.md`；三次调用没有其他工具事件、文件写入、网络或凭据访问。卸载后用户基线哈希保持不变。

## 证据与来源

- [中文研究记录](../../research/2026-10-16.md) · [English](../../research/2026-10-16.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1/skills/tech-writing-proofread)
- [原始仓库](https://github.com/alapha888/agent-skills-en)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
