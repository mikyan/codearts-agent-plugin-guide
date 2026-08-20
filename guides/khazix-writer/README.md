# 在 CodeArts CLI 中使用 khazix-writer

[English](README.en.md)

安装 `khazix-writer`，按其 L1–L4 流程把素材改写成面向中文读者的文章。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合固定团队版本；个人级 `~/.codeartsdoer` 适合当前用户复用。只选一种，项目或个人任一同名目录存在时停止。

## 让 Agent 帮你安装

### 项目级提示词

```text
安装并验证项目级 KKKKhazix/khazix-skills 的 khazix-writer，固定 Commit 7a5c4934be4106ac740ffdb95280bb81b3f4b83c。
1. 只创建 .codeartsdoer/vendor/khazix-writer 和 .codeartsdoer/skills/khazix-writer；不改用户目录、package.json、codearts_cli.json、凭据或其他 Skill。运行 codearts --version、git --version、codearts models 并让我选可用模型；缺凭据或项目/个人同名冲突时停止。
2. 在项目根 PowerShell 执行：
   $s=Join-Path (Get-Location) '.codeartsdoer\vendor\khazix-writer'; $t=Join-Path (Get-Location) '.codeartsdoer\skills\khazix-writer'
   New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/KKKKhazix/khazix-skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/khazix-writer/' '/LICENSE'
   git -C $s checkout --detach 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
   if((git -C $s rev-parse HEAD).Trim() -ne '7a5c4934be4106ac740ffdb95280bb81b3f4b83c'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'khazix-writer') -Destination $t -Recurse
3. debug skill 的唯一同名来源必须是 $t/SKILL.md。把 <模型> 替换后原样运行：codearts run --format json --sandbox --model "<模型>" "Verification contract: call the skill tool exactly once with name khazix-writer. The skill tool is the only allowed tool. Using only the prompt and loaded skill, write a short Chinese public-account excerpt about this fact: 今天我用十五分钟把一个 AI 写作 Skill 复制到 CodeArts 的原生 Skills 目录，并看到 completed skill event. Include one direct reader address using 你, one concrete scene, and a compact quality-check line explicitly naming L1, L2, L3, L4. Do not read or write files, browse, or execute commands."
4. 仅当退出 0、恰好一个 completed skill 事件、无其他工具、中文结果含 CodeArts、你、具体场景和 L1–L4 质检时通过。报告证据；卸载只删 $t 与 $s，禁止删整个根或其他配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装个人级 khazix-writer，固定 Commit 7a5c4934be4106ac740ffdb95280bb81b3f4b83c。
1. 只创建 ~/.codeartsdoer/vendor/khazix-writer 和 ~/.codeartsdoer/skills/khazix-writer；不改根 package.json、codearts_cli.json、凭据、插件或项目。运行版本/models 检查并让我选模型；缺凭据或同名冲突时停止。
2. PowerShell 执行：
   $u=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'; $s=Join-Path $u 'vendor\khazix-writer'; $t=Join-Path $u 'skills\khazix-writer'; $c=Join-Path ([IO.Path]::GetTempPath()) 'codearts-khazix-writer-7a5c493-smoke'
   if((Test-Path $s)-or(Test-Path $t)-or(Test-Path $c)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent),$c -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/KKKKhazix/khazix-skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/khazix-writer/' '/LICENSE'
   git -C $s checkout --detach 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
   if((git -C $s rev-parse HEAD).Trim() -ne '7a5c4934be4106ac740ffdb95280bb81b3f4b83c'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'khazix-writer') -Destination $t -Recurse; Set-Location $c
3. debug skill 必须解析到 $t/SKILL.md；把 <模型> 换成已选 ID 并运行：codearts run --format json --sandbox --model "<模型>" "Verification contract: call the skill tool exactly once with name khazix-writer. The skill tool is the only allowed tool. Using only the prompt and loaded skill, write a short Chinese public-account excerpt about this fact: 今天我用十五分钟把一个 AI 写作 Skill 复制到 CodeArts 的原生 Skills 目录，并看到 completed skill event. Include one direct reader address using 你, one concrete scene, and a compact quality-check line explicitly naming L1, L2, L3, L4. Do not read or write files, browse, or execute commands." 仅当退出 0、一个 completed Skill、无其他工具且中文结果含 CodeArts、你、具体场景和 L1–L4 时通过。卸载仅删 $t、$s 和核对后的 $c，禁止删除用户根或其他配置。
```

## Windows 手动安装

运行版本与 models 检查，选择一种范围，执行匹配提示词的固定检出和整个 `khazix-writer` 目录复制。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级改为：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$s=Join-Path $root 'vendor\khazix-writer'; $t=Join-Path $root 'skills\khazix-writer'
if((Test-Path $s)-or(Test-Path $t)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
git clone --filter=blob:none --no-checkout https://github.com/KKKKhazix/khazix-skills.git $s
git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/khazix-writer/' '/LICENSE'
git -C $s checkout --detach 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
Copy-Item -LiteralPath (Join-Path $s 'khazix-writer') -Destination $t -Recurse
```

## CodeArts 配置

凭据只按官方方式配置，不要写入仓库或输出。

## 验证

先检查唯一来源，再运行真实 smoke test；不得把模型直接输出当作工具事件证据。

## 使用

```text
Use khazix-writer. 请把以下素材改写成 800 字中文文章；先确认读者和观点，不要捏造数据：<素材>
```

## 更新

`git -C $s rev-parse HEAD` 检查固定版本；升级需重新审查和三环境验证。

## 卸载

只删所选范围的 `skills/khazix-writer` 与 `vendor/khazix-writer`，再确认不被发现。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| Commit | `7a5c4934be4106ac740ffdb95280bb81b3f4b83c` |
| Skill / SHA-256 | `khazix-writer` / `8683276199A350DDB5195A9A2A00704A4EF10C0CC8DE9B9FAB43D328ADE41D7A` |
| 许可证 | MIT |
| 环境与范围 | CodeArts 26.8.1；Windows 11 Build 26200；MiMo；两个项目 + 个人级；2026-08-20 |

## 已知限制

只验证短篇中文改写与 L1–L4 自检；长文、事实检索、发布和风格授权未验证。

## 安全

固定目录 3 个文本文件、54,112 字节，不执行 npm、脚本、网络、凭据或遥测。Skill 含作者签名和号召用语，正式使用前应明确要求是否保留，并对事实与版权人工复核。

## 证据与来源

- [中文研究](../../research/2026-08-20.md) · [English](../../research/2026-08-20.en.md)
- [固定 Commit](https://github.com/KKKKhazix/khazix-skills/tree/7a5c4934be4106ac740ffdb95280bb81b3f4b83c/khazix-writer)
- [MIT License](https://github.com/KKKKhazix/khazix-skills/blob/7a5c4934be4106ac740ffdb95280bb81b3f4b83c/LICENSE)
- [CodeArts Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
