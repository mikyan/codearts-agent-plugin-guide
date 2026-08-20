# 在 CodeArts CLI 中使用 ab-testing

[English](README.en.md)

安装 `ab-testing` Skill，用可复现的样本量、假设、主指标和护栏指标设计 A/B 测试。

## 选择安装范围

先且只选一种：项目级 `<项目>/.codeartsdoer` 适合团队固定版本；个人级 `~/.codeartsdoer` 适合当前 Windows 用户跨项目复用。CLI 26.8.1 的同名解析实测与官方优先级说明存在差异，因此任一范围已有 `ab-testing` 时都应停止。

## 让 Agent 帮你安装

### 项目级提示词

```text
请在当前项目根目录安装并验证项目级 sickn33/agentic-awesome-skills 的 ab-testing Skill，固定 Commit e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5。
1. 只创建 <项目>/.codeartsdoer/vendor/agentic-awesome-ab-testing 和 <项目>/.codeartsdoer/skills/ab-testing；不得修改 ~/.codeartsdoer、package.json、codearts_cli.json、凭据或其他 Skill。
2. 运行 codearts --version、git --version、codearts models，让我选择可用 provider/model；缺少凭据就停止，禁止读取或打印秘密。
3. 检查上述两个项目路径及 ~/.codeartsdoer/skills/ab-testing；任一存在就停止，不得覆盖。
4. 在项目根目录 PowerShell 执行：
   $source=Join-Path (Get-Location) '.codeartsdoer\vendor\agentic-awesome-ab-testing'; $target=Join-Path (Get-Location) '.codeartsdoer\skills\ab-testing'
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
   git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
   git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/plugins/agentic-awesome-skills-claude/skills/ab-testing/' '/LICENSE'
   git -C $source checkout --detach e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
   if((git -C $source rev-parse HEAD).Trim() -ne 'e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $source 'plugins\agentic-awesome-skills-claude\skills\ab-testing') -Destination $target -Recurse
5. 运行 codearts debug skill；唯一的 ab-testing location 必须是项目目标 SKILL.md。
6. 把 <模型> 换成已选 ID，原样运行：codearts run --format json --sandbox --model "<模型>" "Verification contract: call the skill tool exactly once with name ab-testing. No other tool is allowed. Using the loaded skill, design a signup-page A/B test with 5% baseline, 20% relative MDE, 2,000 eligible visitors/day, and 50/50 split. State exactly 18,000 per variant and 18 days, one hypothesis, one primary metric, and one guardrail metric."
7. 成功必须为退出 0、恰好一个 name=ab-testing 且 status=completed 的 skill 事件、无其他工具事件，结果含指定数字及四个部分。
8. 报告 Commit、来源路径、事件、结果和卸载清单。卸载仅可删除上述 target 与 source；禁止删除整个 .codeartsdoer、package.json、ProjectSkillStatus.txt 或其他 Skill。失败必须如实停止。
```

### 个人级提示词

```text
请为当前 Windows 用户安装并验证个人级 ab-testing，固定 sickn33/agentic-awesome-skills Commit e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5。
1. 只创建 ~/.codeartsdoer/vendor/agentic-awesome-ab-testing 与 ~/.codeartsdoer/skills/ab-testing；不得修改用户根 package.json、codearts_cli.json、凭据、插件或项目配置。
2. 运行 codearts --version、git --version、codearts models，让我选择可用模型；凭据不足时停止。确认两个用户目标与验证目录中的 .codeartsdoer/skills/ab-testing 都不存在；冲突时停止。
3. 在 PowerShell 执行：
   $u=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'; $source=Join-Path $u 'vendor\agentic-awesome-ab-testing'; $target=Join-Path $u 'skills\ab-testing'; $consumer=Join-Path ([IO.Path]::GetTempPath()) 'codearts-ab-testing-e2b6ad1-smoke'
   if((Test-Path $source)-or(Test-Path $target)-or(Test-Path $consumer)){throw 'Collision'}
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent),$consumer -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
   git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
   git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/plugins/agentic-awesome-skills-claude/skills/ab-testing/' '/LICENSE'
   git -C $source checkout --detach e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
   if((git -C $source rev-parse HEAD).Trim() -ne 'e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $source 'plugins\agentic-awesome-skills-claude\skills\ab-testing') -Destination $target -Recurse; Set-Location $consumer
4. 运行 codearts debug skill；唯一匹配必须是用户目标。把 <模型> 换成已选 ID 并原样运行：codearts run --format json --sandbox --model "<模型>" "Verification contract: call the skill tool exactly once with name ab-testing. No other tool is allowed. Using the loaded skill, design a signup-page A/B test with 5% baseline, 20% relative MDE, 2,000 eligible visitors/day, and 50/50 split. State exactly 18,000 per variant and 18 days, one hypothesis, one primary metric, and one guardrail metric."
5. 仅当退出 0、恰好一个 completed ab-testing Skill 事件、无其他工具且结果包含全部数字与部分时通过。卸载只能删除 $target、$source 和核对后的 $consumer；禁止删除用户根、package.json、codearts_cli.json、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version`、`codearts models`。把 `$root` 设为项目根的 `.codeartsdoer`，或个人级 `~/.codeartsdoer`；只选一种并先检查两种范围无同名冲突。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级改为 Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-ab-testing'; $target=Join-Path $root 'skills\ab-testing'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/plugins/agentic-awesome-skills-claude/skills/ab-testing/' '/LICENSE'
git -C $source checkout --detach e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
Copy-Item -LiteralPath (Join-Path $source 'plugins\agentic-awesome-skills-claude\skills\ab-testing') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择真实可用模型；按官方文档配置凭据，不写入仓库。

## 验证

用 `codearts debug skill` 核对唯一来源，再执行提示词中的真实 `codearts run` 命令和成功断言。

## 使用

```text
Use ab-testing. 基线转化率 8%，请先澄清 MDE、流量和护栏指标，再给出测试计划。
```

## 更新

用 `git -C $source rev-parse HEAD` 检查固定版本。新 Commit 必须重新审查许可证、Skill、脚本和依赖，并重做三环境验证。

## 卸载

精确解析并只删除 `$root/skills/ab-testing` 与 `$root/vendor/agentic-awesome-ab-testing`，随后 `codearts debug skill` 不应再发现它。不得删除整个 `.codeartsdoer`、根配置或其他 Skill。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | release `v15.15.0`；Commit `e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5` |
| Skill / SHA-256 | `ab-testing` / `4594DD8DE675AC8B383032E58D8954BCAA45E2EC26CC637E3817A7C381C6E8AF` |
| 许可证 | MIT |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-08-20 |

## 已知限制

只验证所选 Skill，不代表仓库其他插件。样本量参考表不替代专业统计审查；同名解析以 `codearts debug skill` 为准。

## 安全

固定目录为 4 个文本文件、34,491 字节；不运行 npm、生命周期脚本、二进制、网络、凭据或遥测。

## 证据与来源

- [中文研究记录](../../research/2026-08-20.md) · [English](../../research/2026-08-20.en.md)
- [固定 Commit](https://github.com/sickn33/agentic-awesome-skills/tree/e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5/plugins/agentic-awesome-skills-claude/skills/ab-testing)
- [MIT License](https://github.com/sickn33/agentic-awesome-skills/blob/e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5/LICENSE)
- [CodeArts CLI Skills 官方文档](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
