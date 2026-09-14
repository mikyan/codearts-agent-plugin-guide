# 在 CodeArts CLI 中使用 documentation

[English](README.en.md)

安装 `documentation` Skill，为 README、API、架构、运维和发布文档建立受众、信息架构、责任人与质量门槛。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合统一单个仓库的文档流程；个人级 `~/.codeartsdoer` 适合跨项目复用。二选一；项目级同名 Skill 优先。目标 Skill 或 vendor 路径存在时必须停止，不能覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目中安装并验证项目级 documentation。固定来源 https://github.com/sickn33/agentic-awesome-skills.git，release v17.2.0，Commit 2fce708d4f3871ccce3d705b4748371f94c730ec，源目录 skills/documentation。
先运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-documentation'，$target=Join-Path $root 'skills\documentation'。检查 $source、$target 和 ~/.codeartsdoer/skills/documentation；任一冲突就停止。只创建 $source 和 $target，不修改 package.json、codearts_cli.json、凭据、插件或其他 Skill。
从项目根依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/documentation/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec。确认 git -C $source rev-parse HEAD 精确等于该 Commit，再执行 Copy-Item -LiteralPath (Join-Path $source 'skills\documentation') -Destination $target -Recurse。
运行 codearts debug skill；唯一生效的 documentation location 必须是 $target/SKILL.md。把 <model> 换成已选 ID，原样执行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name documentation. Use no other tool, do not access the network, and do not read or write files. Create a documentation plan for a synthetic local REST service that has a README but lacks API reference, architecture overview, troubleshooting, and release notes. The audience is new contributors and operators. Return exact sections AUDIENCES, INFORMATION ARCHITECTURE, DELIVERABLES, OWNERSHIP, QUALITY GATES, and OPEN QUESTIONS. Include link checking, executable example validation, review cadence, and state that no repository content was inspected.'
只有退出码 0、恰好一个来自 $target 的 name=documentation/status=completed 事件、没有其他工具事件、最终文本含六个章节及全部要求时通过。报告 Commit、绝对路径和事件。卸载只能删除精确 $target 与 $source，再用 codearts debug skill 确认旧路径消失；禁止删除整个 .codeartsdoer 或任何根配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 documentation。固定来源 https://github.com/sickn33/agentic-awesome-skills.git，release v17.2.0，Commit 2fce708d4f3871ccce3d705b4748371f94c730ec，源目录 skills/documentation。
运行 codearts --version、git --version、codearts models，让我选择 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-documentation'，$target=Join-Path $root 'skills\documentation'。确认 $source、$target 及全新 consumer 的 .codeartsdoer/skills/documentation 都不存在；冲突时停止。不得修改用户 package.json、codearts_cli.json、凭据、插件或项目配置。
依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/documentation/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec；确认 HEAD 后，Copy-Item -LiteralPath (Join-Path $source 'skills\documentation') -Destination $target -Recurse。
进入没有同名项目 Skill 的全新 consumer。codearts debug skill 的唯一 documentation location 必须是 $target/SKILL.md。把 <model> 换成已选 ID，原样运行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name documentation. Use no other tool, do not access the network, and do not read or write files. Create a documentation plan for a synthetic local REST service that has a README but lacks API reference, architecture overview, troubleshooting, and release notes. The audience is new contributors and operators. Return exact sections AUDIENCES, INFORMATION ARCHITECTURE, DELIVERABLES, OWNERSHIP, QUALITY GATES, and OPEN QUESTIONS. Include link checking, executable example validation, review cadence, and state that no repository content was inspected.'
沿用项目级成功判据。卸载只删除精确 $target、$source 和核对后的 consumer；不得删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version`、`codearts models`。下例默认项目级；个人级只替换 `$root`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-documentation'; $target=Join-Path $root 'skills\documentation'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/documentation/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec
if((git -C $source rev-parse HEAD).Trim() -ne '2fce708d4f3871ccce3d705b4748371f94c730ec'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\documentation') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择可用的 `provider/model`。凭据只保存在官方用户配置中；本 Skill 不要求修改 `package.json` 或 `codearts_cli.json`。

## 验证

先用 `codearts debug skill` 核对唯一来源，再运行 Agent 提示词中的完整 `codearts run`。成功判据是退出码 0、唯一 completed `documentation` 事件来自目标绝对路径、没有其他工具事件，且六个章节同时包含链接检查、可执行示例验证、审查周期与“未检查仓库”声明。

## 使用

```text
Use documentation. 为这个仓库规划中文优先的 README、API、架构、运维、故障排查和发布文档；先说明受众与缺失证据，再给信息架构、责任人和可自动化质量门槛。
```

## 更新

用 `git -C $source rev-parse HEAD` 核对固定版本。更新 Commit 前重新审查许可证和 `skills/documentation` 全部内容，并重做两个项目、个人范围和回滚测试。

## 卸载

解析绝对路径后，只删除 `$root/skills/documentation` 与 `$root/vendor/agentic-awesome-documentation`，再用 `codearts debug skill` 确认旧路径消失。不得删除整个 `.codeartsdoer`、根配置或其他 Skill。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | release `v17.2.0`；Commit `2fce708d4f3871ccce3d705b4748371f94c730ec` |
| Skill / SHA-256 | `documentation` / `DCF5CE03E82B0ACBABE2C2DDF3B8C217FB35989E931F9F211B1A9ADBE03A7FFC` |
| 内容许可证 | CC BY 4.0（AAS `LICENSE-CONTENT`） |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-09-14 |

## 已知限制

只验证了只读文档规划，没有生成或修改实际文档，也没有验证正文提到的其他组合 Skill、文档站点或 CI。三次输出会提出不同工具与目录建议，落地前仍需以仓库现状为准。

## 安全

固定目录只有一个 5,930 字节的 `SKILL.md`，无依赖、锁文件、生命周期脚本、二进制、下载器或遥测。三次实测均只有目标 Skill 事件，没有文件、Shell 或网络访问。

## 证据与来源

- [中文研究记录](../../research/2026-09-14.md) · [English](../../research/2026-09-14.en.md)
- [固定 Skill 目录](https://github.com/sickn33/agentic-awesome-skills/tree/2fce708d4f3871ccce3d705b4748371f94c730ec/skills/documentation)
- [AAS 内容许可证](https://github.com/sickn33/agentic-awesome-skills/blob/2fce708d4f3871ccce3d705b4748371f94c730ec/LICENSE-CONTENT)
- [CodeArts CLI Skills 官方文档](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
