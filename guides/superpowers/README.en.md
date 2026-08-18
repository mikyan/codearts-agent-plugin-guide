# Superpowers on CodeArts CLI

[简体中文](README.md)

Install Superpowers in a project to add its 14 development-workflow skills to CodeArts CLI.

## Install with an agent

Open the target project in CodeArts, paste the prompt below, and review the proposed changes before allowing installation:

```text
Install Superpowers 6.3.0 for CodeArts CLI in the current project.

Requirements:
1. Work only inside this project's .codeartsdoer directory. Do not modify user-level CodeArts configuration, global npm packages, or credential environment variables.
2. Do not print or record API keys, CODEARTS_CLI_AK, or CODEARTS_CLI_SK values.
3. Check that codearts, Node.js, npm, and Git are available. Run codearts models and ask me to choose a provider/model ID only if the intended model is ambiguous.
4. Inspect existing .codeartsdoer/package.json, plugins, and skills before editing. Merge changes and stop on same-name skill conflicts; do not overwrite unrelated configuration.
5. Add the exact dependency superpowers from git+https://github.com/obra/superpowers.git#v6.3.0 and install it with npm lifecycle scripts disabled.
6. Create .codeartsdoer/plugins/superpowers.js as an ES module that re-exports SuperpowersPlugin from the superpowers package.
7. Copy all 14 package skill directories from node_modules into .codeartsdoer/skills without overwriting existing directories.
8. Verify discovery with codearts debug skill.
9. Run a sandboxed, non-interactive CodeArts test with the selected model. Require the model to call the skill tool with name systematic-debugging and confirm that the completed tool call returns its Iron Law: NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST. A text answer without a successful skill tool event does not pass.
10. Report the exact files changed, commands run, verification evidence, conflicts or limitations, and precise rollback steps. If any verification fails, stop and report failure instead of claiming success.
```

The manual procedure below describes exactly what the agent should do.

## Install manually on Windows

### 1. Check prerequisites

- Install CodeArts CLI using the official [installation guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html).
- Install Node.js/npm and Git.
- Run the steps from the target project's root directory.

```powershell
codearts --version
node --version
npm --version
git --version
```

### 2. Add the pinned dependency

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

### 3. Add the CodeArts plugin entrypoint

Create `.codeartsdoer/plugins/superpowers.js`:

```js
export { SuperpowersPlugin } from "superpowers";
```

The maintained copy is [adapters/superpowers/superpowers.js](../../adapters/superpowers/superpowers.js).

### 4. Install the skills into CodeArts' native directory

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

## Configure CodeArts

CodeArts model configuration is user-level; do not place model credentials in this project.

1. Follow the official [configuration example](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html) and [AK/SK configuration](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html).
2. Open a new PowerShell window if environment variables were added after CodeArts started.
3. Confirm the model you intend to use appears in the `provider/model` format:

```powershell
codearts models
```

The examples below use `mimo/mimo-v2.5`; substitute your configured model ID.

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

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Adapter Required** |
| Upstream | [obra/superpowers](https://github.com/obra/superpowers) |
| Upstream version | 6.3.0 (`b36e0829c6d0140e93cfef2ca599b1b07d4a7797`) |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model used | `mimo/mimo-v2.5` |
| Last verified | 2026-08-18 |

The adapted installation was completed in two isolated projects. In both, a real MiMo session successfully called `systematic-debugging` through CodeArts' native `skill` tool and returned its Iron Law. Removing the project `.codeartsdoer` installation made the third-party skills disappear, confirming the rollback boundary.

Why the adapter is required on CodeArts CLI 26.8.1:

- The local `.js` plugin wrapper loaded successfully.
- Skills added dynamically through the upstream plugin's `config.skills.paths` hook appeared in `codearts debug skill`, but were unavailable to the `skill` tool in a real `codearts run` session.
- Copying the skills into `.codeartsdoer/skills` made them available at runtime.

Not yet verified: CodeArts desktop/IDE, Linux, the automatic first-message bootstrap as a separate behavioral assertion, and complete workflows for all skills. Subagent, todo, worktree, and review workflows may require CodeArts-specific tool mapping because some upstream instructions use OpenCode terminology.

## Security notes

- Version 6.3.0 is installed from a pinned Git tag. The tag resolved to the commit recorded above; review the lockfile before committing it.
- The verified package had no dependencies or install/postinstall lifecycle scripts, but future tags must be reviewed again.
- Installation is project-local and does not require `--auto`.
- Superpowers is intentionally prescriptive. Its skill instructions can materially change an agent's workflow, so review them before use on sensitive repositories.
- The copy step intentionally stops on name collisions to avoid overwriting an existing skill source.

## Evidence and sources

- [Hands-on verification log](../../research/2026-08-18.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI Hooks](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0018.html)
- [Superpowers OpenCode installation](https://github.com/obra/superpowers/blob/main/.opencode/INSTALL.md)
- [Superpowers repository](https://github.com/obra/superpowers)
