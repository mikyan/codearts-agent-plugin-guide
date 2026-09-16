# 在 CodeArts CLI 中使用 cross-platform-contract-propagation-audit

[English](README.en.md)

安装 `cross-platform-contract-propagation-audit` Skill，以只读方式追踪字段、枚举、功能开关或 API 合同是否贯穿存储、服务、客户端、分析与测试。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合固定一个仓库；个人级 `~/.codeartsdoer` 适合跨项目复用。二选一；项目级同名 Skill 优先。目标 Skill 或 vendor 路径存在时必须停止。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目中安装并验证项目级 cross-platform-contract-propagation-audit。固定来源 https://github.com/sickn33/agentic-awesome-skills.git，release v17.3.0，Commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294，源目录 skills/cross-platform-contract-propagation-audit。
先在项目根运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-cross-platform-contract-propagation-audit'，$target=Join-Path $root 'skills\cross-platform-contract-propagation-audit'。检查 $source、$target 和 ~/.codeartsdoer/skills/cross-platform-contract-propagation-audit；任一冲突就停止。只创建 $source 和 $target，不修改 package.json、codearts_cli.json、凭据、插件或其他 Skill。
从项目根依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/cross-platform-contract-propagation-audit/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294。确认 git -C $source rev-parse HEAD 精确等于该 Commit，再执行 Copy-Item -LiteralPath (Join-Path $source 'skills\cross-platform-contract-propagation-audit') -Destination $target -Recurse。
运行 codearts debug skill；唯一生效的同名 location 必须是 $target/SKILL.md。把 <model> 换成已选 ID，原样执行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name cross-platform-contract-propagation-audit. Use no other tool, do not access the network, and do not read or write files. Audit only these synthetic facts for nullable can_complete: database and detail API contain it; list API omits it; Web hides the action when the field is missing; Android explicit-null behavior is untested; feature flag is evaluated in detail API only; click analytics omits capability and cohort; no runtime tests were executed. Return exact sections CONTRACT, PROPAGATION GRAPH, STATUS TABLE, STATE MATRIX, RELEASE GATES, VERDICT, and LIMITS. Mark the list projection and analytics gaps missing, Android and unexecuted cells unknown, and conclude blocked rather than complete.'
只有退出码 0、恰好一个来自 $target 的 name=cross-platform-contract-propagation-audit/status=completed 事件、没有其他工具事件，且七个章节都把 list/analytics 标为 missing、Android 与未执行单元格标为 unknown、结论为 blocked 时通过。报告 Commit、绝对路径和事件。卸载只能删除精确 $target 与 $source，再用 codearts debug skill 确认旧路径消失；禁止删除整个 .codeartsdoer 或任何根配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 cross-platform-contract-propagation-audit。固定来源 https://github.com/sickn33/agentic-awesome-skills.git，release v17.3.0，Commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294，源目录 skills/cross-platform-contract-propagation-audit。
运行 codearts --version、git --version、codearts models，让我选择 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-cross-platform-contract-propagation-audit'，$target=Join-Path $root 'skills\cross-platform-contract-propagation-audit'。确认 $source、$target 及全新 consumer 的同名项目 Skill 都不存在；冲突时停止。不得修改用户 package.json、codearts_cli.json、凭据、插件或项目配置。
依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；git -C $source sparse-checkout init --no-cone；git -C $source sparse-checkout set --no-cone '/skills/cross-platform-contract-propagation-audit/' '/LICENSE' '/LICENSE-CONTENT'；git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294；确认 HEAD 后，Copy-Item -LiteralPath (Join-Path $source 'skills\cross-platform-contract-propagation-audit') -Destination $target -Recurse。
进入没有同名项目 Skill 的全新 consumer。codearts debug skill 的唯一同名 location 必须是 $target/SKILL.md。把 <model> 换成已选 ID，原样执行：codearts run --format json --model "<model>" 'Call the skill tool exactly once with name cross-platform-contract-propagation-audit. Use no other tool, do not access the network, and do not read or write files. Audit only these synthetic facts for nullable can_complete: database and detail API contain it; list API omits it; Web hides the action when the field is missing; Android explicit-null behavior is untested; feature flag is evaluated in detail API only; click analytics omits capability and cohort; no runtime tests were executed. Return exact sections CONTRACT, PROPAGATION GRAPH, STATUS TABLE, STATE MATRIX, RELEASE GATES, VERDICT, and LIMITS. Mark the list projection and analytics gaps missing, Android and unexecuted cells unknown, and conclude blocked rather than complete.' 只有退出码 0、恰好一个来自 $target 的 name=cross-platform-contract-propagation-audit/status=completed 事件、没有其他工具事件，且七个章节满足所有 missing/unknown/blocked 判据时通过。报告 Commit、绝对路径和事件。卸载只删除精确 $target、$source 和核对后的 consumer，再用 codearts debug skill 确认旧路径消失；不得删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version`、`codearts models`。下例默认项目级；个人级只替换 `$root`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-cross-platform-contract-propagation-audit'
$target=Join-Path $root 'skills\cross-platform-contract-propagation-audit'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/cross-platform-contract-propagation-audit/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\cross-platform-contract-propagation-audit') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择可用的 `provider/model`。凭据只保存在官方用户配置中；本 Skill 不要求修改 `package.json` 或 `codearts_cli.json`。

## 验证

先用 `codearts debug skill` 核对唯一来源，再运行 Agent 提示词中的完整 `codearts run`。成功判据是退出码 0、唯一 completed 同名 Skill 事件来自目标绝对路径、没有其他工具事件，并输出可追溯的状态表、矩阵、发布门槛和 blocked 结论。

## 使用

```text
Use cross-platform-contract-propagation-audit. 只读审计这个字段从数据库到各 API、客户端、分析事件与测试的传播；把 missing、null、false、true 和未知枚举分别列入状态矩阵，缺证据时写 unknown。
```

## 更新

用 `git -C $source rev-parse HEAD` 核对固定版本。更新 Commit 前重新审查许可证和固定目录全部内容，并重做两个项目、个人范围和回滚测试。

## 卸载

只删除 `$root/skills/cross-platform-contract-propagation-audit` 与 `$root/vendor/agentic-awesome-cross-platform-contract-propagation-audit`，再用 `codearts debug skill` 确认旧路径消失。不得删除整个 `.codeartsdoer`、根配置或其他 Skill。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | release `v17.3.0`；Commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `cross-platform-contract-propagation-audit` / `EECA23D5CC381ED3CB3B816004DBF7401FA30C11EBEDF9D58A333B0A95415655` |
| 内容许可证 | CC BY 4.0（AAS `LICENSE-CONTENT`） |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-09-16 |

## 已知限制

只验证了用户直接提供事实的只读合成审计，没有读真实仓库、生成代码、运行测试或更改功能开关。使用时必须先给出业务不变量和范围；未提供的 mapper、客户端或运行时事实应保持 `unknown`。

## 安全

固定目录只有一个 9,238 字节的 `SKILL.md`，无依赖、锁文件、生命周期脚本、二进制、下载器或遥测。三次实测均只有目标 Skill 事件，没有文件、Shell 或网络访问。

## 证据与来源

- [中文研究记录](../../research/2026-09-16.md) · [English](../../research/2026-09-16.en.md)
- [固定 Skill 目录](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/cross-platform-contract-propagation-audit)
- [AAS 内容许可证](https://github.com/sickn33/agentic-awesome-skills/blob/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/LICENSE-CONTENT)
- [CodeArts CLI Skills 官方文档](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
