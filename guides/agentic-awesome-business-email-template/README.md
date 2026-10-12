# 在 CodeArts CLI 中使用 business-email-template

[English](README.en.md)

安装 `business-email-template` Skill，让 Agent 先用可审计的字段表管理业务邮件模板、触发条件、个性化占位符、认证与发送检查，而不是直接处理客户名单或发送邮件。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 只影响当前仓库；个人级 `~/.codeartsdoer` 可供所有项目使用。请二选一。CodeArts 官方规则是项目级同名 Skill 优先；若目标 Skill、vendor 或另一范围已有同名目录，立即停止，不要覆盖。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前 Windows 项目中安装并验证项目级 business-email-template Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v19.4.0，Commit 9f2c979c0841ad3f24cc42e38f9f7cb377034558，复制源 skills/business-email-template（完整复制 SKILL.md 与 references/common-pitfalls.md），目标目录 <项目>/.codeartsdoer/skills/business-email-template，独立 vendor 目录 <项目>/.codeartsdoer/vendor/agentic-awesome-business-email-template。先在项目根运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-business-email-template'，$target=Join-Path $root 'skills\business-email-template'。检查 $source、$target 和 ~/.codeartsdoer/skills/business-email-template，任一存在即停止；不得修改 package.json、codearts_cli.json、凭据、插件、权限文件或其他 Skill。
在项目根依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/business-email-template；确认 git -C $source rev-parse HEAD 精确等于 9f2c979c0841ad3f24cc42e38f9f7cb377034558；Copy-Item -LiteralPath (Join-Path $source 'skills\business-email-template') -Destination $target -Recurse；确认 $target/SKILL.md 与 $target/references/common-pitfalls.md 都存在。
运行 codearts debug skill，唯一 name=business-email-template 的 location 必须是 $target/SKILL.md。将 <model> 替换成已选 provider/model ID 后原样运行：codearts run -m "<model>" --format json 'Call the skill tool exactly once with name business-email-template and use no other tool. Build a data-only empty CSV template register for transactional enquiry replies. Return exactly one CSV header line using the skill''s complete field list, followed by one short sentence that no customer data, DNS values, or email bodies were used. Do not create a file, send email, or invent a domain. End with the literal markers EMAIL_SCHEMA_22_COLUMNS and DATA_ONLY.' 只有退出码 0、恰好一个来自 $target 的 name=business-email-template/status=completed 事件、没有其他工具事件，且唯一 CSV 表头按顺序含以下 22 列时通过：Template ID, Template Name, Template Type, Trigger, Sender Role, Recipient Type, Subject Line Pattern, Preview Text, Body Structure, Tone, Call To Action, Required Fields, Personalisation Tokens, Signature Block, Unsubscribe Required, Spam Risk Notes, Test Status, Mobile Checked, Accessibility Checked, Version, Status, Notes。答案还必须声明没有使用客户数据、DNS 值或邮件正文，并含两个标记。报告 Commit、绝对路径和事件。卸载只删除精确 $target 与 $source，再运行 codearts debug skill 确认旧路径消失；禁止删除整个 .codeartsdoer、项目根、用户根或任何根配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 business-email-template Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v19.4.0，Commit 9f2c979c0841ad3f24cc42e38f9f7cb377034558，完整复制 skills/business-email-template 到 ~/.codeartsdoer/skills/business-email-template，独立 vendor 目录为 ~/.codeartsdoer/vendor/agentic-awesome-business-email-template。先运行 codearts --version、git --version、codearts models，让我选择真实可用的 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-business-email-template'，$target=Join-Path $root 'skills\business-email-template'。检查 $source、$target 以及用于验证的全新 consumer 中 .codeartsdoer/skills/business-email-template，任一存在即停止；不得修改用户 package.json、codearts_cli.json、凭据、插件、权限文件或项目配置。
依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/business-email-template；确认 HEAD 精确匹配 9f2c979c0841ad3f24cc42e38f9f7cb377034558；Copy-Item -LiteralPath (Join-Path $source 'skills\business-email-template') -Destination $target -Recurse；确认两个固定 Markdown 文件都存在。进入一个没有项目级同名 Skill 的全新 consumer，运行 codearts debug skill，唯一 name=business-email-template 的 location 必须是 $target/SKILL.md。替换 <model> 后原样运行：codearts run -m "<model>" --format json 'Call the skill tool exactly once with name business-email-template and use no other tool. Build a data-only empty CSV template register for transactional enquiry replies. Return exactly one CSV header line using the skill''s complete field list, followed by one short sentence that no customer data, DNS values, or email bodies were used. Do not create a file, send email, or invent a domain. End with the literal markers EMAIL_SCHEMA_22_COLUMNS and DATA_ONLY.' 成功判据与项目级相同。报告 Commit、绝对路径和事件。卸载只删除精确 $target、$source 和核对后的 consumer，再运行 codearts debug skill 确认旧路径消失；不得删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version` 和 `codearts models`。下例默认项目级；个人级只替换 `$root`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-business-email-template'
$target=Join-Path $root 'skills\business-email-template'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/business-email-template
if((git -C $source rev-parse HEAD).Trim() -ne '9f2c979c0841ad3f24cc42e38f9f7cb377034558'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\business-email-template') -Destination $target -Recurse
if(-not (Test-Path (Join-Path $target 'references\common-pitfalls.md'))){throw 'Incomplete copy'}
```

## CodeArts 配置

用 `codearts models` 选择真实可用的 `provider/model`。这个内容型 Skill 没有运行时依赖，不需要修改 `package.json`、`codearts_cli.json` 或权限文件。它不会连接邮箱、Notion、DNS 或发送服务。

## 验证

先用 `codearts debug skill` 核对唯一绝对路径，再运行上面提示词中的完整 `codearts run` 命令。成功判据是一个且仅一个目标 Skill 事件、无其他工具事件、精确 22 列表头以及明确的数据边界声明。

## 使用

```text
Use business-email-template. 我们需要 enquiry reply、quote 和 invoice 三类可复用邮件。先说明最小模板集、触发条件、发送角色、收件人类型、主题模式、正文结构、个性化占位符、签名、退订与发送前检查；不要索取客户名单、真实邮件正文、DNS 值或凭据。只有我明确要求后才生成数据模板，绝不发送邮件。
```

## 更新

更新固定 Commit 前，重新审查 `SKILL.md`、参考文件与许可证，并重新执行两个全新项目、个人级 consumer、22 列输出和回滚验证。

## 卸载

只删除 `$root/skills/business-email-template` 与 `$root/vendor/agentic-awesome-business-email-template`，再用 `codearts debug skill` 确认旧绝对路径不再出现。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | Agentic Awesome `v19.4.0`；Commit `9f2c979c0841ad3f24cc42e38f9f7cb377034558` |
| 文件 / SHA-256 | `SKILL.md` / `5EE81C9BFC7FA76957274C4379B64AC0DF16D306B03B20CFE9C494C108ABBC24`；`references/common-pitfalls.md` / `F30CB313C7EEEDD2002D4E1F39EAA65E3F78CA22F3E07312442DD1BB3F8AA333` |
| 许可证 | AAS 原创非代码内容 CC BY 4.0；仓库代码与工具 MIT；Skill frontmatter 为 `source: self` |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 无项目覆盖的个人级 consumer；主验证和发布流各三次；2026-10-12 |

## 已知限制

Skill 生成的是模板登记结构与建议，不会配置 SPF/DKIM/DMARC、测量垃圾邮件分数、渲染邮件、验证法律文本或发送消息。不同地区的营销同意、退订、地址和监管措辞必须由业务与法律负责人确认。正式数据中不要放客户名单或个人信息。

## 安全

固定目录只有两个 Markdown 文件，没有 manifest、锁文件、脚本、二进制、下载器、运行时网络、凭据或遥测。六次最终调用都只有目标 Skill 事件，没有访问文件、邮箱、DNS、Notion 或网络；用户根配置与权限哈希在回滚后保持不变。

## 证据与来源

- [中文研究记录](../../research/2026-10-12.md) · [English](../../research/2026-10-12.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/9f2c979c0841ad3f24cc42e38f9f7cb377034558/skills/business-email-template)
- [Agentic Awesome v19.4.0](https://github.com/sickn33/agentic-awesome-skills/releases/tag/v19.4.0)
- [AAS 内容许可证](https://github.com/sickn33/agentic-awesome-skills/blob/9f2c979c0841ad3f24cc42e38f9f7cb377034558/LICENSE-CONTENT)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
