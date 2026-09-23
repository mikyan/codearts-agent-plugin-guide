# 在 CodeArts CLI 中使用 andrej-karpathy

[English](README.en.md)

安装 `andrej-karpathy` Skill，让 Agent 在写代码、审查或重构前先显式说明假设，坚持最小改动，并把任务改写成可验证的成功标准。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 只影响当前仓库；个人级 `~/.codeartsdoer` 可供所有项目使用。请二选一。CodeArts 官方规则是项目级同名 Skill 优先；若目标 Skill、vendor 或另一范围已有同名目录，立即停止，不要覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目中安装并验证项目级 andrej-karpathy Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v18.2.0，Commit 3bc6d8121ed9eeac844e6a652989251f883cb72a，复制源 skills/andrej-karpathy，目标目录 <项目>/.codeartsdoer/skills/andrej-karpathy，独立 vendor 目录 <项目>/.codeartsdoer/vendor/agentic-awesome-andrej-karpathy。先在项目根运行 codearts --version、git --version、codearts models，让我选择可用的 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-andrej-karpathy'，$target=Join-Path $root 'skills\andrej-karpathy'。检查 $source、$target 和 ~/.codeartsdoer/skills/andrej-karpathy，任一存在即停止；不得修改 package.json、codearts_cli.json、凭据、插件或其他 Skill。
在项目根依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 3bc6d8121ed9eeac844e6a652989251f883cb72a；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/andrej-karpathy/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 3bc6d8121ed9eeac844e6a652989251f883cb72a；确认 git -C $source rev-parse HEAD 精确等于该 Commit；Copy-Item -LiteralPath (Join-Path $source 'skills\andrej-karpathy') -Destination $target -Recurse。
运行 codearts debug skill，唯一 name=andrej-karpathy 的 location 必须是 $target/SKILL.md。将 <model> 替换成已选 provider/model ID 后原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name andrej-karpathy and use no other tool. Do not access files, commands, or the network. Apply the skill to this complete synthetic task: Add email validation to src/signup.js; validation must reject blank or missing-at-sign input before submit; existing valid submit behavior must remain unchanged; only src/signup.js and test/signup.test.js may change; the existing test command is npm test. Return explicit assumptions, the simplest surgical approach, a three-step verb-first plan where every step has a verification check, exact in-scope and out-of-scope files, and measurable success criteria. Do not implement code and do not invent repository facts.' 只有退出码 0、恰好一个来自 $target 的 name=andrej-karpathy/status=completed 事件、没有其他工具事件，且最终答案包含假设、最小方案、三步验证计划、准确文件范围和成功标准时通过。报告 Commit、绝对路径和事件。卸载只删除精确 $target 与 $source，再运行 codearts debug skill 确认旧路径消失；禁止删除整个 .codeartsdoer 或任何根配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 andrej-karpathy Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v18.2.0，Commit 3bc6d8121ed9eeac844e6a652989251f883cb72a，复制源 skills/andrej-karpathy，目标目录 ~/.codeartsdoer/skills/andrej-karpathy，独立 vendor 目录 ~/.codeartsdoer/vendor/agentic-awesome-andrej-karpathy。先运行 codearts --version、git --version、codearts models，让我选择可用的 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-andrej-karpathy'，$target=Join-Path $root 'skills\andrej-karpathy'。检查 $source、$target 以及将用于验证的全新 consumer 中 .codeartsdoer/skills/andrej-karpathy，任一存在即停止；不得修改用户 package.json、codearts_cli.json、凭据、插件或项目配置。
依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 3bc6d8121ed9eeac844e6a652989251f883cb72a；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/andrej-karpathy/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 3bc6d8121ed9eeac844e6a652989251f883cb72a；确认 HEAD 精确匹配；Copy-Item -LiteralPath (Join-Path $source 'skills\andrej-karpathy') -Destination $target -Recurse。进入一个没有项目级同名 Skill 的全新 consumer，运行 codearts debug skill，唯一 name=andrej-karpathy 的 location 必须是 $target/SKILL.md。替换 <model> 后原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name andrej-karpathy and use no other tool. Do not access files, commands, or the network. Apply the skill to this complete synthetic task: Add email validation to src/signup.js; validation must reject blank or missing-at-sign input before submit; existing valid submit behavior must remain unchanged; only src/signup.js and test/signup.test.js may change; the existing test command is npm test. Return explicit assumptions, the simplest surgical approach, a three-step verb-first plan where every step has a verification check, exact in-scope and out-of-scope files, and measurable success criteria. Do not implement code and do not invent repository facts.' 只有退出码 0、恰好一个来自 $target 的 name=andrej-karpathy/status=completed 事件、没有其他工具事件，且答案包含全部五类结果时通过。报告 Commit、绝对路径和事件。卸载只删除精确 $target、$source 和核对后的 consumer，再运行 codearts debug skill 确认旧路径消失；不得删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version` 和 `codearts models`。下例默认项目级；个人级只替换 `$root`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-andrej-karpathy'
$target=Join-Path $root 'skills\andrej-karpathy'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 3bc6d8121ed9eeac844e6a652989251f883cb72a
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/andrej-karpathy/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 3bc6d8121ed9eeac844e6a652989251f883cb72a
if((git -C $source rev-parse HEAD).Trim() -ne '3bc6d8121ed9eeac844e6a652989251f883cb72a'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\andrej-karpathy') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择真实可用的 `provider/model`。这个内容型 Skill 没有运行时依赖，不需要修改 `package.json` 或 `codearts_cli.json`。

## 验证

先用 `codearts debug skill` 核对唯一绝对路径，再运行上面提示词里的完整 `codearts run` 命令。成功判据是一个且仅一个来自目标目录的 completed Skill 事件、没有其他工具事件，并且答案完整交付五类计划信息。

## 使用

```text
Use andrej-karpathy. 在动手前列出假设和最小变更范围，把每一步改写成带验证方法的成功标准；不要顺手重构相邻代码。
```

## 更新

更新固定 Commit 前，重新审查 Skill 与许可证，并重新执行两个全新项目、个人级 consumer、核心调用和回滚验证。

## 卸载

只删除 `$root/skills/andrej-karpathy` 与 `$root/vendor/agentic-awesome-andrej-karpathy`，再用 `codearts debug skill` 确认旧绝对路径不再出现。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | Agentic Awesome `v18.2.0`；Commit `3bc6d8121ed9eeac844e6a652989251f883cb72a` |
| Skill / SHA-256 | `andrej-karpathy` / `FA7CD608E04C663477C452689EAB11207E9CCDA92AB3F689F780539910330FE8` |
| 许可证 | Skill frontmatter：MIT |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 无项目覆盖的个人级 consumer；2026-09-23 |

## 已知限制

它是行为约束，不会自动读取仓库事实、运行测试或替代项目自己的架构规范。测试使用的是受限合成计划任务；真实修改仍要由用户明确授权并在实际仓库验证。

## 安全

固定目录只有一个 4,264 字节的 `SKILL.md`，没有 manifest、锁文件、脚本、二进制、下载器、网络调用、凭据或遥测。主验证与发布流的六次最终调用都只有目标 Skill 事件。

## 证据与来源

- [中文研究记录](../../research/2026-09-23.md) · [English](../../research/2026-09-23.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/3bc6d8121ed9eeac844e6a652989251f883cb72a/skills/andrej-karpathy)
- [原始项目](https://github.com/multica-ai/andrej-karpathy-skills)
- [Agentic Awesome v18.2.0](https://github.com/sickn33/agentic-awesome-skills/releases/tag/v18.2.0)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
