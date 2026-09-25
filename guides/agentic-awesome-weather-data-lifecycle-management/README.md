# 在 CodeArts CLI 中使用 weather-data-lifecycle-management

[English](README.en.md)

安装 `weather-data-lifecycle-management` Skill，让 Agent 在天气数据工作流中先划清临时文件、交互会话、共享缓存和用户导出的所有权，再设计安全、可复现的清理时机。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 只影响当前仓库；个人级 `~/.codeartsdoer` 可供所有项目使用。请二选一。CodeArts 官方规则是项目级同名 Skill 优先；若目标 Skill、vendor 或另一范围已有同名目录，立即停止，不要覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目中安装并验证项目级 weather-data-lifecycle-management Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v18.4.0，Commit 7b534bc15d833baf3bc98b3ca4fb23eda48342bb，复制源 skills/weather-data-lifecycle-management，目标目录 <项目>/.codeartsdoer/skills/weather-data-lifecycle-management，独立 vendor 目录 <项目>/.codeartsdoer/vendor/agentic-awesome-weather-data-lifecycle-management。先在项目根运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-weather-data-lifecycle-management'，$target=Join-Path $root 'skills\weather-data-lifecycle-management'。检查 $source、$target 和 ~/.codeartsdoer/skills/weather-data-lifecycle-management，任一存在即停止；不得修改 package.json、codearts_cli.json、凭据、插件、权限文件或其他 Skill。
在项目根依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout --branch v18.4.0 --single-branch https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source sparse-checkout init --cone；git -C $source sparse-checkout set skills/weather-data-lifecycle-management；git -C $source checkout v18.4.0；确认 git -C $source rev-parse HEAD 精确等于 7b534bc15d833baf3bc98b3ca4fb23eda48342bb；Copy-Item -LiteralPath (Join-Path $source 'skills\weather-data-lifecycle-management') -Destination $target -Recurse。
运行 codearts debug skill，唯一 name=weather-data-lifecycle-management 的 location 必须是 $target/SKILL.md。将 <model> 替换成已选 provider/model ID 后原样运行：codearts run --format json -m "<model>" 'Call the skill tool exactly once with name weather-data-lifecycle-management and use no other tool. Do not access or modify files, run commands, or access the network. Design the lifecycle for this synthetic desktop flow: worker creates request directory R with partial.grib2 and view.nc; a viewer may outlive the worker; validated cache entry C is shared by two viewers; final user export E is outside R; viewer construction can fail; cancellation can race with viewer close. Return an ownership table and exact events: worker cleans partials and R on failure, transfers view.nc ownership only after successful viewer creation, final consumer close performs one idempotent cleanup after handles close, shared cache C is never request cleanup, and export E is never automatically deleted. Explicitly refuse deletion when ownership is unknown and do not claim cleanup ran.' 只有退出码 0、恰好一个来自 $target 的 name=weather-data-lifecycle-management/status=completed 事件、没有其他工具事件，且最终答案包含所有权表、失败清理、成功后所有权转移、最终关闭清理、共享缓存/用户导出边界和未知所有权拒绝时通过。报告 Commit、绝对路径和事件。卸载只删除精确 $target 与 $source，再运行 codearts debug skill 确认旧路径消失；禁止删除整个 .codeartsdoer、项目根、用户根或任何根配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 weather-data-lifecycle-management Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v18.4.0，Commit 7b534bc15d833baf3bc98b3ca4fb23eda48342bb，复制源 skills/weather-data-lifecycle-management，目标目录 ~/.codeartsdoer/skills/weather-data-lifecycle-management，独立 vendor 目录 ~/.codeartsdoer/vendor/agentic-awesome-weather-data-lifecycle-management。先运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-weather-data-lifecycle-management'，$target=Join-Path $root 'skills\weather-data-lifecycle-management'。检查 $source、$target 以及用于验证的全新 consumer 中 .codeartsdoer/skills/weather-data-lifecycle-management，任一存在即停止；不得修改用户 package.json、codearts_cli.json、凭据、插件、权限文件或项目配置。
依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --filter=blob:none --no-checkout --branch v18.4.0 --single-branch https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source config core.longpaths true；git -C $source sparse-checkout init --cone；git -C $source sparse-checkout set skills/weather-data-lifecycle-management；git -C $source checkout v18.4.0；确认 HEAD 精确匹配 7b534bc15d833baf3bc98b3ca4fb23eda48342bb；Copy-Item -LiteralPath (Join-Path $source 'skills\weather-data-lifecycle-management') -Destination $target -Recurse。进入一个没有项目级同名 Skill 的全新 consumer，运行 codearts debug skill，唯一 name=weather-data-lifecycle-management 的 location 必须是 $target/SKILL.md。替换 <model> 后原样运行：codearts run --format json -m "<model>" 'Call the skill tool exactly once with name weather-data-lifecycle-management and use no other tool. Do not access or modify files, run commands, or access the network. Design the lifecycle for this synthetic desktop flow: worker creates request directory R with partial.grib2 and view.nc; a viewer may outlive the worker; validated cache entry C is shared by two viewers; final user export E is outside R; viewer construction can fail; cancellation can race with viewer close. Return an ownership table and exact events: worker cleans partials and R on failure, transfers view.nc ownership only after successful viewer creation, final consumer close performs one idempotent cleanup after handles close, shared cache C is never request cleanup, and export E is never automatically deleted. Explicitly refuse deletion when ownership is unknown and do not claim cleanup ran.' 只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、没有其他工具事件，且答案包含全部六类结果时通过。报告 Commit、绝对路径和事件。卸载只删除精确 $target、$source 和核对后的 consumer，再运行 codearts debug skill 确认旧路径消失；不得删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version` 和 `codearts models`。下例默认项目级；个人级只替换 `$root`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-weather-data-lifecycle-management'
$target=Join-Path $root 'skills\weather-data-lifecycle-management'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout --branch v18.4.0 --single-branch https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set skills/weather-data-lifecycle-management
git -C $source checkout v18.4.0
if((git -C $source rev-parse HEAD).Trim() -ne '7b534bc15d833baf3bc98b3ca4fb23eda48342bb'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\weather-data-lifecycle-management') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择真实可用的 `provider/model`。这个内容型 Skill 没有运行时依赖，不需要修改 `package.json`、`codearts_cli.json` 或权限文件。

## 验证

先用 `codearts debug skill` 核对唯一绝对路径，再运行上面提示词里的完整 `codearts run` 命令。成功判据是一个且仅一个来自目标目录的 completed Skill 事件、没有其他工具事件，并且答案完整覆盖所有权、转移、失败、关闭、缓存/导出边界和未知所有权拒绝。

## 使用

```text
Use weather-data-lifecycle-management. 为这个天气数据流程列出每个临时文件、缓存和导出的所有者与删除事件；成功、失败、取消和最终消费者关闭分别说明，并拒绝删除所有权不明的路径。先给方案，不要实际删除文件。
```

## 更新

更新固定 Commit 前，重新审查 Skill 与许可证，并重新执行两个全新项目、个人级 consumer、核心调用和回滚验证。

## 卸载

只删除 `$root/skills/weather-data-lifecycle-management` 与 `$root/vendor/agentic-awesome-weather-data-lifecycle-management`，再用 `codearts debug skill` 确认旧绝对路径不再出现。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | Agentic Awesome `v18.4.0`；Commit `7b534bc15d833baf3bc98b3ca4fb23eda48342bb` |
| Skill / SHA-256 | `weather-data-lifecycle-management` / `32FC91E27B27ED7F9AD188885BF82D26034E2754B7CE52630755FC9BFB0354F8` |
| 许可证 | 原创非代码内容 CC BY 4.0；仓库代码与工具 MIT；该 Skill frontmatter 标记 `source: self` |
| 环境 | CodeArts CLI 26.8.1；Windows 11 25H2 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 无项目覆盖的个人级 consumer；2026-09-25 |

## 已知限制

本次验证覆盖的是只读生命周期设计，不是实际删除执行。Skill 不会替代应用里的锁、租约、跨进程协调或缓存驱逐实现；真实清理前仍须解析并核对绝对路径、关闭句柄，并用项目测试验证成功、失败、取消和竞争条件。

## 安全

固定目录只有一个 Markdown `SKILL.md`，没有 manifest、锁文件、脚本、二进制、下载器、运行时网络、凭据或遥测。主验证与发布流程的 6 次最终调用都只有目标 Skill 事件；用户根 `package.json`、`codearts_cli.json` 和持久权限文件哈希在回滚后保持不变。

## 证据与来源

- [中文研究记录](../../research/2026-09-25.md) · [English](../../research/2026-09-25.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/7b534bc15d833baf3bc98b3ca4fb23eda48342bb/skills/weather-data-lifecycle-management)
- [Agentic Awesome v18.4.0](https://github.com/sickn33/agentic-awesome-skills/releases/tag/v18.4.0)
- [AAS 内容许可证](https://github.com/sickn33/agentic-awesome-skills/blob/7b534bc15d833baf3bc98b3ca4fb23eda48342bb/LICENSE-CONTENT)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
