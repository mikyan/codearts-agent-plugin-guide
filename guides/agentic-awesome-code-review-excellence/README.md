# 在 CodeArts CLI 中使用 code-review-excellence

[English](README.en.md)

安装 `code-review-excellence` Skill，用结构化严重级别、修改建议和测试说明审查代码，同时保留事实边界。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合统一单个仓库的评审方法；个人级 `~/.codeartsdoer` 适合跨项目复用。二选一；项目级同名 Skill 优先。目标 Skill、vendor 或另一范围的同名 Skill 已存在时停止，不要覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目中安装并验证项目级 code-review-excellence。固定来源 https://github.com/sickn33/agentic-awesome-skills.git，release v17.2.0，Commit 2fce708d4f3871ccce3d705b4748371f94c730ec，源目录 skills/code-review-excellence。
先运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-code-review-excellence'，$target=Join-Path $root 'skills\code-review-excellence'。检查 $source、$target 和 ~/.codeartsdoer/skills/code-review-excellence；任一冲突就停止。只创建 $source 和 $target，不修改 package.json、codearts_cli.json、凭据、插件或其他 Skill。
从项目根依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/code-review-excellence/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'；git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec。确认 git -C $source rev-parse HEAD 精确等于该 Commit，再执行 Copy-Item -LiteralPath (Join-Path $source 'skills\code-review-excellence') -Destination $target -Recurse。
运行 codearts debug skill；唯一生效的 code-review-excellence location 必须是 $target/SKILL.md。把 <model> 换成已选 ID，原样执行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name code-review-excellence. Use no other tool, do not access the network, and do not read or write files. Review only this inline JavaScript: async function get(id){try{return await db.user.findUnique({where:{id}})}catch(e){return null}}. The contract requires database failures to remain distinguishable from a missing user. Return exact sections SUMMARY, BLOCKING, IMPORTANT, MINOR, SUGGESTED CHANGE, TEST NOTES, and QUESTIONS. Flag swallowed database errors as blocking, explain why null is ambiguous, suggest a typed or explicit error boundary, and do not invent repository context.'
只有退出码 0、恰好一个来自 $target 的 name=code-review-excellence/status=completed 事件、没有其他工具事件，且最终文本具有七个章节或明确等价标题并满足全部语义要求时通过。报告 Commit、绝对路径和事件。卸载只能删除精确 $target 与 $source，再用 codearts debug skill 确认旧路径消失；禁止删除整个 .codeartsdoer、根配置或其他 Skill。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 code-review-excellence。固定来源 https://github.com/sickn33/agentic-awesome-skills.git，release v17.2.0，Commit 2fce708d4f3871ccce3d705b4748371f94c730ec，源目录 skills/code-review-excellence。
运行 codearts --version、git --version、codearts models，让我选择 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-code-review-excellence'，$target=Join-Path $root 'skills\code-review-excellence'。确认 $source、$target 及全新 consumer 的 .codeartsdoer/skills/code-review-excellence 都不存在；冲突时停止。不得修改用户 package.json、codearts_cli.json、凭据、插件或项目配置。
依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/code-review-excellence/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'；git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec；确认 HEAD 后，Copy-Item -LiteralPath (Join-Path $source 'skills\code-review-excellence') -Destination $target -Recurse。
进入没有同名项目 Skill 的全新 consumer。codearts debug skill 的唯一 code-review-excellence location 必须是 $target/SKILL.md。把 <model> 换成已选 ID，原样运行项目级提示词中的 codearts run 命令，并沿用相同成功判据。卸载只删除精确 $target、$source 和核对后的 consumer；不得删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version`、`codearts models`。下例默认项目级；个人级只替换 `$root`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-code-review-excellence'; $target=Join-Path $root 'skills\code-review-excellence'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/code-review-excellence/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'
git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec
if((git -C $source rev-parse HEAD).Trim() -ne '2fce708d4f3871ccce3d705b4748371f94c730ec'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\code-review-excellence') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择可用的 `provider/model`。凭据只保存在官方用户配置中；本 Skill 不要求修改 `package.json` 或 `codearts_cli.json`。

## 验证

先用 `codearts debug skill` 核对唯一来源，再运行 Agent 提示词中的完整 `codearts run`。成功判据是退出码 0、唯一 completed `code-review-excellence` 事件来自目标绝对路径、没有其他工具事件，并清楚区分数据库故障与未找到用户。

## 使用

```text
Use code-review-excellence. 只审查这段变更，按 blocking、important、minor 分组；每项说明证据、影响、建议修改与应补测试，不要推断未提供的仓库事实。
```

## 更新

用 `git -C $source rev-parse HEAD` 核对固定版本。更新 Commit 前重新审查许可证和整个 `skills/code-review-excellence` 目录，并重做两个项目、个人范围和回滚测试。

## 卸载

解析绝对路径后，只删除 `$root/skills/code-review-excellence` 与 `$root/vendor/agentic-awesome-code-review-excellence`，再用 `codearts debug skill` 确认旧路径消失。不得删除整个 `.codeartsdoer`、根配置或其他 Skill。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | release `v17.2.0`；Commit `2fce708d4f3871ccce3d705b4748371f94c730ec` |
| Skill / SHA-256 | `code-review-excellence` / `B5141711AFADAD742F31E576BD45D6A68E9D259425B064BF1352BCB19DADF49A` |
| 内容许可证 | CC BY 4.0（AAS `LICENSE-CONTENT`） |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-09-15 |

## 已知限制

只验证了内联代码的只读评审，没有读取真实仓库、PR、测试或详细 playbook。三个范围的标题措辞和修改方案略有差异；用户仍需结合项目错误类型与上层契约复核建议。

## 安全

固定目录含两个 Markdown 文件，共 15,670 字节；无依赖、锁文件、生命周期脚本、二进制、下载器或遥测。三次实测都只有目标 Skill 事件，没有文件、Shell 或网络访问。

## 证据与来源

- [中文研究记录](../../research/2026-09-15.md) · [English](../../research/2026-09-15.en.md)
- [固定 Skill 目录](https://github.com/sickn33/agentic-awesome-skills/tree/2fce708d4f3871ccce3d705b4748371f94c730ec/skills/code-review-excellence)
- [AAS 内容许可证](https://github.com/sickn33/agentic-awesome-skills/blob/2fce708d4f3871ccce3d705b4748371f94c730ec/LICENSE-CONTENT)
- [CodeArts CLI Skills 官方文档](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
