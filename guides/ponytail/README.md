# Ponytail on CodeArts CLI

[简体中文](README.zh-CN.md)

## Result

| Item | Verified value |
| --- | --- |
| Compatibility | **Adapter Required** |
| Upstream | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) |
| Upstream version | 4.9.0 (`0a4dd63ad4541f4f655c4108a295916f3c1d8fda`) |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model used | `mimo/mimo-v2.5` |
| Last verified | 2026-08-18 |

Ponytail's six skills work through CodeArts' native `skill` tool after a thin project adapter is installed. The result was reproduced in two isolated projects and rollback-tested.

The upstream OpenCode installation is not sufficient on CodeArts CLI 26.8.1:

- CodeArts did not scan the upstream `.mjs` plugin entrypoint; the adapter uses `.js`.
- Skills added dynamically through the plugin's `config.skills.paths` hook appeared in `codearts debug skill`, but were unavailable to the `skill` tool in a real `codearts run` session.
- The skills must also be copied to the native project path `.codeartsdoer/skills`.

## Scope and limitations

Verified:

- project-local installation from the pinned npm release;
- loading the `.js` plugin wrapper;
- discovery of all six skills;
- a real MiMo session calling `ponytail-help` through the CodeArts `skill` tool;
- a second clean-project reproduction and project-local rollback.

Not verified:

- CodeArts desktop/IDE client or Linux;
- Ponytail's persisted `/ponytail <level>` mode switching;
- every skill's full workflow.

The mode command was not exercised because upstream writes its state to `~/.config/opencode/.ponytail-active`. The tested, recommended path is explicit use of the CodeArts `skill` tool.

## Prerequisites

1. Install and configure CodeArts CLI. See the official [installation](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html), [configuration example](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html), and [AK/SK configuration](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html).
2. Confirm `codearts --version` and `codearts models` work.
3. Install Node.js/npm.
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
    "@dietrichgebert/ponytail": "4.9.0"
  }
}
```

If the file exists, merge this dependency instead of replacing the file.

Install without package lifecycle scripts:

```powershell
npm install --prefix .codeartsdoer --ignore-scripts --no-audit --no-fund
```

### 2. Add the CodeArts plugin entrypoint

Create `.codeartsdoer/plugins/ponytail.js`:

```js
import Ponytail from "@dietrichgebert/ponytail";

export const PonytailPlugin = Ponytail;
```

The maintained copy is [adapters/ponytail/ponytail.js](../../adapters/ponytail/ponytail.js). Keep the `.js` extension; `.mjs` was not discovered in the verified CodeArts version.

### 3. Install the skills into CodeArts' native directory

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

## Security notes

- Version 4.9.0 had no npm install/postinstall lifecycle script, but future releases must be reviewed again.
- Installation is project-local and does not require `--auto`.
- Skill instructions can influence agent behavior. Read the pinned `SKILL.md` files before using them on sensitive repositories.
- The copy step intentionally stops on name collisions to avoid overwriting an existing skill source.

## Evidence and sources

- [Hands-on verification log](../../research/2026-08-18.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI Hooks](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0018.html)
- [Ponytail portability notes](https://github.com/DietrichGebert/ponytail/blob/main/docs/agent-portability.md)
- [Ponytail OpenCode plugin](https://github.com/DietrichGebert/ponytail/blob/main/.opencode/plugins/ponytail.mjs)
