# 在 CodeArts CLI 中使用 read-all-adrs

[English](README.en.md)

安装 `read-all-adrs` Skill，在开始设计或实现前完整读取项目的架构决策记录（ADR）。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合随仓库固定版本，个人级 `~/.codeartsdoer` 适合跨项目复用。二选一；先用 `codearts debug skill` 检查同名项，项目级同名 Skill 优先。任何目标或 vendor 路径已存在时停止，不覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
请在当前项目根目录安装并验证项目级 read-all-adrs。来源 https://github.com/sickn33/agentic-awesome-skills.git，固定 Commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3，源目录 skills/read-all-adrs。
1. 只创建 <项目>/.codeartsdoer/vendor/agentic-awesome-read-all-adrs 和 <项目>/.codeartsdoer/skills/read-all-adrs；不得修改 ~/.codeartsdoer、package.json、codearts_cli.json、ADR、凭据或其他 Skill。
2. 运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；缺少凭据就停止，禁止读取或打印秘密。
3. 检查上述项目路径与 ~/.codeartsdoer/skills/read-all-adrs；任一冲突就停止。确认 docs/adr 下至少有一个 Markdown 文件；没有就停止，不得声称验证成功。
4. 从项目根运行“Windows 手动安装”命令，$root 必须为当前项目 .codeartsdoer；复制源严格为 vendor/agentic-awesome-read-all-adrs/skills/read-all-adrs，目标严格为 .codeartsdoer/skills/read-all-adrs。
5. 运行 codearts debug skill；唯一生效的 read-all-adrs location 必须是项目目标 SKILL.md。
6. 将 <model> 换成已选 ID，逐字运行“验证”中的 codearts run 命令。
7. 只有退出码 0、一个 completed read-all-adrs Skill 事件、每个 docs/adr/*.md 都有完成的读取、没有写入/Shell/网络事件，且最终文本列出全部文件并忠实保留决策、后果和待决问题时通过。
8. 报告 Commit、路径、读取事件与卸载清单。卸载仅可删除精确 target 和 source；禁止删除整个 .codeartsdoer、docs/adr、根配置、凭据或其他 Skill。失败必须如实停止。
```

### 个人级提示词

```text
请为当前 Windows 用户安装并验证个人级 read-all-adrs。来源 https://github.com/sickn33/agentic-awesome-skills.git，固定 Commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3，源目录 skills/read-all-adrs。
1. 只创建 ~/.codeartsdoer/vendor/agentic-awesome-read-all-adrs 和 ~/.codeartsdoer/skills/read-all-adrs；不得修改用户 package.json、codearts_cli.json、凭据、插件、项目配置或 ADR。
2. 运行 codearts --version、git --version、codearts models，让我选择 <model>。检查两个用户路径，并选择一个无项目级同名 Skill、但 docs/adr 下已有 Markdown ADR 的 consumer；冲突或没有 ADR 就停止。
3. 执行“Windows 手动安装”命令，$root 必须为 Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，再进入该 consumer。
4. codearts debug skill 的唯一 read-all-adrs location 必须是用户目标。替换 <model> 后逐字运行“验证”命令；成功条件与项目级完全相同。
5. 卸载只可删除精确 target 与 source；不得删除 consumer、ADR、用户根、配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version`、`codearts models`。下例默认项目级；个人级只替换 `$root`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级改为 Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-read-all-adrs'; $target=Join-Path $root 'skills\read-all-adrs'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/read-all-adrs/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
if((git -C $source rev-parse HEAD).Trim() -ne 'bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\read-all-adrs') -Destination $target -Recurse
```

## CodeArts 配置

用 `codearts models` 选择可用模型，凭据只放在官方用户配置中，不写入仓库、提示词或日志。验证任务需要读取工作区 ADR，但不需要修改 `package.json` 或 `codearts_cli.json`。

## 验证

先确认 `docs/adr/*.md` 非空，并用 `codearts debug skill` 核对唯一来源。替换 `<model>` 后逐字执行：

```powershell
codearts run --format json --model "<model>" 'Call the skill tool exactly once with name read-all-adrs. Then use only read tools to read every Markdown file under docs/adr completely. Do not access the network or write files. Return sections ADRS READ, CURRENT DECISIONS, CONSEQUENCES, OPEN QUESTIONS; list every filename.'
```

成功信号：退出码 0；目标绝对路径的 `read-all-adrs` Skill 完成；每个 ADR 都有完成的 Read；无 Write、Edit、Bash 或 Web；最终列全文件，且不补造决策。目录为空时这是未验证，不是通过。

## 使用

```text
Use read-all-adrs. 在提出缓存层设计前，完整读取 docs/adr 下所有 ADR，列出现行约束、冲突和仍待决定的问题。
```

## 更新

用 `git -C $source rev-parse HEAD` 核对固定版本。升级前重新审查许可证和 `skills/read-all-adrs` 全部内容，并用包含多个 ADR 的项目 A、全新项目 B、个人级重做完整读取与回滚验证。

## 卸载

解析并确认绝对路径后，只删除 `$root/skills/read-all-adrs` 与 `$root/vendor/agentic-awesome-read-all-adrs`；不删除 ADR 或 consumer。再运行 `codearts debug skill` 确认旧路径消失。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | release `v17.0.0`；Commit `bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3` |
| Skill / SHA-256 | `read-all-adrs` / `8413F2FA1D9504FBBD179E356ED1FA2210CF29F98962BC5BA4CEEE4C3CA0AABB` |
| 许可证 | MIT |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 个人级；2026-09-11 |

## 已知限制

实测使用两个合成 ADR，验证了 SQLite、可选同步、服务端只存密文与密钥恢复待定等事实的完整保留。frontmatter 的 `disable-model-invocation: true` 在 CodeArts 中的自动调用语义未单独证明；本指南只验证显式调用。它不能判断 ADR 是否过时或彼此正确。

## 安全

固定目录只有一个 1,331 字节的 `SKILL.md`，无依赖、脚本、二进制、下载器或遥测。验证只授权读取 `docs/adr`；个人级实测虽重复读取，仍无写入、Shell 或网络事件。ADR 可能含敏感架构信息，应使用符合项目数据边界的模型。

## 证据与来源

- [中文研究记录](../../research/2026-09-11.md) · [English](../../research/2026-09-11.en.md)
- [固定 Skill 目录](https://github.com/sickn33/agentic-awesome-skills/tree/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/skills/read-all-adrs)
- [MIT License](https://github.com/sickn33/agentic-awesome-skills/blob/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/LICENSE)
- [CodeArts CLI Skills 官方文档](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
