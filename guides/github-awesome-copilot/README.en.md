# GitHub Commit Message Storyteller on CodeArts CLI

[简体中文](README.md)

Install the verified `commit-message-storyteller` skill from GitHub Awesome Copilot to produce Conventional Commit messages that explain why a change was made.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Repository-pinned versions, team sharing, or one project. Recommended by default. |
| User | `~/.codeartsdoer` | Repeated use across projects for the current user. |

A same-named project skill takes priority over a user skill. Do not install the same version in both scopes unless the project intentionally overrides the user copy.

## Install with an agent

### Project installation prompt

```text
Install and verify the commit-message-storyteller skill from github/awesome-copilot for CodeArts CLI in the current project, pinned to commit 318066d2213b510e89b500ed0d53506c54093ddc.

Follow exactly:
1. Modify only this project's .codeartsdoer. Do not modify ~/.codeartsdoer, credentials, or global software.
2. Run codearts --version, git --version, and codearts models. Ask me to select a provider/model ID if ambiguous.
3. Check .codeartsdoer/vendor/github-awesome-copilot and .codeartsdoer/skills/commit-message-storyteller. Stop and report if either exists; never overwrite.
4. From the project root in PowerShell run exactly:
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\github-awesome-copilot"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\commit-message-storyteller"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/github/awesome-copilot.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/commit-message-storyteller"
   git -C $source checkout --detach "318066d2213b510e89b500ed0d53506c54093ddc"
   if ((git -C $source rev-parse HEAD).Trim() -ne "318066d2213b510e89b500ed0d53506c54093ddc") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\commit-message-storyteller") -Destination $target -Recurse
5. Run codearts debug skill. Confirm commit-message-storyteller resolves under this project's .codeartsdoer/skills/commit-message-storyteller/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name commit-message-storyteller. Do not use any other tool. Write one Conventional Commit message for this change: a request cache was recreated for every request, causing repeated work and latency; the cache is now kept at module scope and reused. Output only the commit message."
7. Passing criteria: JSON must contain exactly one completed `commit-message-storyteller` skill call. The subject must use `perf` or `fix`, and the body must explain the cache, repeated work or latency, and reuse. A code fence around the commit message is acceptable.
8. Report the commit, changed paths, tool event, final result, and removal list. If any step fails, stop and do not claim success.
```

### User installation prompt

```text
Install and verify the user-scoped commit-message-storyteller skill from github/awesome-copilot for the current Windows user, pinned to commit 318066d2213b510e89b500ed0d53506c54093ddc.

Follow exactly:
1. In PowerShell run $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Modify only vendor/github-awesome-copilot and skills/commit-message-storyteller below it. Do not modify $userRoot/package.json, codearts_cli.json, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. Ask me to select a provider/model ID if ambiguous.
3. Check $userRoot/vendor/github-awesome-copilot and $userRoot/skills/commit-message-storyteller. Stop and report if either exists; never overwrite.
4. In the same PowerShell session run exactly:
   $source = Join-Path $userRoot "vendor\github-awesome-copilot"
   $target = Join-Path $userRoot "skills\commit-message-storyteller"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/github/awesome-copilot.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/commit-message-storyteller"
   git -C $source checkout --detach "318066d2213b510e89b500ed0d53506c54093ddc"
   if ((git -C $source rev-parse HEAD).Trim() -ne "318066d2213b510e89b500ed0d53506c54093ddc") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\commit-message-storyteller") -Destination $target -Recurse
5. From a directory with no same-named project skill, run codearts debug skill. Confirm commit-message-storyteller resolves under the current user's .codeartsdoer/skills/commit-message-storyteller/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly from the same directory:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name commit-message-storyteller. Do not use any other tool. Write one Conventional Commit message for this change: a request cache was recreated for every request, causing repeated work and latency; the cache is now kept at module scope and reused. Output only the commit message."
7. Passing criteria: JSON must contain exactly one completed `commit-message-storyteller` skill call. The subject must use `perf` or `fix`, and the body must explain the cache, repeated work or latency, and reuse. A code fence around the commit message is acceptable.
8. Removal may include only $userRoot/skills/commit-message-storyteller and $userRoot/vendor/github-awesome-copilot. Report the tool event and result; do not claim success after any failure.
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
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\github-awesome-copilot"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\commit-message-storyteller"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/github/awesome-copilot.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/commit-message-storyteller"
git -C $source checkout --detach "318066d2213b510e89b500ed0d53506c54093ddc"
if ((git -C $source rev-parse HEAD).Trim() -ne "318066d2213b510e89b500ed0d53506c54093ddc") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\commit-message-storyteller") -Destination $target -Recurse
```

### User scope

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\github-awesome-copilot"
$target = Join-Path $userRoot "skills\commit-message-storyteller"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/github/awesome-copilot.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/commit-message-storyteller"
git -C $source checkout --detach "318066d2213b510e89b500ed0d53506c54093ddc"
if ((git -C $source rev-parse HEAD).Trim() -ne "318066d2213b510e89b500ed0d53506c54093ddc") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\commit-message-storyteller") -Destination $target -Recurse
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
$skills | Where-Object { $_.name -eq "commit-message-storyteller" } | Select-Object name, location
```

Then perform the real call, replacing the model ID if needed:

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name commit-message-storyteller. Do not use any other tool. Write one Conventional Commit message for this change: a request cache was recreated for every request, causing repeated work and latency; the cache is now kept at module scope and reused. Output only the commit message."
```

JSON must contain exactly one completed `commit-message-storyteller` skill call. The subject must use `perf` or `fix`, and the body must explain the cache, repeated work or latency, and reuse. A code fence around the commit message is acceptable. A plausible final answer without a completed `skill` event is not a pass.

## Use

```text
Call the skill tool with name commit-message-storyteller, inspect the staged diff, and propose a Conventional Commit message without committing.
```

## Update and remove

Review a new commit before updating. Remove the old skill and dedicated `vendor/github-awesome-copilot` in the selected scope, reinstall the new pinned commit, and repeat verification. Do not treat floating `main` as a verified version.

Project removal targets only:

- `.codeartsdoer/skills/commit-message-storyteller`
- `.codeartsdoer/vendor/github-awesome-copilot`

User removal targets only:

- `~/.codeartsdoer/skills/commit-message-storyteller`
- `~/.codeartsdoer/vendor/github-awesome-copilot`

Do not delete the CodeArts user-root `package.json`, `codearts_cli.json`, or unrelated skills.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Works** |
| Upstream | [github/awesome-copilot](https://github.com/github/awesome-copilot) |
| Pinned source | [318066d](https://github.com/github/awesome-copilot/commit/318066d2213b510e89b500ed0d53506c54093ddc) |
| Verified skill | `commit-message-storyteller` |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model | `mimo/mimo-v2.5` |
| Verified scopes | Project and user |
| Last verified | 2026-08-19 |

The installation was reproduced in two fresh projects and then verified at user scope from a directory without project configuration. Real CodeArts sessions called `commit-message-storyteller` and completed the representative task above. All three scopes were rolled back, after which the skill disappeared from discovery.

Only `commit-message-storyteller` was verified. The other 400-plus skills, agents, hooks, MCP definitions, and extensions in Awesome Copilot were not tested.

## Security notes

- Copy only the selected skill directory from the pinned commit; do not execute unrelated repository code.
- Review `SKILL.md` and resources copied with it before installation.
- Skill instructions influence agent behavior; review them before use in sensitive repositories.
- This process does not require `--auto` or read/write credentials.

## Evidence and sources

- [2026-08-19 batch verification](../../research/2026-08-19.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned skill](https://github.com/github/awesome-copilot/blob/318066d2213b510e89b500ed0d53506c54093ddc/skills/commit-message-storyteller/SKILL.md)
- [Upstream repository](https://github.com/github/awesome-copilot)
