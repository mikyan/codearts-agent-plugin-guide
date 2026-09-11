# 在 CodeArts CLI 中使用 effective-agent-skills

[English](README.en.md)

安装 `effective-agent-skills` Skill，按路由、结构、确定性、验证与安全性审查 Agent Skill 设计。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合仓库内统一审查标准；个人级 `~/.codeartsdoer` 适合跨项目复用。二选一并先运行 `codearts debug skill`；项目级同名 Skill 优先，目标或 vendor 路径有任何冲突都停止，不覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
请在当前项目根目录安装并验证项目级 effective-agent-skills。来源 https://github.com/sickn33/agentic-awesome-skills.git，固定 Commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3，源目录 skills/effective-agent-skills。
1. 只创建 <项目>/.codeartsdoer/vendor/agentic-awesome-effective-agent-skills 和 <项目>/.codeartsdoer/skills/effective-agent-skills；不得修改 ~/.codeartsdoer、package.json、codearts_cli.json、凭据或其他 Skill。
2. 运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；缺少凭据就停止，不读取或打印秘密。
3. 检查上述项目路径和 ~/.codeartsdoer/skills/effective-agent-skills；任一存在就停止，不得覆盖。
4. 从项目根执行“Windows 手动安装”命令，$root 必须为项目 .codeartsdoer；复制源严格为 vendor/agentic-awesome-effective-agent-skills/skills/effective-agent-skills，目标严格为 .codeartsdoer/skills/effective-agent-skills。
5. 运行 codearts debug skill；唯一生效的 effective-agent-skills location 必须是项目目标 SKILL.md。
6. 将 <model> 换成已选 ID，逐字运行“验证”中的 codearts run 命令。
7. 只有退出码 0、恰好一个 name=effective-agent-skills 且 status=completed 的 Skill 事件、无其他工具事件，并在六个指定章节指出名称不匹配、描述缺 what/when、流程/确定性/验证/安全缺失，且提出小写连字符名称与完整描述时通过。
8. 报告 Commit、路径、事件和卸载清单。卸载仅可删除精确 target 和 source；禁止删除整个 .codeartsdoer、package.json、配置、凭据或其他 Skill。失败必须如实停止。
```

### 个人级提示词

```text
请为当前 Windows 用户安装并验证个人级 effective-agent-skills。来源 https://github.com/sickn33/agentic-awesome-skills.git，固定 Commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3，源目录 skills/effective-agent-skills。
1. 只创建 ~/.codeartsdoer/vendor/agentic-awesome-effective-agent-skills 和 ~/.codeartsdoer/skills/effective-agent-skills；不得修改用户 package.json、codearts_cli.json、凭据、插件或项目配置。
2. 运行 codearts --version、git --version、codearts models，让我选择 <model>。确认两个用户路径和全新临时 consumer 中的 .codeartsdoer/skills/effective-agent-skills 都不存在；冲突时停止。
3. 执行“Windows 手动安装”命令，$root 必须为 Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'；进入没有同名项目 Skill 的 consumer。
4. codearts debug skill 的唯一 effective-agent-skills location 必须是用户目标。替换 <model> 后逐字执行“验证”命令，应用相同成功断言。
5. 卸载只删除精确 target、source 和核对后的临时 consumer；禁止删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version`、`codearts models`。下例默认项目级；个人级只替换 `$root`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级改为 Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-effective-agent-skills'; $target=Join-Path $root 'skills\effective-agent-skills'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/effective-agent-skills/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
if((git -C $source rev-parse HEAD).Trim() -ne 'bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\effective-agent-skills') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择可用模型；凭据只存于官方用户配置，不写进仓库、提示词或日志。安装与验证不要求修改 `package.json` 或 `codearts_cli.json`。

## 验证

用 `codearts debug skill` 核对唯一来源，替换 `<model>` 后逐字执行：

```powershell
codearts run --format json --model "<model>" 'Call the skill tool exactly once with name effective-agent-skills. Use no other tool, do not access the network, and do not read or write files. Review this inline draft skill: folder name report-helper; frontmatter name ReportHelper; description "Helps with reports"; body says "Do the workflow"; no trigger, steps, output format, validation loop, failure handling, or safety notes. Return exact sections ROUTING, STRUCTURE, DETERMINISM, VALIDATION, SECURITY, and VERDICT. Identify the invalid name mismatch and vague description, then propose a lowercase-hyphen name and a description containing what and when. Do not write a replacement file.'
```

成功信号：退出码 0；只有一次来自目标绝对路径的 completed `effective-agent-skills` 事件；无其他工具事件；最终文本包含六个章节、所有缺陷和要求的修正建议，且没有写入替换文件。

## 使用

```text
Use effective-agent-skills. 只读审查这个 Skill 的触发描述、步骤、输出契约、验证循环、失败处理和安全边界；先列证据，再给最小修改建议。
```

## 更新

用 `git -C $source rev-parse HEAD` 核对固定版本。更新 Commit 前重新审查许可证、`skills/effective-agent-skills` 的全部内容及依赖，并重做项目 A、全新项目 B、个人级与回滚验证。

## 卸载

解析绝对路径后，只删除 `$root/skills/effective-agent-skills` 与 `$root/vendor/agentic-awesome-effective-agent-skills`，再用 `codearts debug skill` 确认旧路径消失。不得删除整个 `.codeartsdoer`、根配置或其他 Skill。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | release `v17.0.0`；Commit `bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3` |
| Skill / SHA-256 | `effective-agent-skills` / `801183F8629EDFCEA977F3C0C8D4ECAEEF2B59626DB6527E785DC84938EA8C73` |
| 许可证 | MIT |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-09-11 |

## 已知限制

这里只验证只读审查一个内联草稿，不证明它能自动修复 Skill，也不代表合集中的其他 Skill。16 KB 的规则可能产生冗长回答；实际审查仍需结合目标 Agent 的 frontmatter 规范、工具接口与仓库约定。

## 安全

固定目录只有一个 16,172 字节的 `SKILL.md`，没有依赖、生命周期脚本、二进制、下载器或遥测。三次实测都只有目标 Skill 事件，未授权文件、Shell 和网络访问；建议先用只读提示词审查，再由人确认任何修改。

## 证据与来源

- [中文研究记录](../../research/2026-09-11.md) · [English](../../research/2026-09-11.en.md)
- [固定 Skill 目录](https://github.com/sickn33/agentic-awesome-skills/tree/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/skills/effective-agent-skills)
- [MIT License](https://github.com/sickn33/agentic-awesome-skills/blob/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/LICENSE)
- [CodeArts CLI Skills 官方文档](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
