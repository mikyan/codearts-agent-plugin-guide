# 在 CodeArts CLI 中使用 Superpowers

[English](README.md)

## 验证结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Adapter Required** |
| 上游项目 | [obra/superpowers](https://github.com/obra/superpowers) |
| 上游版本 | 6.3.0（`b36e0829c6d0140e93cfef2ca599b1b07d4a7797`） |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 最后验证日期 | 2026-08-18 |

安装轻量的项目级适配器后，Superpowers Skills 可以通过 CodeArts 原生 `skill` 工具使用。该结果已在两个隔离项目中重复验证，并完成回滚测试。

CodeArts CLI 26.8.1 不能直接照搬上游 OpenCode 安装方法：

- CodeArts 项目插件需要本地 `.js` 入口。
- 上游插件通过 `config.skills.paths` 动态注册的 Skills 会出现在 `codearts debug skill` 中，但真实 `codearts run` 会话无法加载它们。
- 因此还必须把上游 Skills 复制到 CodeArts 原生项目目录 `.codeartsdoer/skills`。

## 验证范围与限制

已验证：

- 从固定 Git Tag 进行项目级安装；
- 加载 `.js` 插件包装入口；
- 发现上游全部 14 个 Skills；
- MiMo 在真实会话中通过 CodeArts `skill` 工具调用 `systematic-debugging`；
- 第二个干净项目复现，以及项目级回滚。

未验证：

- CodeArts 桌面端/IDE 客户端和 Linux；
- 把“首条消息自动注入”单独作为行为进行断言；
- 每个 Skill 的完整工作流，尤其是子 Agent、Todo、Worktree 和代码审查流程。

部分 Superpowers 指令使用 OpenCode 术语。CodeArts 提供了不少兼容工具，但复杂流程仍可能需要针对宿主做判断。因此，本次结果证明的是安装、发现和核心 Skill 调用可用，并不代表所有工作流都已完全移植。

## 前置条件

1. 安装并配置 CodeArts CLI。参考官方的[安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)、[配置示例](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html)和 [AK/SK 配置](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html)。
2. 确认 `codearts --version` 和 `codearts models` 可正常运行。
3. 安装 Node.js/npm 和 Git。
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
    "superpowers": "git+https://github.com/obra/superpowers.git#v6.3.0"
  }
}
```

如果文件已经存在，只合并该依赖，不要覆盖原内容。

禁止执行包生命周期脚本并安装依赖：

```powershell
npm install --prefix .codeartsdoer --ignore-scripts --no-audit --no-fund
```

### 2. 添加 CodeArts 插件入口

创建 `.codeartsdoer/plugins/superpowers.js`：

```js
export { SuperpowersPlugin } from "superpowers";
```

仓库中维护的副本见 [adapters/superpowers/superpowers.js](../../adapters/superpowers/superpowers.js)。

### 3. 把 Skills 安装到 CodeArts 原生目录

以下脚本遇到项目中已有的同名 Skill 时会停止，不会直接覆盖：

```powershell
$source = (Resolve-Path ".codeartsdoer\node_modules\superpowers\skills").Path
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
  node_modules/superpowers/
  plugins/superpowers.js
  skills/
    brainstorming/
    dispatching-parallel-agents/
    executing-plans/
    finishing-a-development-branch/
    receiving-code-review/
    requesting-code-review/
    subagent-driven-development/
    systematic-debugging/
    test-driven-development/
    using-git-worktrees/
    using-superpowers/
    verification-before-completion/
    writing-plans/
    writing-skills/
```

## 验证

先检查发现结果：

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$expected = @("using-superpowers", "systematic-debugging", "brainstorming")
$skills |
  Where-Object { $expected -contains $_.name } |
  Select-Object name, location
```

列出的 Skills 应来自项目 `.codeartsdoer/skills` 目录。

然后进行真实模型调用；如有需要，请替换模型 ID：

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name systematic-debugging. Do not use glob, read, or shell tools. After the skill tool returns, output only its Iron Law sentence."
```

通过时，JSON 事件中应包含 `"tool":"skill"`、`"status":"completed"`，最终文本为：

```text
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

如果模型只是给出了看似正确的答案，但没有成功的 `skill` 事件，不能视为验证通过。

## 使用

建议明确要求 CodeArts 调用指定 Skill，例如：

```text
Call the skill tool with name brainstorming before helping me design this feature.
```

排查故障时可以使用：

```text
Call the skill tool with name systematic-debugging, then investigate this failing test. Do not change files until the root cause is established.
```

在允许修改文件或执行命令前，应先阅读对应 Skill。提到 OpenCode `task`、`todowrite` 或其他宿主专用工具的 Skills，可能需要映射到当前 CodeArts 会话实际提供的工具。

## 更新

修改 `.codeartsdoer/package.json` 中固定的 Git Tag，使用同样的安全 npm 命令安装，检查上游变更，然后只替换复制过来的 14 个 Superpowers Skill 目录。重新完成“发现检查”和“真实模型调用”后，才能声明新版本可用。

## 卸载

保留其他 CodeArts 插件和 Skills，只删除：

- `.codeartsdoer/plugins/superpowers.js`；
- 预期结构中列出的 14 个 Superpowers Skill 目录；
- 使用 `npm uninstall --prefix .codeartsdoer superpowers --ignore-scripts` 移除依赖。

如果其他集成可能安装过同名目录，删除前必须检查。最后重新运行发现命令，确认项目级 Superpowers Skills 已消失。

## 安全说明

- 6.3.0 从固定 Git Tag 安装，该 Tag 解析到上表记录的 Commit；提交 lockfile 前应检查其内容。
- 已验证版本没有依赖，也没有 install/postinstall 生命周期脚本，但后续 Tag 仍需重新检查。
- 安装范围限制在项目内，不需要使用 `--auto`。
- Superpowers 的设计本身具有较强流程约束，Skill 指令会明显改变 Agent 工作方式；在敏感仓库中使用前应先审阅。
- 复制脚本遇到名称冲突会停止，避免覆盖其他来源的同名 Skill。

## 证据与来源

- [本地实测记录](../../research/2026-08-18.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI Hooks](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0018.html)
- [Superpowers OpenCode 安装说明](https://github.com/obra/superpowers/blob/main/.opencode/INSTALL.md)
- [Superpowers 仓库](https://github.com/obra/superpowers)
