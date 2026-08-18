# Humanizer on CodeArts CLI

[简体中文](README.md)

Install the verified `humanizer` skill to remove common AI-writing patterns while preserving facts and the writer's voice.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Repository-pinned versions, team sharing, or one project. Recommended by default. |
| User | `~/.codeartsdoer` | Repeated use across projects for the current user. |

A same-named project skill takes priority over a user skill. Do not install the same version in both scopes unless the project intentionally overrides the user copy.

## Install with an agent

### Project installation prompt

```text
Install and verify the humanizer skill from blader/humanizer for CodeArts CLI in the current project, pinned to commit ebf637bdae86b62a7b006c447bb98cfa0ccc979d.

Follow exactly:
1. Modify only this project's .codeartsdoer. Do not modify ~/.codeartsdoer, credentials, or global software.
2. Run codearts --version, git --version, and codearts models. Ask me to select a provider/model ID if ambiguous.
3. Check .codeartsdoer/vendor/humanizer and .codeartsdoer/skills/humanizer. Stop and report if either exists; never overwrite.
4. From the project root in PowerShell run exactly:
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\humanizer"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\humanizer"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/blader/humanizer.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/humanizer"
   git -C $source checkout --detach "ebf637bdae86b62a7b006c447bb98cfa0ccc979d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "ebf637bdae86b62a7b006c447bb98cfa0ccc979d") { throw "Unexpected commit." }
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   Copy-Item -LiteralPath (Join-Path $source "SKILL.md") -Destination (Join-Path $target "SKILL.md")
5. Run codearts debug skill. Confirm humanizer resolves under this project's .codeartsdoer/skills/humanizer/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name humanizer. Do not use any other tool. Use embedded mode and output only the final rewrite of this sentence, preserving its launch year: Launched in 2024, this groundbreaking platform stands as a pivotal testament to innovation and promises a bright future."
7. Passing criteria: JSON must contain exactly one completed `humanizer` skill call. The final result must preserve `2024` and remove `groundbreaking`, `pivotal`, `testament`, and `bright future`.
8. Report the commit, changed paths, tool event, final result, and removal list. If any step fails, stop and do not claim success.
```

### User installation prompt

```text
Install and verify the user-scoped humanizer skill from blader/humanizer for the current Windows user, pinned to commit ebf637bdae86b62a7b006c447bb98cfa0ccc979d.

Follow exactly:
1. In PowerShell run $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Modify only vendor/humanizer and skills/humanizer below it. Do not modify $userRoot/package.json, codearts_cli.json, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. Ask me to select a provider/model ID if ambiguous.
3. Check $userRoot/vendor/humanizer and $userRoot/skills/humanizer. Stop and report if either exists; never overwrite.
4. In the same PowerShell session run exactly:
   $source = Join-Path $userRoot "vendor\humanizer"
   $target = Join-Path $userRoot "skills\humanizer"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/blader/humanizer.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/humanizer"
   git -C $source checkout --detach "ebf637bdae86b62a7b006c447bb98cfa0ccc979d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "ebf637bdae86b62a7b006c447bb98cfa0ccc979d") { throw "Unexpected commit." }
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   Copy-Item -LiteralPath (Join-Path $source "SKILL.md") -Destination (Join-Path $target "SKILL.md")
5. From a directory with no same-named project skill, run codearts debug skill. Confirm humanizer resolves under the current user's .codeartsdoer/skills/humanizer/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly from the same directory:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name humanizer. Do not use any other tool. Use embedded mode and output only the final rewrite of this sentence, preserving its launch year: Launched in 2024, this groundbreaking platform stands as a pivotal testament to innovation and promises a bright future."
7. Passing criteria: JSON must contain exactly one completed `humanizer` skill call. The final result must preserve `2024` and remove `groundbreaking`, `pivotal`, `testament`, and `bright future`.
8. Removal may include only $userRoot/skills/humanizer and $userRoot/vendor/humanizer. Report the tool event and result; do not claim success after any failure.
```

## Install manually on Windows

### Prerequisites

Install CodeArts CLI and Git, then confirm the intended model:

```powershell
codearts --version
git --version
codearts models
```

### Project scope

Run from the target project root:

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\humanizer"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\humanizer"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/blader/humanizer.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/humanizer"
git -C $source checkout --detach "ebf637bdae86b62a7b006c447bb98cfa0ccc979d"
if ((git -C $source rev-parse HEAD).Trim() -ne "ebf637bdae86b62a7b006c447bb98cfa0ccc979d") { throw "Unexpected commit." }
New-Item -ItemType Directory -Path $target -Force | Out-Null
Copy-Item -LiteralPath (Join-Path $source "SKILL.md") -Destination (Join-Path $target "SKILL.md")
```

### User scope

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\humanizer"
$target = Join-Path $userRoot "skills\humanizer"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/blader/humanizer.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/humanizer"
git -C $source checkout --detach "ebf637bdae86b62a7b006c447bb98cfa0ccc979d"
if ((git -C $source rev-parse HEAD).Trim() -ne "ebf637bdae86b62a7b006c447bb98cfa0ccc979d") { throw "Unexpected commit." }
New-Item -ItemType Directory -Path $target -Force | Out-Null
Copy-Item -LiteralPath (Join-Path $source "SKILL.md") -Destination (Join-Path $target "SKILL.md")
```

Neither scope changes CodeArts model configuration or executes third-party install scripts.

## CodeArts configuration

This skill does not require changes to `codearts_cli.json`. If CodeArts CLI is not installed, follow the [official installation guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html), then list the available models:

```powershell
codearts models
```

Replace `mimo/mimo-v2.5` in later commands with an actual `provider/model` ID from that list. For a custom model, follow the [official configuration example](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html). Keep API keys only in local configuration or environment variables, never in this project.

In this custom-provider test, CodeArts CLI 26.8.1 still required `CODEARTS_CLI_AK` and `CODEARTS_CLI_SK` to exist in the process. Non-secret placeholders passed that preflight while the Provider's own API key authenticated the model request. This is observed behavior, not a compatibility guarantee. Use valid credentials as described in the [official AK/SK guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html) for Huawei Cloud-hosted models.

## Verify

Check the resolved source path first:

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills | Where-Object { $_.name -eq "humanizer" } | Select-Object name, location
```

Then perform the real call, replacing the model ID if needed:

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name humanizer. Do not use any other tool. Use embedded mode and output only the final rewrite of this sentence, preserving its launch year: Launched in 2024, this groundbreaking platform stands as a pivotal testament to innovation and promises a bright future."
```

JSON must contain exactly one completed `humanizer` skill call. The final result must preserve `2024` and remove `groundbreaking`, `pivotal`, `testament`, and `bright future`. A plausible final answer without a completed `skill` event is not a pass.

## Use

```text
Call the skill tool with name humanizer, then rewrite this draft in embedded mode without changing facts.
```

## Update and remove

Review a new commit before updating. Remove the old skill and dedicated `vendor/humanizer` in the selected scope, reinstall the new pinned commit, and repeat verification. Do not treat floating `main` as a verified version.

Project removal targets only:

- `.codeartsdoer/skills/humanizer`
- `.codeartsdoer/vendor/humanizer`

User removal targets only:

- `~/.codeartsdoer/skills/humanizer`
- `~/.codeartsdoer/vendor/humanizer`

Do not delete the CodeArts user-root `package.json`, `codearts_cli.json`, or unrelated skills.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Adapter Required** |
| Upstream | [blader/humanizer](https://github.com/blader/humanizer) |
| Pinned source | [ebf637b](https://github.com/blader/humanizer/commit/ebf637bdae86b62a7b006c447bb98cfa0ccc979d) |
| Verified skill | `humanizer` |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model | `mimo/mimo-v2.5` |
| Verified scopes | Project and user |
| Last verified | 2026-08-19 |

The installation was reproduced in two fresh projects and then verified at user scope from a directory without project configuration. Real CodeArts sessions called `humanizer` and completed the representative task above. All three scopes were rolled back, after which the skill disappeared from discovery.

On a default Windows Git checkout, upstream `skills/humanizer/SKILL.md` becomes a plain file containing only `../../SKILL.md`. This guide therefore copies the repository-root `SKILL.md` into CodeArts' native directory and records `Adapter Required`. Whole-file write-back mode and repository scripts were not tested.

## Security notes

- Copy the repository-root `SKILL.md` from the pinned commit; do not install the broken Windows symlink or execute unrelated repository code.
- Review `SKILL.md` and resources copied with it before installation.
- Skill instructions influence agent behavior; review them before use in sensitive repositories.
- This process does not require `--auto` or read/write credentials.

## Evidence and sources

- [2026-08-19 batch verification](../../research/2026-08-19.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned skill](https://github.com/blader/humanizer/blob/ebf637bdae86b62a7b006c447bb98cfa0ccc979d/SKILL.md)
- [Upstream repository](https://github.com/blader/humanizer)
