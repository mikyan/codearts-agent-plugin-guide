# Ponytail on CodeArts CLI

[简体中文](README.md)

Install Ponytail in a project to add its six code-simplification and review skills to CodeArts CLI.

## Install with an agent

Open the target project in CodeArts, paste the prompt below, and review the proposed changes before allowing installation:

```text
Install Ponytail 4.9.0 for CodeArts CLI in the current project.

Requirements:
1. Work only inside this project's .codeartsdoer directory. Do not modify user-level CodeArts configuration, global npm packages, or credential environment variables.
2. Do not print or record API keys, CODEARTS_CLI_AK, or CODEARTS_CLI_SK values.
3. Check that codearts, Node.js, and npm are available. Run codearts models and ask me to choose a provider/model ID only if the intended model is ambiguous.
4. Inspect existing .codeartsdoer/package.json, plugins, and skills before editing. Merge changes and stop on same-name skill conflicts; do not overwrite unrelated configuration.
5. Add the exact dependency @dietrichgebert/ponytail 4.9.0 and install it with npm lifecycle scripts disabled.
6. Create .codeartsdoer/plugins/ponytail.js as an ES module that imports the package default export and re-exports it as PonytailPlugin. Do not use an .mjs entrypoint.
7. Copy the package's six skill directories from node_modules into .codeartsdoer/skills without overwriting existing directories.
8. Verify discovery with codearts debug skill.
9. Run a sandboxed, non-interactive CodeArts test with the selected model. Require the model to call the skill tool with name ponytail-help and confirm that the completed tool call returns the levels Lite, Full, Ultra in that order. A text answer without a successful skill tool event does not pass.
10. Report the exact files changed, commands run, verification evidence, conflicts or limitations, and precise rollback steps. If any verification fails, stop and report failure instead of claiming success.
```

The manual procedure below describes exactly what the agent should do.

## Install manually on Windows

### 1. Check prerequisites

- Install CodeArts CLI using the official [installation guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html).
- Install Node.js/npm.
- Run the steps from the target project's root directory.

```powershell
codearts --version
node --version
npm --version
```

### 2. Add the pinned dependency

Create `.codeartsdoer/package.json` if the project does not already have one:

```json
{
  "private": true,
  "type": "module",
  "dependencies": {
    "@dietrichgebert/ponytail": "4.9.0"
  }
}
```

If the file exists, merge this dependency instead of replacing the file.

Install without package lifecycle scripts:

```powershell
npm install --prefix .codeartsdoer --ignore-scripts --no-audit --no-fund
```

### 3. Add the CodeArts plugin entrypoint

Create `.codeartsdoer/plugins/ponytail.js`:

```js
import Ponytail from "@dietrichgebert/ponytail";

export const PonytailPlugin = Ponytail;
```

The maintained copy is [adapters/ponytail/ponytail.js](../../adapters/ponytail/ponytail.js). Keep the `.js` extension; `.mjs` was not discovered in the verified CodeArts version.

### 4. Install the skills into CodeArts' native directory

The following refuses to overwrite same-named skills already present in the project:

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

Expected project layout:

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
$skills |
  Where-Object { $_.name -like "ponytail*" } |
  Select-Object name, location
```

Six Ponytail skills should be listed from the project's `.codeartsdoer/skills` directory.

Then perform a real model invocation. Replace the model ID if needed:

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name ponytail-help. Do not use glob, read, or shell tools. After the skill tool returns, output the three level names in the same order as its Levels table."
```

Passing evidence includes a JSON event with `"tool":"skill"`, `"status":"completed"`, and this final text:

```text
Lite, Full, Ultra
```

A plausible answer without a completed `skill` event is not sufficient verification.

## Use

Ask CodeArts to call a specific skill explicitly, for example:

```text
Call the skill tool with name ponytail-review, then review the current diff for unnecessary complexity. Do not apply changes.
```

Available skills are `ponytail`, `ponytail-review`, `ponytail-audit`, `ponytail-debt`, `ponytail-gain`, and `ponytail-help`.

## Update

Change the pinned version in `.codeartsdoer/package.json`, run the same safe npm install, inspect upstream changes, and replace only the six copied Ponytail skill directories. Re-run both discovery and real model verification before claiming the new version works.

## Remove

Preserve unrelated CodeArts plugins and skills. Remove only:

- `.codeartsdoer/plugins/ponytail.js`;
- the six Ponytail directories listed above under `.codeartsdoer/skills`;
- the `@dietrichgebert/ponytail` dependency with `npm uninstall --prefix .codeartsdoer @dietrichgebert/ponytail --ignore-scripts`.

If you used Ponytail mode switching, review and optionally remove the upstream state file at `.config/opencode/.ponytail-active` under your user profile. It was not created during this verification.

Finally, run the discovery command again and confirm that no `ponytail*` project skills remain.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Adapter Required** |
| Upstream | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) |
| Upstream version | 4.9.0 (`0a4dd63ad4541f4f655c4108a295916f3c1d8fda`) |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model used | `mimo/mimo-v2.5` |
| Last verified | 2026-08-18 |

The adapted installation was completed in two isolated projects. In both, a real MiMo session successfully called `ponytail-help` through CodeArts' native `skill` tool and returned `Lite, Full, Ultra`. Removing the project `.codeartsdoer` installation made the third-party skills disappear, confirming the rollback boundary.

Why the adapter is required on CodeArts CLI 26.8.1:

- CodeArts did not scan the upstream `.mjs` plugin entrypoint; `.js` loaded successfully.
- Skills added dynamically through the upstream plugin's `config.skills.paths` hook appeared in `codearts debug skill`, but were unavailable to the `skill` tool in a real `codearts run` session.
- Copying the skills into `.codeartsdoer/skills` made them available at runtime.

Not yet verified: CodeArts desktop/IDE, Linux, persisted `/ponytail <level>` mode switching, and every skill's full workflow. The mode command was intentionally not exercised because upstream writes state to `~/.config/opencode/.ponytail-active`.

## Security notes

- Version 4.9.0 had no npm install/postinstall lifecycle script, but future releases must be reviewed again.
- Installation is project-local and does not require `--auto`.
- Skill instructions can influence agent behavior. Read the pinned `SKILL.md` files before using them on sensitive repositories.
- The copy step intentionally stops on name collisions to avoid overwriting an existing skill source.

## Evidence and sources

- [Hands-on verification log](../../research/2026-08-18.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI Hooks](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0018.html)
- [Ponytail portability notes](https://github.com/DietrichGebert/ponytail/blob/main/docs/agent-portability.md)
- [Ponytail OpenCode plugin](https://github.com/DietrichGebert/ponytail/blob/main/.opencode/plugins/ponytail.mjs)
