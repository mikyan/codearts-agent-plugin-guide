# 在 CodeArts CLI 中使用 api-design-principles

[English](README.en.md)

安装 `api-design-principles`，让 Agent 用一致的资源、分页和错误模型设计 REST API。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合团队固定版本，个人级 `~/.codeartsdoer` 适合当前用户跨项目复用；只选一种。任一范围已有同名 Skill 就停止，并以 `codearts debug skill` 的实际来源为准。

## 让 Agent 帮你安装

### 项目级提示词

```text
在当前项目安装并验证 wshobson/agents 的项目级 api-design-principles，固定 Commit 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35。
1. 只创建 .codeartsdoer/vendor/wshobson-api-design-principles 和 .codeartsdoer/skills/api-design-principles；不得修改用户目录、package.json、codearts_cli.json、凭据或其他 Skill。
2. 运行 codearts --version、git --version、codearts models 并让我选可用模型；缺凭据就停止。检查项目两个目标和 ~/.codeartsdoer/skills/api-design-principles，冲突时停止。
3. 在项目根 PowerShell 执行：
   $s=Join-Path (Get-Location) '.codeartsdoer\vendor\wshobson-api-design-principles'; $t=Join-Path (Get-Location) '.codeartsdoer\skills\api-design-principles'
   New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/wshobson/agents.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/backend-development/skills/api-design-principles/' '/LICENSE'
   git -C $s checkout --detach 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
   if((git -C $s rev-parse HEAD).Trim() -ne '367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'plugins\backend-development\skills\api-design-principles') -Destination $t -Recurse
4. `codearts debug skill` 中唯一同名 location 必须指向 $t/SKILL.md。把 <模型> 替换后原样运行：codearts run --format json --sandbox --model "<模型>" "Verification contract: call the skill tool exactly once with name api-design-principles. No other tool is allowed. Using only the loaded skill and this prompt, write a concise REST contract for listing a user's orders. Include GET /api/users/{id}/orders, cursor pagination, one uniform error object, 404 when the user is absent, 422 for malformed query parameters, and one sentence on HTTP method semantics."
5. 仅当退出 0、恰好一个 completed Skill 事件、无其他工具且结果包含端点、cursor、404、422 时通过。报告 Commit、来源、事件、结果和卸载清单；卸载只删 $t 与 $s，禁止删整个 .codeartsdoer 或其他配置。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 api-design-principles，固定 wshobson/agents Commit 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35。
1. 只创建 ~/.codeartsdoer/vendor/wshobson-api-design-principles 和 ~/.codeartsdoer/skills/api-design-principles；不改用户根 package.json、codearts_cli.json、凭据、插件或项目。
2. 运行版本与 models 检查并让我选模型。检查两个用户目标和干净验证目录中的项目同名 Skill，冲突或缺凭据就停止。
3. 在 PowerShell 执行：
   $u=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'; $s=Join-Path $u 'vendor\wshobson-api-design-principles'; $t=Join-Path $u 'skills\api-design-principles'; $c=Join-Path ([IO.Path]::GetTempPath()) 'codearts-api-design-367cb6a-smoke'
   if((Test-Path $s)-or(Test-Path $t)-or(Test-Path $c)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent),$c -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/wshobson/agents.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/backend-development/skills/api-design-principles/' '/LICENSE'
   git -C $s checkout --detach 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
   if((git -C $s rev-parse HEAD).Trim() -ne '367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'plugins\backend-development\skills\api-design-principles') -Destination $t -Recurse; Set-Location $c
4. 唯一发现来源必须是 $t/SKILL.md；把 <模型> 换成已选 ID 并运行：codearts run --format json --sandbox --model "<模型>" "Verification contract: call the skill tool exactly once with name api-design-principles. No other tool is allowed. Using only the loaded skill and this prompt, write a concise REST contract for listing a user's orders. Include GET /api/users/{id}/orders, cursor pagination, one uniform error object, 404 when the user is absent, 422 for malformed query parameters, and one sentence on HTTP method semantics." 仅当退出 0、一个 completed Skill 事件、无其他工具且结果含端点、cursor、404、422 时通过。卸载仅删 $t、$s 和核对后的 $c；禁止删除用户根、根配置、秘密或其他 Skill。
```

## Windows 手动安装

运行 `codearts --version`、`git --version`、`codearts models`，选择项目或个人 `$root`，然后执行相应提示词中完全相同的固定 Commit、稀疏检出和目录复制命令。必须复制整个 `api-design-principles` 目录（6 个文件），而非只复制 `SKILL.md`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级改为：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$s=Join-Path $root 'vendor\wshobson-api-design-principles'; $t=Join-Path $root 'skills\api-design-principles'
if((Test-Path $s)-or(Test-Path $t)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
git clone --filter=blob:none --no-checkout https://github.com/wshobson/agents.git $s
git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/backend-development/skills/api-design-principles/' '/LICENSE'
git -C $s checkout --detach 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
Copy-Item -LiteralPath (Join-Path $s 'plugins\backend-development\skills\api-design-principles') -Destination $t -Recurse
```

## CodeArts 配置

按官方文档配置所选模型凭据，不写入仓库。

## 验证

先核对 `codearts debug skill`，再执行提示词中的真实验证命令；四个合同断言与 completed Skill 事件缺一不可。

## 使用

```text
Use api-design-principles. 请审查这个订单 API 的资源命名、分页、幂等性和错误结构：<合同>
```

## 更新

用 `git -C $s rev-parse HEAD` 检查版本。升级必须重新审查和三环境验证。

## 卸载

只删除所选范围的 `skills/api-design-principles` 与 `vendor/wshobson-api-design-principles`，并确认不再被发现；禁止删整个根目录。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| Commit | `367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35` |
| Skill / SHA-256 | `api-design-principles` / `7758937E5BD1F8F5AC7D0C89BE2C776D509855D9CF47DFAE75A5CB45E964AB75` |
| 许可证 | MIT |
| 环境与范围 | CodeArts CLI 26.8.1；Windows 11 Build 26200；MiMo；两个项目 + 个人级；2026-08-20 |

## 已知限制

只验证 REST 合同核心调用，未验证 GraphQL、模板脚本执行或其他仓库插件。

## 安全

固定目录 6 个文件、41,045 字节；其中 Python 文件是模板，安装与 smoke test 均未执行。没有 npm、生命周期脚本、网络、凭据或遥测。

## 证据与来源

- [中文研究](../../research/2026-08-20.md) · [English](../../research/2026-08-20.en.md)
- [固定 Commit](https://github.com/wshobson/agents/tree/367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35/plugins/backend-development/skills/api-design-principles)
- [MIT License](https://github.com/wshobson/agents/blob/367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35/LICENSE)
- [CodeArts Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
