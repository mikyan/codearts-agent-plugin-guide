# Anthropic Frontend Design on CodeArts CLI

[简体中文](README.md)

Install the verified `frontend-design` skill for subject-specific color, typography, layout, and visual-signature guidance.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Repository-pinned versions, team sharing, or one project. Recommended by default. |
| User | `~/.codeartsdoer` | Repeated use across projects for the current user. |

A same-named project skill takes priority over a user skill. Do not install the same version in both scopes unless the project intentionally overrides the user copy.

## Install with an agent

### Project installation prompt

```text
Install and verify the frontend-design skill from anthropics/skills for CodeArts CLI in the current project, pinned to commit 0a64e398ec6bb34a494f0c347e8ccae53a862f8e.

Follow exactly:
1. Modify only this project's .codeartsdoer. Do not modify ~/.codeartsdoer, credentials, or global software.
2. Run codearts --version, git --version, and codearts models. Ask me to select a provider/model ID if ambiguous.
3. Check .codeartsdoer/vendor/anthropic-skills and .codeartsdoer/skills/frontend-design. Stop and report if either exists; never overwrite.
4. From the project root in PowerShell run exactly:
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\anthropic-skills"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\frontend-design"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/anthropics/skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/frontend-design"
   git -C $source checkout --detach "0a64e398ec6bb34a494f0c347e8ccae53a862f8e"
   if ((git -C $source rev-parse HEAD).Trim() -ne "0a64e398ec6bb34a494f0c347e8ccae53a862f8e") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\frontend-design") -Destination $target -Recurse
5. Run codearts debug skill. Confirm frontend-design resolves under this project's .codeartsdoer/skills/frontend-design/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name frontend-design. Do not use any other tool. Apply the skill to a landing page for a tea-brewing timer aimed at careful home brewers. Output exactly four labeled lines: Color, Type, Layout, Signature. The Color line must contain at least four six-digit hex colors."
7. Passing criteria: JSON must contain exactly one completed `frontend-design` skill call. The final result must contain `Color`, `Type`, `Layout`, and `Signature` lines plus at least four six-digit hex colors.
8. Report the commit, changed paths, tool event, final result, and removal list. If any step fails, stop and do not claim success.
```

### User installation prompt

```text
Install and verify the user-scoped frontend-design skill from anthropics/skills for the current Windows user, pinned to commit 0a64e398ec6bb34a494f0c347e8ccae53a862f8e.

Follow exactly:
1. In PowerShell run $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Modify only vendor/anthropic-skills and skills/frontend-design below it. Do not modify $userRoot/package.json, codearts_cli.json, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. Ask me to select a provider/model ID if ambiguous.
3. Check $userRoot/vendor/anthropic-skills and $userRoot/skills/frontend-design. Stop and report if either exists; never overwrite.
4. In the same PowerShell session run exactly:
   $source = Join-Path $userRoot "vendor\anthropic-skills"
   $target = Join-Path $userRoot "skills\frontend-design"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/anthropics/skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/frontend-design"
   git -C $source checkout --detach "0a64e398ec6bb34a494f0c347e8ccae53a862f8e"
   if ((git -C $source rev-parse HEAD).Trim() -ne "0a64e398ec6bb34a494f0c347e8ccae53a862f8e") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\frontend-design") -Destination $target -Recurse
5. From a directory with no same-named project skill, run codearts debug skill. Confirm frontend-design resolves under the current user's .codeartsdoer/skills/frontend-design/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly from the same directory:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name frontend-design. Do not use any other tool. Apply the skill to a landing page for a tea-brewing timer aimed at careful home brewers. Output exactly four labeled lines: Color, Type, Layout, Signature. The Color line must contain at least four six-digit hex colors."
7. Passing criteria: JSON must contain exactly one completed `frontend-design` skill call. The final result must contain `Color`, `Type`, `Layout`, and `Signature` lines plus at least four six-digit hex colors.
8. Removal may include only $userRoot/skills/frontend-design and $userRoot/vendor/anthropic-skills. Report the tool event and result; do not claim success after any failure.
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
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\anthropic-skills"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\frontend-design"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/anthropics/skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/frontend-design"
git -C $source checkout --detach "0a64e398ec6bb34a494f0c347e8ccae53a862f8e"
if ((git -C $source rev-parse HEAD).Trim() -ne "0a64e398ec6bb34a494f0c347e8ccae53a862f8e") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\frontend-design") -Destination $target -Recurse
```

### User scope

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\anthropic-skills"
$target = Join-Path $userRoot "skills\frontend-design"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/anthropics/skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/frontend-design"
git -C $source checkout --detach "0a64e398ec6bb34a494f0c347e8ccae53a862f8e"
if ((git -C $source rev-parse HEAD).Trim() -ne "0a64e398ec6bb34a494f0c347e8ccae53a862f8e") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\frontend-design") -Destination $target -Recurse
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
$skills | Where-Object { $_.name -eq "frontend-design" } | Select-Object name, location
```

Then perform the real call, replacing the model ID if needed:

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name frontend-design. Do not use any other tool. Apply the skill to a landing page for a tea-brewing timer aimed at careful home brewers. Output exactly four labeled lines: Color, Type, Layout, Signature. The Color line must contain at least four six-digit hex colors."
```

JSON must contain exactly one completed `frontend-design` skill call. The final result must contain `Color`, `Type`, `Layout`, and `Signature` lines plus at least four six-digit hex colors. A plausible final answer without a completed `skill` event is not a pass.

## Use

```text
Call the skill tool with name frontend-design, then design a distinctive landing-page direction for this product before writing code.
```

## Update and remove

Review a new commit before updating. Remove the old skill and dedicated `vendor/anthropic-skills` in the selected scope, reinstall the new pinned commit, and repeat verification. Do not treat floating `main` as a verified version.

Project removal targets only:

- `.codeartsdoer/skills/frontend-design`
- `.codeartsdoer/vendor/anthropic-skills`

User removal targets only:

- `~/.codeartsdoer/skills/frontend-design`
- `~/.codeartsdoer/vendor/anthropic-skills`

Do not delete the CodeArts user-root `package.json`, `codearts_cli.json`, or unrelated skills.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Works** |
| Upstream | [anthropics/skills](https://github.com/anthropics/skills) |
| Pinned source | [0a64e39](https://github.com/anthropics/skills/commit/0a64e398ec6bb34a494f0c347e8ccae53a862f8e) |
| Verified skill | `frontend-design` |
| License | Apache-2.0 |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model | `mimo/mimo-v2.5` |
| Verified scopes | Project and user |
| Last verified | 2026-08-19 |

The installation was reproduced in two fresh projects and then verified at user scope from a directory without project configuration. Real CodeArts sessions called `frontend-design` and completed the representative task above. All three scopes were rolled back, after which the skill disappeared from discovery.

Only `frontend-design` was verified. Other Anthropic skills, actual page-file generation, and browser visual QA were outside this run.

## Security notes

- Copy only the selected skill directory from the pinned commit; do not execute unrelated repository code.
- Review `SKILL.md` and resources copied with it before installation.
- Skill instructions influence agent behavior; review them before use in sensitive repositories.
- This process does not require `--auto` or read/write credentials.

## Evidence and sources

- [2026-08-19 batch verification](../../research/2026-08-19.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned skill](https://github.com/anthropics/skills/blob/0a64e398ec6bb34a494f0c347e8ccae53a862f8e/skills/frontend-design/SKILL.md)
- [Upstream repository](https://github.com/anthropics/skills)
