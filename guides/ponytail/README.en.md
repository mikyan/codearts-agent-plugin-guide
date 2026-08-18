# Ponytail on CodeArts CLI

[简体中文](README.md)

Ponytail can be installed for one project or for every project used by the current user, adding six code-simplification and review skills to CodeArts CLI.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Team sharing, repository-pinned versions, and use in one project. Recommended by default. |
| User | `~/.codeartsdoer` | General-purpose tools you use across many projects. |

CodeArts officially gives a same-named **project skill priority over a user skill**. Do not install the same version in both scopes unless the project is intentionally overriding the user installation.

## Install with an agent

Open any project in CodeArts and copy the prompt for the scope you want. Each prompt includes exact paths, file contents, commands, and verification criteria.

### Project installation prompt

```text
Install and verify project-scoped Ponytail 4.9.0 for CodeArts CLI in the current project.

Follow these exact steps:
1. Modify only this project's .codeartsdoer directory. Do not modify ~/.codeartsdoer, global npm packages, or credentials. Never print API keys, CODEARTS_CLI_AK, or CODEARTS_CLI_SK values.
2. Run codearts --version, node --version, npm --version, and codearts models. Ask me to choose a provider/model ID first if the intended model is ambiguous.
3. Inspect .codeartsdoer/package.json and .codeartsdoer/skills for an existing Ponytail installation. Stop and report any same-name skill or different installed version; do not overwrite it.
4. If .codeartsdoer/package.json does not exist, create it with exactly:
   {
     "private": true,
     "type": "module",
     "dependencies": {
       "@dietrichgebert/ponytail": "4.9.0"
     }
   }
   If it exists, preserve every existing field, merge "@dietrichgebert/ponytail": "4.9.0" into dependencies, and ensure type is module.
5. From the project root run:
   npm install --prefix .codeartsdoer --ignore-scripts --no-audit --no-fund
6. Create .codeartsdoer/plugins/ponytail.js with exactly:
   import Ponytail from "@dietrichgebert/ponytail";

   export const PonytailPlugin = Ponytail;
7. From the project root, run this PowerShell exactly. It copies the named directories from the package to .codeartsdoer/skills and stops if any target already exists:
   $names = @("ponytail", "ponytail-audit", "ponytail-debt", "ponytail-gain", "ponytail-help", "ponytail-review")
   $source = (Resolve-Path ".codeartsdoer\node_modules\@dietrichgebert\ponytail\skills").Path
   $target = Join-Path (Get-Location) ".codeartsdoer\skills"
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   $conflicts = @($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) })
   if ($conflicts.Count -gt 0) { throw "Existing skill directories: $($conflicts -join ', ')" }
   foreach ($name in $names) { Copy-Item -LiteralPath (Join-Path $source $name) -Destination $target -Recurse }
8. Run codearts debug skill and confirm ponytail, ponytail-audit, ponytail-debt, ponytail-gain, ponytail-help, and ponytail-review are present.
9. Replace <selected model> with the model ID confirmed in step 2, then run exactly:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name ponytail-help. Do not use glob, read, or shell tools. After the skill tool returns, output the three level names in the same order as its Levels table."
   Pass only if JSON contains a completed skill event and the final result is Lite, Full, Ultra.
10. Report exact files, commands, tool event, final output, and removal list. Stop and report failure if any step fails; do not claim success.
```

### User installation prompt

```text
Install and verify user-scoped Ponytail 4.9.0 for the current Windows user so every CodeArts CLI project can use it.

Follow these exact steps:
1. In PowerShell run $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer" and use it as the only installation root. Do not modify any project's .codeartsdoer, $userRoot/package.json, global npm packages, or credentials.
2. Run codearts --version, node --version, npm --version, and codearts models. Ask me to choose a provider/model ID first if the intended model is ambiguous.
3. Check for $userRoot/vendor/ponytail, $userRoot/plugins/ponytail.ts, and the six ponytail* directories under $userRoot/skills. Stop and report if any target exists; do not overwrite.
4. Create $userRoot/vendor/ponytail/package.json with exactly:
   {
     "private": true,
     "type": "module",
     "dependencies": {
       "@dietrichgebert/ponytail": "4.9.0"
     }
   }
5. Run:
   npm install --prefix "$userRoot/vendor/ponytail" --ignore-scripts --no-audit --no-fund
6. Create $userRoot/plugins/ponytail.ts with exactly:
   import Ponytail from "../vendor/ponytail/node_modules/@dietrichgebert/ponytail/.opencode/plugins/ponytail.mjs";

   export const PonytailPlugin = Ponytail;
7. In the same PowerShell session, run this exactly. It copies the named directories from the user dependency directory to $userRoot/skills and stops if any target already exists:
   $names = @("ponytail", "ponytail-audit", "ponytail-debt", "ponytail-gain", "ponytail-help", "ponytail-review")
   $source = (Resolve-Path (Join-Path $userRoot "vendor\ponytail\node_modules\@dietrichgebert\ponytail\skills")).Path
   $target = Join-Path $userRoot "skills"
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   $conflicts = @($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) })
   if ($conflicts.Count -gt 0) { throw "Existing skill directories: $($conflicts -join ', ')" }
   foreach ($name in $names) { Copy-Item -LiteralPath (Join-Path $source $name) -Destination $target -Recurse }
8. In a directory without project-scoped Ponytail, run codearts debug skill. Confirm all six skills are present and their locations are under the current user's .codeartsdoer.
9. In the same directory, replace <selected model> with the model ID confirmed in step 2, then run exactly:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name ponytail-help. Do not use glob, read, or shell tools. After the skill tool returns, output the three level names in the same order as its Levels table."
   Pass only if JSON contains a completed skill event, its base directory is ~/.codeartsdoer/skills/ponytail-help, and the final result is Lite, Full, Ultra.
10. Report exact files, commands, tool event, final output, and removal list. Removal may include only $userRoot/plugins/ponytail.ts, $userRoot/vendor/ponytail, and the six user skill directories. Stop and report failure if any step fails.
```

The manual procedures below perform the same operations.

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

### 2. Project installation

#### 2.1 Add the pinned dependency

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

#### 2.2 Add the CodeArts plugin entrypoint

Create `.codeartsdoer/plugins/ponytail.js`:

```js
import Ponytail from "@dietrichgebert/ponytail";

export const PonytailPlugin = Ponytail;
```

The maintained copy is [adapters/ponytail/ponytail.js](../../adapters/ponytail/ponytail.js). Keep the `.js` extension; `.mjs` was not discovered in the verified CodeArts version.

#### 2.3 Install the skills into CodeArts' native directory

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

### 3. User installation

Use an isolated `vendor/ponytail` dependency directory. **Do not modify** the existing CodeArts user-root `~/.codeartsdoer/package.json`.

#### 3.1 Create an isolated dependency manifest

Create `~/.codeartsdoer/vendor/ponytail/package.json`:

```json
{
  "private": true,
  "type": "module",
  "dependencies": {
    "@dietrichgebert/ponytail": "4.9.0"
  }
}
```

Install the pinned version:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$vendor = Join-Path $userRoot "vendor\ponytail"
npm install --prefix $vendor --ignore-scripts --no-audit --no-fund
```

#### 3.2 Add the user plugin entrypoint

Create `~/.codeartsdoer/plugins/ponytail.ts`:

```ts
import Ponytail from "../vendor/ponytail/node_modules/@dietrichgebert/ponytail/.opencode/plugins/ponytail.mjs";

export const PonytailPlugin = Ponytail;
```

The maintained user-scoped copy is [adapters/ponytail/ponytail.user.ts](../../adapters/ponytail/ponytail.user.ts).

#### 3.3 Install the user skills

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

Expected user layout:

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

## Configure CodeArts

CodeArts model configuration is user-level. Do not put model credentials in project dependency manifests, user `vendor` directories, plugin files, or Git.

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

All six Ponytail skills should be listed. Project installations resolve under the current project's `.codeartsdoer`; user installations resolve under the current user's `~/.codeartsdoer`. Verify user scope from a directory without project-scoped Ponytail so project priority cannot hide the result.

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

- Project: change the pinned version in `<project>/.codeartsdoer/package.json`, reinstall in the project dependency directory, and replace the six project skill directories.
- User: change the pinned version in `~/.codeartsdoer/vendor/ponytail/package.json`, reinstall in that vendor directory, and replace the six user skill directories. Do not modify user-root `~/.codeartsdoer/package.json`.

For either scope, review upstream changes and repeat discovery plus real model verification.

## Remove

Preserve unrelated CodeArts plugins and skills. Remove only items in the selected scope.

Project scope:

- `.codeartsdoer/plugins/ponytail.js`;
- the six Ponytail directories listed above under `.codeartsdoer/skills`;
- the `@dietrichgebert/ponytail` dependency with `npm uninstall --prefix .codeartsdoer @dietrichgebert/ponytail --ignore-scripts`.

User scope:

- `~/.codeartsdoer/plugins/ponytail.ts`;
- `~/.codeartsdoer/vendor/ponytail`;
- the six Ponytail directories listed above under `~/.codeartsdoer/skills`.

Do not delete or rewrite user-root `~/.codeartsdoer/package.json`, `codearts_cli.json`, or unrelated plugins.

If you used Ponytail mode switching, review and optionally remove the upstream state file at `.config/opencode/.ponytail-active` under your user profile. It was not created during this verification.

Finally, verify the selected scope again and confirm the target `ponytail*` skills are absent.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Adapter Required** |
| Upstream | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) |
| Upstream version | 4.9.0 (`0a4dd63ad4541f4f655c4108a295916f3c1d8fda`) |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model used | `mimo/mimo-v2.5` |
| Verified scopes | Project and user |
| Last verified | 2026-08-19 |

Project installation was reproduced in two isolated projects. User installation was then verified from an independent directory with no project CodeArts configuration. In all three real sessions, MiMo called `ponytail-help` through CodeArts' native `skill` tool and returned `Lite, Full, Ultra`. Both scopes were rolled back successfully, after which the third-party skills disappeared.

Why the adapter is required on CodeArts CLI 26.8.1:

- CodeArts did not scan the upstream `.mjs` plugin entrypoint; `.js` loaded successfully.
- Skills added dynamically through the upstream plugin's `config.skills.paths` hook appeared in `codearts debug skill`, but were unavailable to the `skill` tool in a real `codearts run` session.
- Copying the skills into `.codeartsdoer/skills` made them available at runtime.
- The tested user-root `~/.codeartsdoer/package.json` contains CodeArts' internal `@opencode-ai/plugin@26.8.1`; neither system npm nor CodeArts reification could resolve that version from the public registry. The user adapter therefore uses isolated `vendor/ponytail` dependencies and leaves CodeArts' own manifest untouched.

Not yet verified: CodeArts desktop/IDE, Linux, persisted `/ponytail <level>` mode switching, and every skill's full workflow. The mode command was intentionally not exercised because upstream writes state to `~/.config/opencode/.ponytail-active`.

## Security notes

- Version 4.9.0 had no npm install/postinstall lifecycle script, but future releases must be reviewed again.
- Project scope affects one repository; user scope affects every CodeArts project for the current user. Neither requires `--auto`.
- Project skills take priority over same-named user skills. Check both scopes before installing so an older copy is not hidden elsewhere.
- Skill instructions can influence agent behavior. Read the pinned `SKILL.md` files before using them on sensitive repositories.
- The copy step intentionally stops on name collisions to avoid overwriting an existing skill source.

## Evidence and sources

- [Project and user verification log](../../research/2026-08-19.en.md)
- [Initial project verification log](../../research/2026-08-18.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI Hooks](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0018.html)
- [Ponytail portability notes](https://github.com/DietrichGebert/ponytail/blob/main/docs/agent-portability.md)
- [Ponytail OpenCode plugin](https://github.com/DietrichGebert/ponytail/blob/main/.opencode/plugins/ponytail.mjs)
