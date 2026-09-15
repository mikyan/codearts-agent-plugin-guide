# 在 CodeArts CLI 中使用 debugging-strategies

[English](README.en.md)

安装 `debugging-strategies` Skill，用复现、观察、假设、受控实验和验证链路系统化定位问题。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 用于单仓库调试规范；个人级 `~/.codeartsdoer` 用于跨项目复用。二选一；项目级同名 Skill 优先。Skill、vendor 或另一范围同名目标存在时停止。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目中安装并验证项目级 debugging-strategies。固定来源 https://github.com/sickn33/agentic-awesome-skills.git，release v17.2.0，Commit 2fce708d4f3871ccce3d705b4748371f94c730ec，源目录 skills/debugging-strategies。
先运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-debugging-strategies'，$target=Join-Path $root 'skills\debugging-strategies'。检查 $source、$target 和 ~/.codeartsdoer/skills/debugging-strategies；任一冲突就停止。只创建 $source 和 $target，不修改 package.json、codearts_cli.json、凭据、插件或其他 Skill。
从项目根依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/debugging-strategies/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'；git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec。确认 git -C $source rev-parse HEAD 精确等于该 Commit，再执行 Copy-Item -LiteralPath (Join-Path $source 'skills\debugging-strategies') -Destination $target -Recurse。
运行 codearts debug skill；唯一生效的 debugging-strategies location 必须是 $target/SKILL.md。把 <model> 换成已选 ID，原样执行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name debugging-strategies. Use no other tool, do not access the network, and do not read or write files. Analyze only this synthetic evidence: formatPrice(12.30) returns "$12.3" on three identical runs; trace shows it concatenates "$" + amount.toString(); expected is always two decimal places; no fix has been applied. Return exact sections REPRODUCTION, OBSERVATIONS, HYPOTHESES, CONTROLLED EXPERIMENT, ROOT CAUSE, FIX VERIFICATION, and LIMITS. Identify toString formatting as the leading root cause, propose a controlled test using 12, 12.3, and 12.345, and do not claim the test ran.'
只有退出码 0、恰好一个来自 $target 的 name=debugging-strategies/status=completed 事件、没有其他工具事件、最终文本含七个章节及全部要求，并明确实验未执行时通过。报告 Commit、绝对路径和事件。卸载只能删除精确 $target 与 $source，再用 codearts debug skill 确认旧路径消失；禁止删除整个 .codeartsdoer 或根配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 debugging-strategies。固定来源 https://github.com/sickn33/agentic-awesome-skills.git，release v17.2.0，Commit 2fce708d4f3871ccce3d705b4748371f94c730ec，源目录 skills/debugging-strategies。
运行 codearts --version、git --version、codearts models，让我选择 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-debugging-strategies'，$target=Join-Path $root 'skills\debugging-strategies'。确认 $source、$target 及全新 consumer 的 .codeartsdoer/skills/debugging-strategies 不存在；冲突时停止。不得修改用户 package.json、codearts_cli.json、凭据、插件或项目配置。
依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/debugging-strategies/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'；git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec；确认 HEAD 后，Copy-Item -LiteralPath (Join-Path $source 'skills\debugging-strategies') -Destination $target -Recurse。
进入没有同名项目 Skill 的全新 consumer；唯一 codearts debug skill location 必须是 $target/SKILL.md。把 <model> 换成已选 ID，原样运行项目级提示词中的 codearts run，并沿用相同成功判据。卸载只删除精确 $target、$source 和核对后的 consumer；不得删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-debugging-strategies'; $target=Join-Path $root 'skills\debugging-strategies'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/debugging-strategies/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'
git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec
if((git -C $source rev-parse HEAD).Trim() -ne '2fce708d4f3871ccce3d705b4748371f94c730ec'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\debugging-strategies') -Destination $target -Recurse
```

## CodeArts 配置

先运行 `codearts --version`、`git --version`、`codearts models`，选择可用 `provider/model`。本 Skill 不要求修改 `package.json` 或 `codearts_cli.json`。

## 验证

用 `codearts debug skill` 核对唯一来源，再运行完整 smoke test。成功需要唯一 completed 目标 Skill 事件、无额外工具、稳定复现说明、`toString()` 根因、三组受控输入和未执行声明。

## 使用

```text
Use debugging-strategies. 只根据我提供的复现、日志与追踪，分开列观察、假设、受控实验、根因与修复验证；未知项标为证据缺口，不要声称未执行的测试已运行。
```

## 更新

先核对 `git -C $source rev-parse HEAD`。更新 Commit 前审查许可证和整个 Skill 目录，并重做两个项目、个人范围与回滚。

## 卸载

只删除 `$root/skills/debugging-strategies` 与 `$root/vendor/agentic-awesome-debugging-strategies`，再确认旧路径从 `codearts debug skill` 消失。不要删除 `.codeartsdoer` 根或其他 Skill。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | `v17.2.0` / `2fce708d4f3871ccce3d705b4748371f94c730ec` |
| Skill / SHA-256 | `debugging-strategies` / `0AA1A6597A9B97EBEF24054C6DB8083D65640358EA0EB8F228AB8D0718247365` |
| 内容许可证 | CC BY 4.0 |
| 环境与范围 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5`；两个项目 + 个人级；2026-09-15 |

## 已知限制

只验证了合成内联证据的只读分析，没有读取真实日志、执行实验、修改代码或打开详细 playbook。输出建议的 `toFixed(2)` 仍需结合金额精度与国际化要求复核。

## 安全

固定目录含两个 Markdown 文件，共 14,047 字节；无依赖、脚本、二进制、下载器或遥测。三次调用都只有目标 Skill 事件。

## 证据与来源

- [中文研究记录](../../research/2026-09-15.md) · [English](../../research/2026-09-15.en.md)
- [固定 Skill 目录](https://github.com/sickn33/agentic-awesome-skills/tree/2fce708d4f3871ccce3d705b4748371f94c730ec/skills/debugging-strategies)
- [内容许可证](https://github.com/sickn33/agentic-awesome-skills/blob/2fce708d4f3871ccce3d705b4748371f94c730ec/LICENSE-CONTENT)
- [CodeArts CLI Skills 官方文档](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
