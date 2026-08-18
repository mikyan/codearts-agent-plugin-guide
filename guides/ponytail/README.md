# 在 CodeArts CLI 中使用 Ponytail

[English](README.en.md)

Ponytail 可以安装到当前项目，也可以安装到当前用户的所有项目，为 CodeArts CLI 增加 6 个代码精简和审查 Skills。

## 选择安装范围

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 团队共享、随仓库固定版本、只在当前项目使用。推荐默认选择。 |
| 个人级 | `~/.codeartsdoer` | 自己在多个项目中长期使用的通用工具。 |

CodeArts 官方说明：同名 Skill 同时存在时，**项目级优先于个人级**。不要在两个范围重复安装同一版本，除非你有意让项目覆盖个人配置。

## 让 Agent 帮你安装

在 CodeArts 中打开任意项目，根据需要复制其中一段。两段提示词都把文件路径、内容、命令和验证标准写全了。

### 项目级安装提示词

```text
请在当前项目中为 CodeArts CLI 安装项目级 Ponytail 4.9.0，并实际验证。

严格执行以下步骤：
1. 只修改当前项目的 .codeartsdoer 目录；不要修改 ~/.codeartsdoer、全局 npm 包或任何凭据。不得输出 API Key、CODEARTS_CLI_AK、CODEARTS_CLI_SK 的值。
2. 先运行 codearts --version、node --version、npm --version 和 codearts models。模型不明确时先询问我选择 provider/model ID。
3. 检查 .codeartsdoer/package.json 和 .codeartsdoer/skills 下是否已有 Ponytail。发现同名 Skill 或已有不同版本时停止并报告，不得覆盖。
4. 如果 .codeartsdoer/package.json 不存在，创建为：
   {
     "private": true,
     "type": "module",
     "dependencies": {
       "@dietrichgebert/ponytail": "4.9.0"
     }
   }
   如果文件已存在，保留所有原字段，只把 "@dietrichgebert/ponytail": "4.9.0" 合并到 dependencies，并确保 type 为 module。
5. 在项目根目录执行：
   npm install --prefix .codeartsdoer --ignore-scripts --no-audit --no-fund
6. 创建 .codeartsdoer/plugins/ponytail.js，完整内容为：
   import Ponytail from "@dietrichgebert/ponytail";

   export const PonytailPlugin = Ponytail;
7. 在项目根目录原样执行以下 PowerShell；它会把指定目录从依赖包复制到 .codeartsdoer/skills，并在任何同名目标存在时停止：
   $names = @("ponytail", "ponytail-audit", "ponytail-debt", "ponytail-gain", "ponytail-help", "ponytail-review")
   $source = (Resolve-Path ".codeartsdoer\node_modules\@dietrichgebert\ponytail\skills").Path
   $target = Join-Path (Get-Location) ".codeartsdoer\skills"
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   $conflicts = @($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) })
   if ($conflicts.Count -gt 0) { throw "Existing skill directories: $($conflicts -join ', ')" }
   foreach ($name in $names) { Copy-Item -LiteralPath (Join-Path $source $name) -Destination $target -Recurse }
8. 运行 codearts debug skill，确认能看到 ponytail、ponytail-audit、ponytail-debt、ponytail-gain、ponytail-help、ponytail-review。
9. 把 <选择的模型> 替换为第 2 步确认的模型 ID，然后原样执行：
   codearts run --format json --sandbox --model "<选择的模型>" "Call the skill tool exactly once with name ponytail-help. Do not use glob, read, or shell tools. After the skill tool returns, output the three level names in the same order as its Levels table."
   只有 JSON 中出现成功的 skill 工具事件，且最终结果为 Lite, Full, Ultra，才算通过。
10. 报告修改的准确文件、命令、工具事件、最终输出和卸载清单。任一步失败就停止并如实报告，不得宣称安装成功。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装个人级 Ponytail 4.9.0，使其可被 CodeArts CLI 的所有项目使用，并实际验证。

严格执行以下步骤：
1. 在 PowerShell 中执行 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"，以此作为唯一安装根目录。不要修改任何项目的 .codeartsdoer，不要修改 $userRoot/package.json，不要安装全局 npm 包，不得输出任何凭据值。
2. 先运行 codearts --version、node --version、npm --version 和 codearts models。模型不明确时先询问我选择 provider/model ID。
3. 检查以下目标是否已存在：$userRoot/vendor/ponytail、$userRoot/plugins/ponytail.ts，以及 $userRoot/skills 下 6 个 ponytail* 目录。任一存在就停止并报告，不得覆盖。
4. 创建 $userRoot/vendor/ponytail/package.json，完整内容为：
   {
     "private": true,
     "type": "module",
     "dependencies": {
       "@dietrichgebert/ponytail": "4.9.0"
     }
   }
5. 执行：
   npm install --prefix "$userRoot/vendor/ponytail" --ignore-scripts --no-audit --no-fund
6. 创建 $userRoot/plugins/ponytail.ts，完整内容为：
   import Ponytail from "../vendor/ponytail/node_modules/@dietrichgebert/ponytail/.opencode/plugins/ponytail.mjs";

   export const PonytailPlugin = Ponytail;
7. 在同一个 PowerShell 会话原样执行；它会把指定目录从个人依赖目录复制到 $userRoot/skills，并在任何同名目标存在时停止：
   $names = @("ponytail", "ponytail-audit", "ponytail-debt", "ponytail-gain", "ponytail-help", "ponytail-review")
   $source = (Resolve-Path (Join-Path $userRoot "vendor\ponytail\node_modules\@dietrichgebert\ponytail\skills")).Path
   $target = Join-Path $userRoot "skills"
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   $conflicts = @($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) })
   if ($conflicts.Count -gt 0) { throw "Existing skill directories: $($conflicts -join ', ')" }
   foreach ($name in $names) { Copy-Item -LiteralPath (Join-Path $source $name) -Destination $target -Recurse }
8. 选择一个没有项目级 Ponytail 的目录运行 codearts debug skill，确认能看到 6 个 Ponytail Skills，且 location 位于当前用户的 .codeartsdoer 下。
9. 在同一目录把 <选择的模型> 替换为第 2 步确认的模型 ID，然后原样执行：
   codearts run --format json --sandbox --model "<选择的模型>" "Call the skill tool exactly once with name ponytail-help. Do not use glob, read, or shell tools. After the skill tool returns, output the three level names in the same order as its Levels table."
   只有 JSON 中出现成功的 skill 工具事件、Skill 基础目录位于 ~/.codeartsdoer/skills/ponytail-help，且最终结果为 Lite, Full, Ultra，才算通过。
10. 报告修改的准确文件、命令、工具事件、最终输出和卸载清单。卸载范围只能包含 $userRoot/plugins/ponytail.ts、$userRoot/vendor/ponytail 和这 6 个个人级 Skill 目录。任一步失败就停止并如实报告。
```

下面是与提示词相同的手动流程。

## Windows 手动安装

### 1. 检查前置条件

- 按官方[安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)安装 CodeArts CLI。
- 安装 Node.js/npm。
- 在目标项目根目录执行后续步骤。

```powershell
codearts --version
node --version
npm --version
```

### 2. 项目级安装

#### 2.1 添加固定版本依赖

如果项目还没有 `.codeartsdoer/package.json`，创建以下文件：

```json
{
  "private": true,
  "type": "module",
  "dependencies": {
    "@dietrichgebert/ponytail": "4.9.0"
  }
}
```

如果文件已经存在，只合并该依赖，不要覆盖原内容。

禁止执行包生命周期脚本并安装依赖：

```powershell
npm install --prefix .codeartsdoer --ignore-scripts --no-audit --no-fund
```

#### 2.2 添加 CodeArts 插件入口

创建 `.codeartsdoer/plugins/ponytail.js`：

```js
import Ponytail from "@dietrichgebert/ponytail";

export const PonytailPlugin = Ponytail;
```

仓库中维护的副本见 [adapters/ponytail/ponytail.js](../../adapters/ponytail/ponytail.js)。必须保留 `.js` 扩展名；已验证的 CodeArts 版本不会发现 `.mjs` 入口。

#### 2.3 把 Skills 安装到 CodeArts 原生目录

以下脚本遇到项目中已有的同名 Skill 时会停止，不会直接覆盖：

```powershell
$source = (Resolve-Path ".codeartsdoer\node_modules\@dietrichgebert\ponytail\skills").Path
$target = Join-Path (Get-Location) ".codeartsdoer\skills"
New-Item -ItemType Directory -Path $target -Force | Out-Null

$skillDirectories = @(Get-ChildItem -LiteralPath $source -Directory)
$conflicts = @($skillDirectories | Where-Object {
  Test-Path -LiteralPath (Join-Path $target $_.Name)
})
if ($conflicts.Count -gt 0) {
  throw "Existing skill directories: $($conflicts.Name -join ', ')"
}

foreach ($skillDirectory in $skillDirectories) {
  Copy-Item -LiteralPath $skillDirectory.FullName -Destination $target -Recurse
}
```

预期项目结构：

```text
.codeartsdoer/
  package.json
  package-lock.json
  node_modules/@dietrichgebert/ponytail/
  plugins/ponytail.js
  skills/
    ponytail/
    ponytail-audit/
    ponytail-debt/
    ponytail-gain/
    ponytail-help/
    ponytail-review/
```

### 3. 个人级安装

个人级安装使用独立的 `vendor/ponytail` 依赖目录，**不要修改** CodeArts 用户根目录中已有的 `~/.codeartsdoer/package.json`。

#### 3.1 创建独立依赖清单

创建 `~/.codeartsdoer/vendor/ponytail/package.json`：

```json
{
  "private": true,
  "type": "module",
  "dependencies": {
    "@dietrichgebert/ponytail": "4.9.0"
  }
}
```

安装固定版本：

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$vendor = Join-Path $userRoot "vendor\ponytail"
npm install --prefix $vendor --ignore-scripts --no-audit --no-fund
```

#### 3.2 添加个人级插件入口

创建 `~/.codeartsdoer/plugins/ponytail.ts`：

```ts
import Ponytail from "../vendor/ponytail/node_modules/@dietrichgebert/ponytail/.opencode/plugins/ponytail.mjs";

export const PonytailPlugin = Ponytail;
```

仓库中维护的个人级副本见 [adapters/ponytail/ponytail.user.ts](../../adapters/ponytail/ponytail.user.ts)。

#### 3.3 安装个人级 Skills

```powershell
$source = (Resolve-Path (Join-Path $vendor "node_modules\@dietrichgebert\ponytail\skills")).Path
$target = Join-Path $userRoot "skills"
New-Item -ItemType Directory -Path $target -Force | Out-Null

$skillDirectories = @(Get-ChildItem -LiteralPath $source -Directory)
$conflicts = @($skillDirectories | Where-Object {
  Test-Path -LiteralPath (Join-Path $target $_.Name)
})
if ($conflicts.Count -gt 0) {
  throw "Existing skill directories: $($conflicts.Name -join ', ')"
}

foreach ($skillDirectory in $skillDirectories) {
  Copy-Item -LiteralPath $skillDirectory.FullName -Destination $target -Recurse
}
```

预期个人级结构：

```text
~/.codeartsdoer/
  plugins/ponytail.ts
  vendor/ponytail/
    package.json
    package-lock.json
    node_modules/@dietrichgebert/ponytail/
  skills/
    ponytail/
    ponytail-audit/
    ponytail-debt/
    ponytail-gain/
    ponytail-help/
    ponytail-review/
```

## 配置 CodeArts

CodeArts 模型配置属于用户级配置。不要把模型凭据写进项目依赖清单、个人级 `vendor` 目录、插件文件或 Git。

1. 参考官方[配置示例](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html)和 [AK/SK 配置](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html)。
2. 如果环境变量是在 CodeArts 启动后新增的，请重新打开 PowerShell。
3. 确认准备使用的模型以 `provider/model` 格式出现在列表中：

```powershell
codearts models
```

后续示例使用 `mimo/mimo-v2.5`，请按实际情况替换成自己的模型 ID。

## 验证

先检查发现结果：

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills |
  Where-Object { $_.name -like "ponytail*" } |
  Select-Object name, location
```

应当看到 6 个 Ponytail Skills。项目级安装的路径位于当前项目 `.codeartsdoer`；个人级安装的路径位于当前用户 `~/.codeartsdoer`。验证个人级安装时，请在没有项目级 Ponytail 的目录运行，避免项目级同名 Skill 覆盖结果。

然后进行真实模型调用；如有需要，请替换模型 ID：

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name ponytail-help. Do not use glob, read, or shell tools. After the skill tool returns, output the three level names in the same order as its Levels table."
```

通过时，JSON 事件中应包含 `"tool":"skill"`、`"status":"completed"`，最终文本为：

```text
Lite, Full, Ultra
```

如果模型只是给出了看似正确的答案，但没有成功的 `skill` 事件，不能视为验证通过。

## 使用

建议明确要求 CodeArts 调用指定 Skill，例如：

```text
Call the skill tool with name ponytail-review, then review the current diff for unnecessary complexity. Do not apply changes.
```

可用 Skills：`ponytail`、`ponytail-review`、`ponytail-audit`、`ponytail-debt`、`ponytail-gain` 和 `ponytail-help`。

## 更新

- 项目级：修改 `<项目>/.codeartsdoer/package.json` 中的固定版本，在项目依赖目录重新安装，然后替换项目级 6 个 Skill 目录。
- 个人级：修改 `~/.codeartsdoer/vendor/ponytail/package.json` 中的固定版本，在该 `vendor` 目录重新安装，然后替换个人级 6 个 Skill 目录。不要修改用户根 `~/.codeartsdoer/package.json`。

两种范围都必须先审查上游变更，再重新完成发现检查和真实模型调用。

## 卸载

保留其他 CodeArts 插件和 Skills，只删除所选范围内的内容。

项目级：

- `.codeartsdoer/plugins/ponytail.js`；
- `.codeartsdoer/skills` 下前文列出的 6 个 Ponytail 目录；
- 使用 `npm uninstall --prefix .codeartsdoer @dietrichgebert/ponytail --ignore-scripts` 移除依赖。

个人级：

- `~/.codeartsdoer/plugins/ponytail.ts`；
- `~/.codeartsdoer/vendor/ponytail`；
- `~/.codeartsdoer/skills` 下前文列出的 6 个 Ponytail 目录。

个人级卸载不要删除或重写 `~/.codeartsdoer/package.json`、`codearts_cli.json` 或其他插件。

如果使用过 Ponytail 模式切换，请检查用户目录下的 `.config/opencode/.ponytail-active`，确认后可删除。此次验证没有创建该文件。

最后在相应范围重新运行发现命令，确认不再存在目标 `ponytail*` Skills。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Adapter Required** |
| 上游项目 | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) |
| 上游版本 | 4.9.0（`0a4dd63ad4541f4f655c4108a295916f3c1d8fda`） |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目级、个人级 |
| 最后验证日期 | 2026-08-19 |

项目级安装已在两个隔离项目中复现。个人级安装随后从一个没有项目级 CodeArts 配置的独立目录验证。三个真实会话里，MiMo 都通过 CodeArts 原生 `skill` 工具成功调用了 `ponytail-help`，并返回 `Lite, Full, Ultra`。两种范围都完成了回滚，回滚后第三方 Skills 从发现结果中消失。

CodeArts CLI 26.8.1 需要适配器的原因：

- CodeArts 没有扫描上游 `.mjs` 插件入口，改为 `.js` 后加载成功。
- 上游插件通过 `config.skills.paths` 动态加入的 Skills 会出现在 `codearts debug skill`，但真实 `codearts run` 会话的 `skill` 工具无法调用。
- 把 Skills 复制到 `.codeartsdoer/skills` 后，运行时调用成功。
- 本机用户根 `~/.codeartsdoer/package.json` 含 CodeArts 内部依赖 `@opencode-ai/plugin@26.8.1`；系统 npm 和 CodeArts 自动 Reify 都无法从公共 Registry 解析该版本。因此个人级适配采用独立 `vendor/ponytail`，避免改动 CodeArts 自己的依赖清单。

尚未验证：CodeArts 桌面端/IDE、Linux、持久化 `/ponytail <level>` 模式切换，以及每个 Skill 的完整工作流。本次刻意没有调用模式切换，因为上游会把状态写入 `~/.config/opencode/.ponytail-active`。

## 安全说明

- 4.9.0 没有 npm install/postinstall 生命周期脚本，但后续版本仍需重新检查。
- 项目级安装只影响当前仓库；个人级安装影响当前用户的所有 CodeArts 项目。两者都不需要使用 `--auto`。
- 同名时项目级 Skill 优先。安装前检查两个范围，避免把旧版本隐藏在另一个范围中。
- Skill 指令会影响 Agent 行为；在敏感仓库中使用前，应阅读固定版本的 `SKILL.md`。
- 复制脚本遇到名称冲突会停止，避免覆盖其他来源的同名 Skill。

## 证据与来源

- [个人级与项目级实测记录](../../research/2026-08-19.md)
- [首轮项目级实测记录](../../research/2026-08-18.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI Hooks](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0018.html)
- [Ponytail 可移植性说明](https://github.com/DietrichGebert/ponytail/blob/main/docs/agent-portability.md)
- [Ponytail OpenCode 插件](https://github.com/DietrichGebert/ponytail/blob/main/.opencode/plugins/ponytail.mjs)
