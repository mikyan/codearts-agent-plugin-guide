# 在 CodeArts CLI 中使用 team-communications

[English](README.en.md)

安装 `team-communications`，把团队事实压缩成清晰的 3P（Progress、Plans、Problems）更新。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 用于团队固定版本；个人级 `~/.codeartsdoer` 用于当前用户跨项目复用。只选一种，任一范围已有同名 Skill 就停止。

## 让 Agent 帮你安装

### 项目级提示词

```text
安装并验证项目级 alirezarezvani/claude-skills team-communications，固定 Commit aa8d778811a557a2c28ccadda4cf3d0bd028a4cc。
1. 只创建 .codeartsdoer/vendor/claude-skills-team-communications 和 .codeartsdoer/skills/team-communications；不改用户目录、package.json、codearts_cli.json、凭据或其他 Skill。运行 codearts --version、git --version、codearts models 并让我选模型；缺凭据或项目/个人同名冲突就停止。
2. 在项目根 PowerShell 执行：
   $s=Join-Path (Get-Location) '.codeartsdoer\vendor\claude-skills-team-communications'; $t=Join-Path (Get-Location) '.codeartsdoer\skills\team-communications'
   New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/alirezarezvani/claude-skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/project-management/skills/team-communications/' '/LICENSE'
   git -C $s checkout --detach aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
   if((git -C $s rev-parse HEAD).Trim() -ne 'aa8d778811a557a2c28ccadda4cf3d0bd028a4cc'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'project-management\skills\team-communications') -Destination $t -Recurse
3. `codearts debug skill` 唯一同名来源必须是 $t/SKILL.md。把 <模型> 换成已选 ID，原样运行：codearts run --format json --sandbox --model "<模型>" "Verification contract: call the skill tool exactly once with name team-communications, then use the read tool exactly once on the loaded skill's references/3p-updates.md. No other tool is allowed. Based only on those sources, write a three-line 3P update headed Platform Team (Aug 10–16): Progress shipped the Windows installer to 100% and cut setup from 20 to 8 minutes; Plans add rollback telemetry next week; Problems two legacy proxies block automatic updates. Use the exact labels Progress:, Plans:, Problems:."
4. 通过必须为退出 0、一个 completed skill 事件、一个 completed read 事件、无其他工具，结果含标题、三个标签和全部事实。报告证据；卸载仅删 $t 与 $s，禁止删整个根或其他配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装个人级 team-communications，固定 Commit aa8d778811a557a2c28ccadda4cf3d0bd028a4cc。
1. 只创建 ~/.codeartsdoer/vendor/claude-skills-team-communications 和 ~/.codeartsdoer/skills/team-communications；不改根 package.json、codearts_cli.json、凭据、插件或项目。运行版本/models 检查并让我选模型；同名冲突或缺凭据就停止。
2. PowerShell 执行：
   $u=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'; $s=Join-Path $u 'vendor\claude-skills-team-communications'; $t=Join-Path $u 'skills\team-communications'; $c=Join-Path ([IO.Path]::GetTempPath()) 'codearts-team-comms-aa8d778-smoke'
   if((Test-Path $s)-or(Test-Path $t)-or(Test-Path $c)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent),$c -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/alirezarezvani/claude-skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/project-management/skills/team-communications/' '/LICENSE'
   git -C $s checkout --detach aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
   if((git -C $s rev-parse HEAD).Trim() -ne 'aa8d778811a557a2c28ccadda4cf3d0bd028a4cc'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'project-management\skills\team-communications') -Destination $t -Recurse; Set-Location $c
3. debug skill 必须解析到 $t/SKILL.md；把 <模型> 换成已选 ID 并运行：codearts run --format json --sandbox --model "<模型>" "Verification contract: call the skill tool exactly once with name team-communications, then use the read tool exactly once on the loaded skill's references/3p-updates.md. No other tool is allowed. Based only on those sources, write a three-line 3P update headed Platform Team (Aug 10–16): Progress shipped the Windows installer to 100% and cut setup from 20 to 8 minutes; Plans add rollback telemetry next week; Problems two legacy proxies block automatic updates. Use the exact labels Progress:, Plans:, Problems:." 通过要求一个 completed Skill、一个 completed read、无其他工具，并包含标题、三个标签和全部事实。卸载只删 $t、$s、核对后的 $c，禁止删用户根或其他配置。
```

## Windows 手动安装

运行 `codearts --version`、`git --version`、`codearts models`，选择一个范围，执行对应提示词中的完整固定检出和目录复制。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级改为：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$s=Join-Path $root 'vendor\claude-skills-team-communications'; $t=Join-Path $root 'skills\team-communications'
if((Test-Path $s)-or(Test-Path $t)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
git clone --filter=blob:none --no-checkout https://github.com/alirezarezvani/claude-skills.git $s
git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/project-management/skills/team-communications/' '/LICENSE'
git -C $s checkout --detach aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
Copy-Item -LiteralPath (Join-Path $s 'project-management\skills\team-communications') -Destination $t -Recurse
```

## CodeArts 配置

凭据只按官方方式配置，不要写入仓库或输出。

## 验证

用 `codearts debug skill` 核对来源，再执行真实命令；Skill 与相对引用文件两个 completed 事件都是成功条件。

## 使用

```text
Use team-communications. 把这些事实改为给管理层看的 3P 周报，不添加事实：<事实>
```

## 更新

用 `git -C $s rev-parse HEAD` 检查版本；升级需重新审查与三环境验证。

## 卸载

只删所选范围的 `skills/team-communications` 与 `vendor/claude-skills-team-communications`，再确认不被发现。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | release `v2.9.0`；Commit `aa8d778811a557a2c28ccadda4cf3d0bd028a4cc` |
| Skill / SHA-256 | `team-communications` / `50594ECF84B29CB6C35C46C536EA9B21F47A3ED929D76F6F8B3BA43FABFA6A53` |
| 许可证 | MIT |
| 环境与范围 | CodeArts 26.8.1；Windows 11 Build 26200；MiMo；两个项目 + 个人级；2026-08-20 |

## 已知限制

只验证 3P 更新和 `references/3p-updates.md` 的相对读取，未验证其他模板或仓库 Skill。

## 安全

固定目录 5 个文本文件、14,181 字节；不执行 npm、脚本、网络、凭据或遥测。输出可能包含内部事实，使用者应先脱敏。

## 证据与来源

- [中文研究](../../research/2026-08-20.md) · [English](../../research/2026-08-20.en.md)
- [固定 Commit](https://github.com/alirezarezvani/claude-skills/tree/aa8d778811a557a2c28ccadda4cf3d0bd028a4cc/project-management/skills/team-communications)
- [MIT License](https://github.com/alirezarezvani/claude-skills/blob/aa8d778811a557a2c28ccadda4cf3d0bd028a4cc/LICENSE)
- [CodeArts Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
