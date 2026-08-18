# Superpowers on CodeArts CLI

[简体中文](README.zh-CN.md)

## Result

| Item | Verified value |
| --- | --- |
| Compatibility | **Adapter Required** |
| Upstream | [obra/superpowers](https://github.com/obra/superpowers) |
| Upstream version | 6.3.0 (`b36e0829c6d0140e93cfef2ca599b1b07d4a7797`) |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model used | `mimo/mimo-v2.5` |
| Last verified | 2026-08-18 |

Superpowers skills work through CodeArts' native `skill` tool after a thin project adapter is installed. The result was reproduced in two isolated projects and rollback-tested.

The upstream OpenCode installation is not sufficient on CodeArts CLI 26.8.1:

- CodeArts project plugins need a local `.js` entrypoint.
- The upstream plugin's dynamic `config.skills.paths` registration appeared in `codearts debug skill`, but a real `codearts run` session could not load those skills.
- The upstream skills must also be copied to CodeArts' native project path, `.codeartsdoer/skills`.

## Scope and limitations

Verified:

- project-local installation from the pinned Git tag;
- loading the `.js` plugin wrapper;
- discovery of all 14 upstream skills;
- a real MiMo session calling `systematic-debugging` through the CodeArts `skill` tool;
- a second clean-project reproduction and project-local rollback.

Not verified:

- CodeArts desktop/IDE client or Linux;
- the automatic first-message bootstrap as a separate behavioral assertion;
- complete workflows for every skill, especially subagent, todo, worktree, and review flows.

Some Superpowers instructions use OpenCode terminology. CodeArts exposes many compatible tools, but complex workflows can still require host-specific judgment. Treat the verification as proof of installation, discovery, and core skill invocation—not proof that every workflow is fully portable.

## Prerequisites

1. Install and configure CodeArts CLI. See the official [installation](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html), [configuration example](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html), and [AK/SK configuration](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html).
2. Confirm `codearts --version` and `codearts models` work.
3. Install Node.js/npm and Git.
4. Run the following steps from the target project's root directory.

Keep provider API keys and real Huawei Cloud credentials out of the project and Git history.

## Install on Windows PowerShell

### 1. Add the pinned dependency

Create `.codeartsdoer/package.json` if the project does not already have one:

```json
{
  "private": true,
  "type": "module",
  "dependencies": {
    "superpowers": "git+https://github.com/obra/superpowers.git#v6.3.0"
  }
}
```

If the file exists, merge this dependency instead of replacing the file.

Install without package lifecycle scripts:

```powershell
npm install --prefix .codeartsdoer --ignore-scripts --no-audit --no-fund
```

### 2. Add the CodeArts plugin entrypoint

Create `.codeartsdoer/plugins/superpowers.js`:

```js
export { SuperpowersPlugin } from "superpowers";
```

The maintained copy is [adapters/superpowers/superpowers.js](../../adapters/superpowers/superpowers.js).

### 3. Install the skills into CodeArts' native directory

The following refuses to overwrite same-named skills already present in the project:

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

Expected project layout:

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

## Verify

First check discovery:

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$expected = @("using-superpowers", "systematic-debugging", "brainstorming")
$skills |
  Where-Object { $expected -contains $_.name } |
  Select-Object name, location
```

The listed skills should come from the project's `.codeartsdoer/skills` directory.

Then perform a real model invocation. Replace the model ID if needed:

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name systematic-debugging. Do not use glob, read, or shell tools. After the skill tool returns, output only its Iron Law sentence."
```

Passing evidence includes a JSON event with `"tool":"skill"`, `"status":"completed"`, and this final text:

```text
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

A plausible answer without a completed `skill` event is not sufficient verification.

## Use

Ask CodeArts to call a specific skill explicitly, for example:

```text
Call the skill tool with name brainstorming before helping me design this feature.
```

For debugging:

```text
Call the skill tool with name systematic-debugging, then investigate this failing test. Do not change files until the root cause is established.
```

Read each skill before permitting file changes or shell commands. Skills that mention OpenCode `task`, `todowrite`, or other host-specific tools may need translation to the tools available in the current CodeArts session.

## Update

Change the pinned Git tag in `.codeartsdoer/package.json`, run the same safe npm install, inspect upstream changes, and replace only the 14 copied Superpowers skill directories. Re-run both discovery and real model verification before claiming the new version works.

## Remove

Preserve unrelated CodeArts plugins and skills. Remove only:

- `.codeartsdoer/plugins/superpowers.js`;
- the 14 Superpowers skill directories listed in the expected layout;
- the dependency with `npm uninstall --prefix .codeartsdoer superpowers --ignore-scripts`.

Review same-name directories before deletion if another integration may also have installed them. Finally, run the discovery command again and confirm that the project-local Superpowers skills are absent.

## Security notes

- Version 6.3.0 is installed from a pinned Git tag. The tag resolved to the commit recorded above; review the lockfile before committing it.
- The verified package had no dependencies or install/postinstall lifecycle scripts, but future tags must be reviewed again.
- Installation is project-local and does not require `--auto`.
- Superpowers is intentionally prescriptive. Its skill instructions can materially change an agent's workflow, so review them before use on sensitive repositories.
- The copy step intentionally stops on name collisions to avoid overwriting an existing skill source.

## Evidence and sources

- [Hands-on verification log](../../research/2026-08-18.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI Hooks](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0018.html)
- [Superpowers OpenCode installation](https://github.com/obra/superpowers/blob/main/.opencode/INSTALL.md)
- [Superpowers repository](https://github.com/obra/superpowers)
