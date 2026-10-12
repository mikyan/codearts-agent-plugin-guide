# 在 CodeArts CLI 中使用 what-could-break

[English](README.en.md)

安装 `what-could-break` Skill，让 Agent 在共享接口或多文件改动前寻找 diff 外的真实消费者，并用实际代码执行证明变更是否安全。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 只影响当前仓库；个人级 `~/.codeartsdoer` 可供所有项目使用。请二选一。CodeArts 官方规则是项目级同名 Skill 优先；若目标 Skill、vendor 或另一范围已有同名目录，立即停止，不要覆盖。

## 让 Agent 帮你安装

下面的提示词会创建一个完全列明内容的 `codearts-what-could-break-smoke` 目录。若该目录或任何目标路径已存在，必须停止；不要在真实同名文件上运行烟雾测试。

### 项目级提示词

```text
在当前 Windows 项目中安装并验证项目级 what-could-break Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v19.4.0，Commit 9f2c979c0841ad3f24cc42e38f9f7cb377034558，完整复制源 skills/what-could-break（SKILL.md 与 references/LICENSE-pstack.txt），目标目录 <项目>/.codeartsdoer/skills/what-could-break，独立 vendor 目录 <项目>/.codeartsdoer/vendor/agentic-awesome-what-could-break。先在项目根运行 codearts --version、git --version、node --version、codearts models，让我选择真实可用的 <model>；不得读取或打印凭据。令 $root=Join-Path (Get-Location) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-what-could-break'，$target=Join-Path $root 'skills\what-could-break'，$smoke=Join-Path (Get-Location) 'codearts-what-could-break-smoke'。检查 $source、$target、$smoke 和 ~/.codeartsdoer/skills/what-could-break，任一存在即停止；不得修改 package.json、codearts_cli.json、凭据、插件、权限文件、其他 Skill 或项目现有文件。
在项目根依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null；git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/what-could-break；确认 git -C $source rev-parse HEAD 精确等于 9f2c979c0841ad3f24cc42e38f9f7cb377034558；Copy-Item -LiteralPath (Join-Path $source 'skills\what-could-break') -Destination $target -Recurse；确认 $target/SKILL.md 与 $target/references/LICENSE-pstack.txt 都存在。
创建全新的 $smoke、$smoke/src 和 $smoke/clients；只创建以下 6 个文件，内容必须逐字等价：
1. $smoke/package.json：{"type":"module"}
2. $smoke/src/settings.mjs：export function serializeSettings(input) { 换行两个空格后 return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total }); 换行 }
3. $smoke/src/proposed-settings.mjs：export function serializeSettings(input) { 换行两个空格后 return JSON.stringify({ theme: input.theme }); 换行 }
4. $smoke/clients/mobile.mjs：export function renderLegacyTotal(payload) { 换行两个空格后 const parsed = JSON.parse(payload); 换行两个空格后 if (typeof parsed.legacy_total !== 'number') throw new Error('legacy_total missing'); 换行两个空格后 return `Total: ${parsed.legacy_total}`; 换行 }
5. $smoke/verify.mjs：import { serializeSettings } from './src/proposed-settings.mjs'; 换行 import { renderLegacyTotal } from './clients/mobile.mjs'; 换行 try { 换行两个空格后 renderLegacyTotal(serializeSettings({ theme: 'dark', legacy_total: 42 })); 换行两个空格后 console.error('unexpected compatibility'); 换行两个空格后 process.exit(1); 换行 } catch (error) { 换行两个空格后 console.log(`expected incompatibility: ${error.message}`); 换行 }
6. $smoke/proposed.diff：--- a/src/settings.mjs 换行 +++ b/src/settings.mjs 换行 @@ 换行 -  return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total }); 换行 +  return JSON.stringify({ theme: input.theme });
运行 codearts debug skill，唯一 name=what-could-break 的 location 必须是 $target/SKILL.md。将 <model> 替换成已选 provider/model ID，在项目根原样运行：codearts run -m "<model>" --format json --auto 'Call the skill tool exactly once with name what-could-break. Then inspect only the codearts-what-could-break-smoke directory using read-only Read, Glob, Grep, and Bash commands. Do not edit or create files, do not access the network, and do not inspect paths outside that directory. Review codearts-what-could-break-smoke/proposed.diff, find the concrete consumer that would break if legacy_total is removed, and run node codearts-what-could-break-smoke/verify.mjs exactly once as the real-code proof. Report the relevant file:line, the proof command and exit result, the evidence rung, and whether the proposed change is safe. Do not propose or apply a fix. End with the literal markers REAL_CODE_PROOF_RUN and CHANGE_UNSAFE.' 只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、只读搜索事件和恰好一个 completed Bash 事件，且答案定位 codearts-what-could-break-smoke/clients/mobile.mjs:3 或 :3-4、报告 node codearts-what-could-break-smoke/verify.mjs、输出 expected incompatibility: legacy_total missing、退出码 0、Rung 4、变更不安全和两个标记时通过。报告 Commit、绝对路径和事件。卸载前确认 $target 与 $source 都在 $root 内、$smoke 精确位于当前项目；只删除这三个精确目标，再运行 codearts debug skill 确认旧路径消失。禁止删除整个 .codeartsdoer、项目根、用户根、根配置或任何其他文件。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 what-could-break Skill。固定仓库 https://github.com/sickn33/agentic-awesome-skills.git，release v19.4.0，Commit 9f2c979c0841ad3f24cc42e38f9f7cb377034558，完整复制 skills/what-could-break（SKILL.md 与 references/LICENSE-pstack.txt）到 ~/.codeartsdoer/skills/what-could-break，独立 vendor 目录为 ~/.codeartsdoer/vendor/agentic-awesome-what-could-break。先运行 codearts --version、git --version、node --version、codearts models，让我选择真实可用的 <model>；不得读取或打印凭据。令 $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'，$source=Join-Path $root 'vendor\agentic-awesome-what-could-break'，$target=Join-Path $root 'skills\what-could-break'。在当前目录下创建一个全新的空 consumer `codearts-what-could-break-consumer`，令 $consumer=Join-Path (Get-Location) 'codearts-what-could-break-consumer'，$smoke=Join-Path $consumer 'codearts-what-could-break-smoke'。检查 $source、$target、$consumer 和 $smoke，任一存在即停止；不得修改用户 package.json、codearts_cli.json、凭据、插件、权限文件或其他项目配置。
依次执行：New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent),$consumer -Force | Out-Null；git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source；git -C $source sparse-checkout set skills/what-could-break；确认 git -C $source rev-parse HEAD 精确等于 9f2c979c0841ad3f24cc42e38f9f7cb377034558；Copy-Item -LiteralPath (Join-Path $source 'skills\what-could-break') -Destination $target -Recurse；确认 $target/SKILL.md 与 $target/references/LICENSE-pstack.txt 都存在。
创建全新的 $smoke、$smoke/src 和 $smoke/clients；只创建以下 6 个文件，内容必须逐字等价：
1. $smoke/package.json：{"type":"module"}
2. $smoke/src/settings.mjs：export function serializeSettings(input) { 换行两个空格后 return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total }); 换行 }
3. $smoke/src/proposed-settings.mjs：export function serializeSettings(input) { 换行两个空格后 return JSON.stringify({ theme: input.theme }); 换行 }
4. $smoke/clients/mobile.mjs：export function renderLegacyTotal(payload) { 换行两个空格后 const parsed = JSON.parse(payload); 换行两个空格后 if (typeof parsed.legacy_total !== 'number') throw new Error('legacy_total missing'); 换行两个空格后 return `Total: ${parsed.legacy_total}`; 换行 }
5. $smoke/verify.mjs：import { serializeSettings } from './src/proposed-settings.mjs'; 换行 import { renderLegacyTotal } from './clients/mobile.mjs'; 换行 try { 换行两个空格后 renderLegacyTotal(serializeSettings({ theme: 'dark', legacy_total: 42 })); 换行两个空格后 console.error('unexpected compatibility'); 换行两个空格后 process.exit(1); 换行 } catch (error) { 换行两个空格后 console.log(`expected incompatibility: ${error.message}`); 换行 }
6. $smoke/proposed.diff：--- a/src/settings.mjs 换行 +++ b/src/settings.mjs 换行 @@ 换行 -  return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total }); 换行 +  return JSON.stringify({ theme: input.theme });
确认 $consumer 没有项目级同名 Skill。进入 $consumer 运行 codearts debug skill，唯一 name=what-could-break 的 location 必须是 $target/SKILL.md。将 <model> 替换成已选 provider/model ID，在 $consumer 原样运行：codearts run -m "<model>" --format json --auto 'Call the skill tool exactly once with name what-could-break. Then inspect only the codearts-what-could-break-smoke directory using read-only Read, Glob, Grep, and Bash commands. Do not edit or create files, do not access the network, and do not inspect paths outside that directory. Review codearts-what-could-break-smoke/proposed.diff, find the concrete consumer that would break if legacy_total is removed, and run node codearts-what-could-break-smoke/verify.mjs exactly once as the real-code proof. Report the relevant file:line, the proof command and exit result, the evidence rung, and whether the proposed change is safe. Do not propose or apply a fix. End with the literal markers REAL_CODE_PROOF_RUN and CHANGE_UNSAFE.' 只有退出码 0、恰好一个来自 $target 的 completed Skill 事件、只读搜索事件和恰好一个 completed Bash 事件，且答案定位 codearts-what-could-break-smoke/clients/mobile.mjs:3 或 :3-4、报告固定 Node 命令与 expected incompatibility: legacy_total missing、退出码 0、Rung 4、不安全结论和两个标记时通过。报告 Commit、绝对路径和事件。卸载前核对绝对路径，只删除精确 $target、$source 与本次新建的 $consumer；再次从没有项目覆盖的目录运行 codearts debug skill 确认旧路径消失。不得删除用户根、根配置、凭据或其他 Skill。
```

## Windows 手动安装

先运行 `codearts --version`、`git --version`、`node --version` 和 `codearts models`。下例默认项目级；个人级只替换 `$root`。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-what-could-break'
$target=Join-Path $root 'skills\what-could-break'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/what-could-break
if((git -C $source rev-parse HEAD).Trim() -ne '9f2c979c0841ad3f24cc42e38f9f7cb377034558'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\what-could-break') -Destination $target -Recurse
if(-not (Test-Path (Join-Path $target 'references\LICENSE-pstack.txt'))){throw 'Incomplete copy'}
```

## CodeArts 配置

用 `codearts models` 选择真实可用的 `provider/model`。Skill 本身没有依赖，不需要修改 `package.json`、`codearts_cli.json` 或权限文件。代表调用需要只读检索与执行一个本地 Node 证明，因此只在已审查的隔离工作区使用 `--auto`。

## 验证

在项目或个人级 consumer 根创建 `codearts-what-could-break-smoke`，其中 6 个文件的完整内容如下：

```text
# package.json
{"type":"module"}

# src/settings.mjs
export function serializeSettings(input) {
  return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total });
}

# src/proposed-settings.mjs
export function serializeSettings(input) {
  return JSON.stringify({ theme: input.theme });
}

# clients/mobile.mjs
export function renderLegacyTotal(payload) {
  const parsed = JSON.parse(payload);
  if (typeof parsed.legacy_total !== 'number') throw new Error('legacy_total missing');
  return `Total: ${parsed.legacy_total}`;
}

# verify.mjs
import { serializeSettings } from './src/proposed-settings.mjs';
import { renderLegacyTotal } from './clients/mobile.mjs';
try {
  renderLegacyTotal(serializeSettings({ theme: 'dark', legacy_total: 42 }));
  console.error('unexpected compatibility');
  process.exit(1);
} catch (error) {
  console.log(`expected incompatibility: ${error.message}`);
}

# proposed.diff
--- a/src/settings.mjs
+++ b/src/settings.mjs
@@
-  return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total });
+  return JSON.stringify({ theme: input.theme });
```

先用 `codearts debug skill` 核对唯一绝对路径，再从项目或 consumer 根原样运行所选范围 Agent 提示词中的 `codearts run` 命令。成功必须同时有目标 Skill 完成事件、真实 `node codearts-what-could-break-smoke/verify.mjs` 事件、具体消费者位置、实际输出/退出码、Rung 4 与不安全结论；只有文本推测不通过。

## 使用

```text
Use what-could-break. 审查这个共享接口变更的 diff 外影响：先列出可能的真实消费者与边界，再选择一条最有辨识力的真实代码路径运行证明。区分观察、推断与未证明事项；报告命令、退出码和证据层级。不要修改代码，除非我随后明确要求。
```

## 更新

更新固定 Commit 前，重新审查 Skill、参考许可证与所有执行建议，并重新执行两个全新项目、个人级 consumer、真实 Node 证明和回滚验证。

## 卸载

只删除 `$root/skills/what-could-break` 与 `$root/vendor/agentic-awesome-what-could-break`。烟雾夹具仅在确认是本指南创建的精确 `codearts-what-could-break-smoke` 路径后删除。最后用 `codearts debug skill` 确认旧绝对路径消失。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| 上游 | Agentic Awesome `v19.4.0`；Commit `9f2c979c0841ad3f24cc42e38f9f7cb377034558` |
| 文件 / SHA-256 | `SKILL.md` / `320611171BB21423CB6157D9504F5E377DD92742CFDBF2A7BE66BAF995982EC2`；`references/LICENSE-pstack.txt` / `0B179DE8C206D95074651DC54810D1278CBE3C7EDEF85F6D3EB09C7BD2C2F02F` |
| 许可证 | Skill frontmatter：MIT，来源 `stas4000/what-could-break`；捆绑 pstack notice：MIT |
| 环境 | CodeArts CLI 26.8.1；Windows 11 Build 26200；Node 24.14.1；`mimo/mimo-v2.5` |
| 范围 | 两个全新项目 + 无项目覆盖的个人级 consumer；主验证、发布流、精确指南布局各三次；2026-10-12 |

## 已知限制

Skill 提供风险发现与证明方法，不保证穷举所有消费者。验证只覆盖一个 Node ESM 兼容性夹具，没有覆盖大型 monorepo、编译语言、远程服务、数据库或生产流量。Bash/Read/Glob/Grep 的权限和命令必须限制到已授权工作区；“搜索没找到”不能当作不存在风险的证明。

## 安全

固定目录只有 Markdown Skill 与 MIT notice，没有 manifest、锁文件、脚本、二进制、下载器、运行时网络、凭据或遥测。证明脚本完全在隔离目录运行，不访问网络或生产数据，也不修改项目。九次通过调用都加载正确 Skill 并实际运行固定证明；用户根配置与权限哈希在回滚后不变。一次从项目子目录启动的排除实验证明项目 Skill 不会向上加载，因此正式命令明确从项目根启动。

## 证据与来源

- [中文研究记录](../../research/2026-10-12.md) · [English](../../research/2026-10-12.en.md)
- [固定 Skill](https://github.com/sickn33/agentic-awesome-skills/tree/9f2c979c0841ad3f24cc42e38f9f7cb377034558/skills/what-could-break)
- [Agentic Awesome v19.4.0](https://github.com/sickn33/agentic-awesome-skills/releases/tag/v19.4.0)
- [原始仓库](https://github.com/stas4000/what-could-break)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
