# 在 CodeArts CLI 中使用 ui-lint

[English](README.en.md)

安装 `ui-lint` Skill，对 React/JSX 界面代码执行设计令牌、逻辑属性、尺寸与组件契约检查。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合团队固定版本；个人级 `~/.codeartsdoer` 适合当前 Windows 用户复用。二选一，并先用 `codearts debug skill` 检查同名项；项目级同名 Skill 会优先于个人级，任何目标或 vendor 路径已存在时都停止，禁止覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
请在当前项目根目录安装并验证项目级 ui-lint，来源 https://github.com/sickn33/agentic-awesome-skills.git，固定 Commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3，源目录 skills/ui-lint。
1. 只允许创建 <项目>/.codeartsdoer/vendor/agentic-awesome-ui-lint 和 <项目>/.codeartsdoer/skills/ui-lint；不得修改 ~/.codeartsdoer、package.json、codearts_cli.json、凭据或其他 Skill。
2. 运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；缺少凭据就停止，禁止读取或打印秘密。
3. 检查上述两个项目路径以及 ~/.codeartsdoer/skills/ui-lint；任一冲突就停止，不得覆盖。
4. 在项目根目录执行下方“Windows 手动安装”命令，$root 必须是当前项目的 .codeartsdoer；复制源必须严格为 vendor/agentic-awesome-ui-lint/skills/ui-lint，目标必须严格为 .codeartsdoer/skills/ui-lint。
5. 运行 codearts debug skill；唯一生效的 ui-lint location 必须是项目目标 SKILL.md。
6. 将 <model> 换成已选 ID，逐字运行“验证”中的 codearts run 命令。
7. 只有退出码 0、恰好一个 name=ui-lint 且 status=completed 的 Skill 事件、没有其他工具事件，并识别全部六类问题、InlineCard.tsx:1 与逐项修复时才通过。
8. 报告 Commit、复制路径、事件和卸载清单。卸载仅可删除上述 target 与 source；禁止删除整个 .codeartsdoer、package.json、配置、凭据或其他 Skill。失败必须如实停止。
```

### 个人级提示词

```text
请为当前 Windows 用户安装并验证个人级 ui-lint，来源 https://github.com/sickn33/agentic-awesome-skills.git，固定 Commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3，源目录 skills/ui-lint。
1. 只允许创建 ~/.codeartsdoer/vendor/agentic-awesome-ui-lint 和 ~/.codeartsdoer/skills/ui-lint；不得修改用户根 package.json、codearts_cli.json、凭据、插件或项目配置。
2. 运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>。确认两个用户目标都不存在，并在一个全新临时 consumer 中确认没有项目级 .codeartsdoer/skills/ui-lint；任一冲突就停止。
3. 在 PowerShell 执行下方“Windows 手动安装”命令，$root 必须为 Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'；然后进入无同名项目 Skill 的全新 consumer。
4. 运行 codearts debug skill；唯一生效的 ui-lint location 必须是用户目标 SKILL.md。将 <model> 换成已选 ID，逐字运行“验证”中的 codearts run 命令。
5. 仅当退出码 0、恰好一个 completed ui-lint Skill 事件、无其他工具事件且结果满足全部断言时通过。卸载只能删除精确 target、source 与核对后的临时 consumer；禁止删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version`、`codearts models`。下例默认项目级；个人级只替换 `$root` 那一行。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级改为 Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-ui-lint'; $target=Join-Path $root 'skills\ui-lint'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/ui-lint/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
if((git -C $source rev-parse HEAD).Trim() -ne 'bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\ui-lint') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择可用模型，并按官方文档在用户配置中管理凭据；不要把凭据写入仓库、提示词或日志。本指南未要求修改 `package.json` 或 `codearts_cli.json`。

## 验证

先用 `codearts debug skill` 核对唯一来源，再把 `<model>` 替换为已选 ID，逐字执行：

```powershell
codearts run --format json --model "<model>" 'Call the skill tool exactly once with name ui-lint. Use no other tool, do not execute shell commands, do not access the network, and do not read or write files. Lint only this inline JSX: function Card(){return <button className={`ml-2 p-[24px] text-[#000000] w-4 h-4`}>Save</button>}. Return exact sections SCORE, ISSUES, FIXES, and TOTAL. Identify the hardcoded color, raw pixel spacing, physical margin, separate width/height, template-literal className, and missing data-slot. Use file reference InlineCard.tsx:1 and provide a concrete fix for every issue.'
```

成功信号：退出码为 0；只有一次来自目标绝对路径的 completed `ui-lint` Skill 事件；没有 Bash、Read、Write、Edit 或 Web 事件；最终文本含四个要求的章节、六类问题、行号和对应修复。

## 使用

```text
Use ui-lint. 审查这个 JSX 片段；按严重级别列出 design-token、RTL、尺寸和 data-slot 问题，并给出 file:line 与最小修复。
```

## 更新

先用 `git -C $source rev-parse HEAD` 记录当前固定版本。升级到新 Commit 前重新审查许可证、`skills/ui-lint` 的全部文件、脚本与依赖，并重做项目 A、全新项目 B、个人级和回滚验证。

## 卸载

确认解析后的绝对路径后，只删除 `$root/skills/ui-lint` 与 `$root/vendor/agentic-awesome-ui-lint`，再运行 `codearts debug skill` 确认旧路径消失。不得删除整个 `.codeartsdoer`、根配置或其他 Skill。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | release `v17.0.0`；Commit `bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3` |
| Skill / SHA-256 | `ui-lint` / `F08A89DB9C2925436FCB5EC155610A78F558CEA1FAD946A234DFF02B00A9A89E` |
| 许可证 | MIT |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-09-11 |

## 已知限制

只验证固定目录中的 `ui-lint`，不代表合集中的其他 Skill。它提供规则驱动的代码审查，不替代浏览器、视觉回归、可访问性树或真实交互测试；同名解析始终以 `codearts debug skill` 为准。

## 安全

固定目录只有一个 3,553 字节的 `SKILL.md`，没有依赖、生命周期脚本、二进制、下载器或遥测。上游正文虽展示 `grep`，本验证只使用完整内联 JSX，并禁止 Shell、文件与网络访问。

## 证据与来源

- [中文研究记录](../../research/2026-09-11.md) · [English](../../research/2026-09-11.en.md)
- [固定 Skill 目录](https://github.com/sickn33/agentic-awesome-skills/tree/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/skills/ui-lint)
- [MIT License](https://github.com/sickn33/agentic-awesome-skills/blob/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/LICENSE)
- [CodeArts CLI Skills 官方文档](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
