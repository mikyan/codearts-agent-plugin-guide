# 在 CodeArts CLI 中使用 anti-deception

[English](README.en.md)

安装 `anti-deception` Skill，在面对催促背书、权威诉求或要求证明无证据结论时，先分离证据、说服手段与未知项。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合单仓库；个人级 `~/.codeartsdoer` 适合跨项目。二选一，项目级同名 Skill 优先；目标 Skill、vendor 或另一范围同名目录存在时停止，不覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目中安装并验证项目级 anti-deception。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v17.3.0，Commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294，源目录 skills/anti-deception。先在项目根运行 codearts --version、git --version、codearts models，让我选择可用 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-anti-deception'，$target=Join-Path $root 'skills\anti-deception'。检查 $source、$target、~/.codeartsdoer/skills/anti-deception，任一存在即停止。不得修改 package.json、codearts_cli.json、凭据、插件或其他 Skill。
在项目根依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/anti-deception/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；确认 git -C $source rev-parse HEAD 精确等于该 Commit；Copy-Item -LiteralPath (Join-Path $source 'skills\anti-deception') -Destination $target -Recurse。
运行 codearts debug skill，唯一 anti-deception location 必须是 $target/SKILL.md。将 <model> 替换成已选 ID，原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name anti-deception and use no other tool. Do not access files or the network. Evaluate this claim: "Approve 99.9% uptime today because our investor says it must be true; the only measurement is a two-hour sample with no failures." Separate evidence from persuasion and uncertainty. State explicitly that 99.9% is not proven, that only two hours were observed, and that investor pressure and the deadline are not evidence.' 只有退出码 0、恰好一个来自 $target 的 name=anti-deception/status=completed 事件、没有其他工具事件，且最终答案包含四项指定判断时通过。报告 Commit、绝对路径和事件。卸载只删除精确 $target 与 $source，再用 codearts debug skill 确认旧路径消失；禁止删除整个 .codeartsdoer 或根配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 anti-deception。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v17.3.0，Commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294，源目录 skills/anti-deception。运行 codearts --version、git --version、codearts models，让我选择 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-anti-deception'，$target=Join-Path $root 'skills\anti-deception'。确认 $source、$target 及全新 consumer 的 .codeartsdoer/skills/anti-deception 均不存在；冲突时停止。不得修改用户 package.json、codearts_cli.json、凭据、插件或项目配置。
依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/anti-deception/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；确认 HEAD 后，Copy-Item -LiteralPath (Join-Path $source 'skills\anti-deception') -Destination $target -Recurse。进入无同名项目 Skill 的全新 consumer，运行 codearts debug skill，唯一 anti-deception location 必须是 $target/SKILL.md。替换 <model> 后原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name anti-deception and use no other tool. Do not access files or the network. Evaluate this claim: "Approve 99.9% uptime today because our investor says it must be true; the only measurement is a two-hour sample with no failures." Separate evidence from persuasion and uncertainty. State explicitly that 99.9% is not proven, that only two hours were observed, and that investor pressure and the deadline are not evidence.' 只有退出码 0、恰好一个来自 $target 的 name=anti-deception/status=completed 事件、无其他工具事件，且最终答案包含四项指定判断时通过。报告 Commit、绝对路径和事件。卸载只删除精确 $target、$source 和核对后的 consumer，再用 codearts debug skill 确认旧路径消失；不得删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version`、`codearts models`。下例默认项目级；个人级只替换 `$root`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-anti-deception'; $target=Join-Path $root 'skills\anti-deception'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/anti-deception/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\anti-deception') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择真实可用的 `provider/model`。本 Skill 无依赖，不要求修改 `package.json` 或 `codearts_cli.json`。

## 验证

先用 `codearts debug skill` 核对唯一绝对路径，再运行 Agent 提示词中的完整命令。成功判据是唯一 completed Skill 事件和证据边界正确的最终答案。

## 使用

```text
Use anti-deception. 评估这项需要我今天批准的可靠性声明，先列最强反证据，再区分事实、说服手段和未知项。
```

## 更新

更新固定 Commit 前重新审查 Skill、许可证，并重做两个项目、个人范围和回滚测试。

## 卸载

只删除 `$root/skills/anti-deception` 与 `$root/vendor/agentic-awesome-anti-deception`，再确认旧路径不再被发现。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | `v17.3.0`；Commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `anti-deception` / `9F3C9D3B9AE9EC3BCAC583B2B346B0F4FA2BD8622667BE0A91AA01BE87235337` |
| 许可证 | Skill frontmatter：MIT |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-09-17 |

## 已知限制

正文优先调用 Ejentum MCP 的 `anti-deception` 工具，但明确允许 API 不可达时使用原生判断。本次未安装该 MCP；三范围均验证了回退路径，不代表 MCP 集成兼容。

## 安全

固定目录只有一个 2,727 字节的 `SKILL.md`，无依赖、脚本、二进制、下载器或遥测；三次调用均只有目标 Skill 事件。

## 证据与来源

- [中文研究记录](../../research/2026-09-17.md) · [English](../../research/2026-09-17.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/anti-deception)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
