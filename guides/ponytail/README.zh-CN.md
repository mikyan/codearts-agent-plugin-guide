# 在 CodeArts CLI 中使用 Ponytail

[English](README.md)

## 验证结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Adapter Required** |
| 上游项目 | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) |
| 上游版本 | 4.9.0（`0a4dd63ad4541f4f655c4108a295916f3c1d8fda`） |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 最后验证日期 | 2026-08-18 |

安装轻量的项目级适配器后，Ponytail 的 6 个 Skills 可以通过 CodeArts 原生 `skill` 工具使用。该结果已在两个隔离项目中重复验证，并完成回滚测试。

CodeArts CLI 26.8.1 不能直接照搬上游 OpenCode 安装方法：

- CodeArts 没有扫描上游的 `.mjs` 插件入口，适配器必须使用 `.js`。
- 插件通过 `config.skills.paths` 动态加入的 Skills 会出现在 `codearts debug skill` 中，但真实 `codearts run` 会话的 `skill` 工具仍然找不到它们。
- 因此还必须把 Skills 复制到 CodeArts 原生项目目录 `.codeartsdoer/skills`。

## 验证范围与限制

已验证：

- 从固定版本的 npm 包进行项目级安装；
- 加载 `.js` 插件包装入口；
- 发现全部 6 个 Skills；
- MiMo 在真实会话中通过 CodeArts `skill` 工具调用 `ponytail-help`；
- 第二个干净项目复现，以及项目级回滚。

未验证：

- CodeArts 桌面端/IDE 客户端和 Linux；
- Ponytail 持久化的 `/ponytail <level>` 模式切换；
- 每个 Skill 的完整工作流。

本次没有调用模式切换命令，因为上游会把状态写入 `~/.config/opencode/.ponytail-active`。当前建议通过 CodeArts `skill` 工具明确调用所需 Skill。

## 前置条件

1. 安装并配置 CodeArts CLI。参考官方的[安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)、[配置示例](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html)和 [AK/SK 配置](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html)。
2. 确认 `codearts --version` 和 `codearts models` 可正常运行。
3. 安装 Node.js/npm。
4. 在目标项目根目录执行以下步骤。

不要把模型 API Key 或真实华为云凭据写入项目或 Git 历史。

## Windows PowerShell 安装步骤

### 1. 添加固定版本依赖

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

### 2. 添加 CodeArts 插件入口

创建 `.codeartsdoer/plugins/ponytail.js`：

```js
import Ponytail from "@dietrichgebert/ponytail";

export const PonytailPlugin = Ponytail;
```

仓库中维护的副本见 [adapters/ponytail/ponytail.js](../../adapters/ponytail/ponytail.js)。必须保留 `.js` 扩展名；已验证的 CodeArts 版本不会发现 `.mjs` 入口。

### 3. 把 Skills 安装到 CodeArts 原生目录

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

## 验证

先检查发现结果：

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills |
  Where-Object { $_.name -like "ponytail*" } |
  Select-Object name, location
```

应当看到来自项目 `.codeartsdoer/skills` 目录的 6 个 Ponytail Skills。

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

修改 `.codeartsdoer/package.json` 中的固定版本，使用同样的安全 npm 命令安装，检查上游变更，然后只替换复制过来的 6 个 Ponytail Skill 目录。重新完成“发现检查”和“真实模型调用”后，才能声明新版本可用。

## 卸载

保留其他 CodeArts 插件和 Skills，只删除：

- `.codeartsdoer/plugins/ponytail.js`；
- `.codeartsdoer/skills` 下前文列出的 6 个 Ponytail 目录；
- 使用 `npm uninstall --prefix .codeartsdoer @dietrichgebert/ponytail --ignore-scripts` 移除依赖。

如果使用过 Ponytail 模式切换，请检查用户目录下的 `.config/opencode/.ponytail-active`，确认后可删除。此次验证没有创建该文件。

最后重新运行发现命令，确认项目中不再存在 `ponytail*` Skills。

## 安全说明

- 4.9.0 没有 npm install/postinstall 生命周期脚本，但后续版本仍需重新检查。
- 安装范围限制在项目内，不需要使用 `--auto`。
- Skill 指令会影响 Agent 行为；在敏感仓库中使用前，应阅读固定版本的 `SKILL.md`。
- 复制脚本遇到名称冲突会停止，避免覆盖其他来源的同名 Skill。

## 证据与来源

- [本地实测记录](../../research/2026-08-18.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI Hooks](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0018.html)
- [Ponytail 可移植性说明](https://github.com/DietrichGebert/ponytail/blob/main/docs/agent-portability.md)
- [Ponytail OpenCode 插件](https://github.com/DietrichGebert/ponytail/blob/main/.opencode/plugins/ponytail.mjs)
