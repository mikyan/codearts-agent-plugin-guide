# 在 CodeArts CLI 中使用 Superpowers

[English](README.en.md)

在项目中安装 Superpowers，即可为 CodeArts CLI 增加 14 个开发工作流 Skills。

## 让 Agent 帮你安装

在 CodeArts 中打开目标项目，把下面整段提示词发给 Agent；执行安装前先检查它计划修改的内容：

```text
请在当前项目中为 CodeArts CLI 安装 Superpowers 6.3.0。

要求：
1. 只允许修改当前项目的 .codeartsdoer 目录。不要修改用户级 CodeArts 配置、全局 npm 包或凭据环境变量。
2. 不要输出或记录 API Key、CODEARTS_CLI_AK、CODEARTS_CLI_SK 的值。
3. 检查 codearts、Node.js、npm 和 Git 是否可用。运行 codearts models；只有无法判断应使用哪个模型时，才询问我选择 provider/model ID。
4. 修改前检查已有的 .codeartsdoer/package.json、plugins 和 skills。采用合并方式，遇到同名 Skill 时停止，不要覆盖无关配置。
5. 添加精确依赖 superpowers，来源固定为 git+https://github.com/obra/superpowers.git#v6.3.0，并在禁用 npm 生命周期脚本的情况下安装。
6. 创建 .codeartsdoer/plugins/superpowers.js：使用 ES Module 从 superpowers 包重新导出 SuperpowersPlugin。
7. 把包中全部 14 个 Skill 目录从 node_modules 复制到 .codeartsdoer/skills，不得覆盖已有目录。
8. 使用 codearts debug skill 检查发现结果。
9. 使用选定模型执行一次沙箱、非交互 CodeArts 测试。明确要求模型调用名为 systematic-debugging 的 skill 工具，并确认成功的工具调用返回 Iron Law：NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST。只有文本答案、没有成功的 skill 工具事件，不能算通过。
10. 最后报告修改的准确文件、执行的命令、验证证据、冲突或限制，以及精确回滚步骤。如果任何验证失败，停止并如实报告，不能声明安装成功。
```

下面的手动步骤就是 Agent 应当执行的完整流程。

## Windows 手动安装

### 1. 检查前置条件

- 按官方[安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)安装 CodeArts CLI。
- 安装 Node.js/npm 和 Git。
- 在目标项目根目录执行后续步骤。

```powershell
codearts --version
node --version
npm --version
git --version
```

### 2. 添加固定版本依赖

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

### 3. 添加 CodeArts 插件入口

创建 `.codeartsdoer/plugins/superpowers.js`：

```js
export { SuperpowersPlugin } from "superpowers";
```

仓库中维护的副本见 [adapters/superpowers/superpowers.js](../../adapters/superpowers/superpowers.js)。

### 4. 把 Skills 安装到 CodeArts 原生目录

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

## 配置 CodeArts

CodeArts 模型配置属于用户级配置，不要把模型凭据放进当前项目。

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

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Adapter Required** |
| 上游项目 | [obra/superpowers](https://github.com/obra/superpowers) |
| 上游版本 | 6.3.0（`b36e0829c6d0140e93cfef2ca599b1b07d4a7797`） |
| 许可证 | MIT |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 最后验证日期 | 2026-08-18 |

适配后的安装已在两个隔离项目中完成。两次测试里，MiMo 都通过 CodeArts 原生 `skill` 工具成功调用了 `systematic-debugging`，并返回其 Iron Law。临时移走项目 `.codeartsdoer` 后，第三方 Skills 会从发现结果中消失，说明回滚边界确实限制在项目内。

CodeArts CLI 26.8.1 需要适配器的原因：

- 本地 `.js` 插件包装入口加载成功。
- 上游插件通过 `config.skills.paths` 动态加入的 Skills 会出现在 `codearts debug skill`，但真实 `codearts run` 会话的 `skill` 工具无法调用。
- 把 Skills 复制到 `.codeartsdoer/skills` 后，运行时调用成功。

尚未验证：CodeArts 桌面端/IDE、Linux、把“首条消息自动注入”单独作为行为进行断言，以及全部 Skills 的完整工作流。子 Agent、Todo、Worktree 和代码审查流程可能需要 CodeArts 专用工具映射，因为部分上游指令使用 OpenCode 术语。

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
