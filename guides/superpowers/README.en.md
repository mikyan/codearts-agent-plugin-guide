# Superpowers on CodeArts CLI

[简体中文](README.md)

Superpowers can be installed for one project or for every project used by the current user, adding 14 development-workflow skills to CodeArts CLI.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Team sharing, repository-pinned versions, and use in one project. Recommended by default. |
| User | `~/.codeartsdoer` | General development workflows you use across many projects. |

CodeArts officially gives a same-named **project skill priority over a user skill**. Do not install the same version in both scopes unless the project is intentionally overriding the user installation.

## Install with an agent

Open any project in CodeArts and copy the prompt for the scope you want. Each prompt includes exact paths, file contents, commands, and verification criteria.

### Project installation prompt

```text
Install and verify project-scoped Superpowers 6.3.0 for CodeArts CLI in the current project.

Follow these exact steps:
1. Modify only this project's .codeartsdoer directory. Do not modify ~/.codeartsdoer, global npm packages, or credentials. Never print API keys, CODEARTS_CLI_AK, or CODEARTS_CLI_SK values.
2. Run codearts --version, node --version, npm --version, git --version, and codearts models. Ask me to choose a provider/model ID first if the intended model is ambiguous.
3. Inspect .codeartsdoer/package.json and .codeartsdoer/skills for an existing Superpowers installation. Stop and report any same-name skill or different installed version; do not overwrite it.
4. If .codeartsdoer/package.json does not exist, create it with exactly:
   {
     "private": true,
     "type": "module",
     "dependencies": {
       "superpowers": "git+https://github.com/obra/superpowers.git#v6.3.0"
     }
   }
   If it exists, preserve every existing field, merge the exact superpowers key and value above into dependencies, and ensure type is module.
5. From the project root run:
   npm install --prefix .codeartsdoer --ignore-scripts --no-audit --no-fund
6. Create .codeartsdoer/plugins/superpowers.js with exactly:
   export { SuperpowersPlugin } from "superpowers";
7. From the project root, run this PowerShell exactly. It copies the 14 explicitly named directories from the package to .codeartsdoer/skills and stops if any target already exists:
   $names = @("brainstorming", "dispatching-parallel-agents", "executing-plans", "finishing-a-development-branch", "receiving-code-review", "requesting-code-review", "subagent-driven-development", "systematic-debugging", "test-driven-development", "using-git-worktrees", "using-superpowers", "verification-before-completion", "writing-plans", "writing-skills")
   $source = (Resolve-Path ".codeartsdoer\node_modules\superpowers\skills").Path
   $target = Join-Path (Get-Location) ".codeartsdoer\skills"
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   $conflicts = @($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) })
   if ($conflicts.Count -gt 0) { throw "Existing skill directories: $($conflicts -join ', ')" }
   foreach ($name in $names) { Copy-Item -LiteralPath (Join-Path $source $name) -Destination $target -Recurse }
8. Run codearts debug skill and confirm at least using-superpowers, brainstorming, systematic-debugging, and test-driven-development are present.
9. Replace <selected model> with the model ID confirmed in step 2, then run exactly:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name systematic-debugging. Do not use glob, read, or shell tools. After the skill tool returns, output only its Iron Law sentence."
   Pass only if JSON contains a completed skill event and the final result is NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.
10. Report exact files, commands, tool event, final output, and removal list. Stop and report failure if any step fails; do not claim success.
```

### User installation prompt

```text
Install and verify user-scoped Superpowers 6.3.0 for the current Windows user so every CodeArts CLI project can use it.

Follow these exact steps:
1. In PowerShell run $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer" and use it as the only installation root. Do not modify any project's .codeartsdoer, $userRoot/package.json, global npm packages, or credentials.
2. Run codearts --version, node --version, npm --version, git --version, and codearts models. Ask me to choose a provider/model ID first if the intended model is ambiguous.
3. Check for $userRoot/vendor/superpowers, $userRoot/plugins/superpowers.ts, and all 14 Superpowers directories under $userRoot/skills. Stop and report if any target exists; do not overwrite.
4. Create $userRoot/vendor/superpowers/package.json with exactly:
   {
     "private": true,
     "type": "module",
     "dependencies": {
       "superpowers": "git+https://github.com/obra/superpowers.git#v6.3.0"
     }
   }
5. Run:
   npm install --prefix "$userRoot/vendor/superpowers" --ignore-scripts --no-audit --no-fund
6. Create $userRoot/plugins/superpowers.ts with exactly:
   export { SuperpowersPlugin } from "../vendor/superpowers/node_modules/superpowers/.opencode/plugins/superpowers.js";
7. In the same PowerShell session, run this exactly. It copies the 14 explicitly named directories from the user dependency directory to $userRoot/skills and stops if any target already exists:
   $names = @("brainstorming", "dispatching-parallel-agents", "executing-plans", "finishing-a-development-branch", "receiving-code-review", "requesting-code-review", "subagent-driven-development", "systematic-debugging", "test-driven-development", "using-git-worktrees", "using-superpowers", "verification-before-completion", "writing-plans", "writing-skills")
   $source = (Resolve-Path (Join-Path $userRoot "vendor\superpowers\node_modules\superpowers\skills")).Path
   $target = Join-Path $userRoot "skills"
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   $conflicts = @($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) })
   if ($conflicts.Count -gt 0) { throw "Existing skill directories: $($conflicts -join ', ')" }
   foreach ($name in $names) { Copy-Item -LiteralPath (Join-Path $source $name) -Destination $target -Recurse }
8. In a directory without project-scoped Superpowers, run codearts debug skill. Confirm at least using-superpowers, brainstorming, systematic-debugging, and test-driven-development are present and their locations are under the current user's .codeartsdoer.
9. In the same directory, replace <selected model> with the model ID confirmed in step 2, then run exactly:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name systematic-debugging. Do not use glob, read, or shell tools. After the skill tool returns, output only its Iron Law sentence."
   Pass only if JSON contains a completed skill event, its base directory is ~/.codeartsdoer/skills/systematic-debugging, and the final result is NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.
10. Report exact files, commands, tool event, final output, and removal list. Removal may include only $userRoot/plugins/superpowers.ts, $userRoot/vendor/superpowers, and the 14 user skill directories. Stop and report failure if any step fails.
```

The manual procedures below perform the same operations.

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

### 2. Project installation

#### 2.1 Add the pinned dependency

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

#### 2.2 Add the CodeArts plugin entrypoint

Create `.codeartsdoer/plugins/superpowers.js`:

```js
export { SuperpowersPlugin } from "superpowers";
```

The maintained copy is [adapters/superpowers/superpowers.js](../../adapters/superpowers/superpowers.js).

#### 2.3 Install the skills into CodeArts' native directory

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

### 3. User installation

Use an isolated `vendor/superpowers` dependency directory. **Do not modify** the existing CodeArts user-root `~/.codeartsdoer/package.json`.

#### 3.1 Create an isolated dependency manifest

Create `~/.codeartsdoer/vendor/superpowers/package.json`:

```json
{
  "private": true,
  "type": "module",
  "dependencies": {
    "superpowers": "git+https://github.com/obra/superpowers.git#v6.3.0"
  }
}
```

Install the pinned version:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$vendor = Join-Path $userRoot "vendor\superpowers"
npm install --prefix $vendor --ignore-scripts --no-audit --no-fund
```

#### 3.2 Add the user plugin entrypoint

Create `~/.codeartsdoer/plugins/superpowers.ts`:

```ts
export { SuperpowersPlugin } from "../vendor/superpowers/node_modules/superpowers/.opencode/plugins/superpowers.js";
```

The maintained copy is [adapters/superpowers/superpowers.user.ts](../../adapters/superpowers/superpowers.user.ts).

#### 3.3 Install the user skills

```powershell
$source = (Resolve-Path (Join-Path $vendor "node_modules\superpowers\skills")).Path
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
  plugins/superpowers.ts
  vendor/superpowers/
    package.json
    package-lock.json
    node_modules/superpowers/
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

CodeArts model configuration is user-level. Do not place model credentials in a project manifest, user `vendor` directory, plugin file, or Git.

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

The target Superpowers skills should appear. For a project installation, their locations should be under the current project's `.codeartsdoer`; for a user installation, under the current user's `~/.codeartsdoer`. Verify a user installation from a directory without project-scoped Superpowers so that same-named project skills cannot mask it.

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

- Project: change the pinned Git tag in `<project>/.codeartsdoer/package.json`, reinstall in the project dependency directory, and replace the 14 project skill directories.
- User: change the pinned Git tag in `~/.codeartsdoer/vendor/superpowers/package.json`, reinstall in that `vendor` directory, and replace the 14 user skill directories. Do not modify the user-root `~/.codeartsdoer/package.json`.

For either scope, inspect upstream changes first, then repeat discovery and real model verification before claiming that the new version works.

## Remove

Preserve unrelated CodeArts plugins and skills. Remove only the content in the selected scope.

Project:

- `.codeartsdoer/plugins/superpowers.js`;
- the 14 Superpowers skill directories listed in the expected layout;
- the dependency with `npm uninstall --prefix .codeartsdoer superpowers --ignore-scripts`.

User:

- `~/.codeartsdoer/plugins/superpowers.ts`;
- `~/.codeartsdoer/vendor/superpowers`;
- the 14 Superpowers skill directories listed in the expected user layout.

For a user uninstall, do not delete or rewrite `~/.codeartsdoer/package.json`, `codearts_cli.json`, or other plugins. Review same-name directories before deletion if another integration may also have installed them. Finally, run discovery in the corresponding scope and confirm that the target skills are absent.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Adapter Required** |
| Upstream | [obra/superpowers](https://github.com/obra/superpowers) |
| Upstream version | 6.3.0 (`b36e0829c6d0140e93cfef2ca599b1b07d4a7797`) |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model used | `mimo/mimo-v2.5` |
| Verified scopes | Project and user |
| Last verified | 2026-08-19 |

The project installation was reproduced in two isolated projects. The user installation was then verified from an independent directory with no project-scoped CodeArts configuration. In all three real MiMo sessions, CodeArts' native `skill` tool successfully called `systematic-debugging` and returned its Iron Law. Both scopes were rolled back, after which the third-party skills disappeared from discovery.

Why the adapter is required on CodeArts CLI 26.8.1:

- The local `.js` plugin wrapper loaded successfully.
- Skills added dynamically through the upstream plugin's `config.skills.paths` hook appeared in `codearts debug skill`, but were unavailable to the `skill` tool in a real `codearts run` session.
- Copying the skills into `.codeartsdoer/skills` made them available at runtime.
- The tested user-root `~/.codeartsdoer/package.json` contains CodeArts' internal `@opencode-ai/plugin@26.8.1` dependency. Both system npm and CodeArts automatic reification failed to resolve that version from the public registry. The user adapter therefore uses an isolated `vendor/superpowers` directory and leaves CodeArts' own dependency manifest untouched.

Not yet verified: CodeArts desktop/IDE, Linux, the automatic first-message bootstrap as a separate behavioral assertion, and complete workflows for all skills. Subagent, todo, worktree, and review workflows may require CodeArts-specific tool mapping because some upstream instructions use OpenCode terminology.

## Security notes

- Version 6.3.0 is installed from a pinned Git tag. The tag resolved to the commit recorded above; review the lockfile before committing it.
- The verified package had no dependencies or install/postinstall lifecycle scripts, but future tags must be reviewed again.
- A project installation affects one repository; a user installation affects all CodeArts projects for the current user. Neither requires `--auto`.
- Project skills take priority over same-named user skills. Inspect both scopes before installation so an older version is not hidden in the other scope.
- Superpowers is intentionally prescriptive. Its skill instructions can materially change an agent's workflow, so review them before use on sensitive repositories.
- The copy step intentionally stops on name collisions to avoid overwriting an existing skill source.

## Evidence and sources

- [Project and user verification log](../../research/2026-08-19.en.md)
- [Initial project verification log](../../research/2026-08-18.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI Hooks](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0018.html)
- [Superpowers OpenCode installation](https://github.com/obra/superpowers/blob/main/.opencode/INSTALL.md)
- [Superpowers repository](https://github.com/obra/superpowers)
