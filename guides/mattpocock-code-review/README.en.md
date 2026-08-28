# Use code-review with CodeArts CLI

[中文](README.md)

Reviews a Git diff on separate Standards and Spec axes so neither can hide failures in the other.

## Choose the installation scope

| Scope | Target | Use it for |
| --- | --- | --- |
| Project | `<project>/.codeartsdoer/skills` | Team-shared, repository-pinned use; recommended by default. |
| User | `~/.codeartsdoer/skills` | Reuse across projects. |

Project Skills override same-named user Skills. Stop on any collision in `code-review`.

## Let the Agent install it

### Project prompt

```text
Install and verify project-scoped Matt Pocock code-review at commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76.
1. Modify only .codeartsdoer/skills in the current project. Do not change ~/.codeartsdoer, codearts_cli.json, package.json, or credentials.
2. From the project root run exactly: $source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"; git clone --filter=blob:none https://github.com/mattpocock/skills.git $source; git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76; if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }. Do not run npm or upstream scripts.
3. Run exactly: $target = Join-Path (Get-Location) ".codeartsdoer\skills"; New-Item -ItemType Directory -Force -Path $target | Out-Null; $names = @("code-review"); if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }; then run: Copy-Item -LiteralPath (Join-Path $source "skills\engineering\code-review") -Destination (Join-Path $target "code-review") -Recurse. This copies skills/engineering/code-review -> .codeartsdoer/skills/code-review.
4. In the isolated work directory for this scope, run this complete fixture exactly, before CodeArts:
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
@'
# Standards

Production code must not call console.log.
'@ | Set-Content CONTRIBUTING.md
@'
# Greeting spec

The greet(name) function returns exactly Hello, <name> and produces no logging side effect.
'@ | Set-Content SPEC.md
Set-Content greet.js 'export function greet(name) { return "Hello, " + name; }'
git add .
git commit -m "baseline greeting"
Set-Content greet.js 'export function greet(name) { console.log(name); return "Hi, " + name; }'
git add greet.js
git commit -m "change greeting output"
5. Set $env:CODEARTS_CLI_AK="local-placeholder" and $env:CODEARTS_CLI_SK="local-placeholder" only in this PowerShell process, then run codearts models. If several external provider/model IDs are available, ask me to choose. Never persist placeholders.
6. Run codearts debug skill and require every location to resolve under this project.
7. Run exactly: codearts run -m <external-model-id> --format json "Explicitly use the code-review skill to review HEAD against fixed point HEAD~1. The spec source is SPEC.md and the standards source is CONTRIBUTING.md. Run the required git diff and git log checks. Report the Standards and Spec axes separately, including the documented console.log violation and the greeting mismatch. Do not modify files."
8. Pass only with completed Skill "code-review" events and at least two completed task events and a two-axis report identifying both regressions.
9. Report exact files and events. Removal must delete .codeartsdoer/skills/code-review; if the source checkout was created by this install and is not shared, also delete the exact .tmp/mattpocock-skills-6654f6b. Preserve the skills and .tmp parents, other Skills, user files, codearts_cli.json, package.json, and credentials.
```

### User prompt

```text
Install and verify user-scoped Matt Pocock code-review at commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76 for the current Windows user.
1. Use $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer" as the only install root. Do not modify any project .codeartsdoer, $userRoot/package.json, codearts_cli.json, or credentials.
2. From an empty directory without project Skills, run exactly: $source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"; git clone --filter=blob:none https://github.com/mattpocock/skills.git $source; git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76; if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }. Do not run npm or upstream scripts.
3. Run exactly: $target = Join-Path $userRoot "skills"; New-Item -ItemType Directory -Force -Path $target | Out-Null; $names = @("code-review"); if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }; then run: Copy-Item -LiteralPath (Join-Path $source "skills\engineering\code-review") -Destination (Join-Path $target "code-review") -Recurse. This copies skills/engineering/code-review -> $userRoot/skills/code-review.
4. In that isolated directory with no project override, run this complete fixture exactly, before CodeArts:
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
@'
# Standards

Production code must not call console.log.
'@ | Set-Content CONTRIBUTING.md
@'
# Greeting spec

The greet(name) function returns exactly Hello, <name> and produces no logging side effect.
'@ | Set-Content SPEC.md
Set-Content greet.js 'export function greet(name) { return "Hello, " + name; }'
git add .
git commit -m "baseline greeting"
Set-Content greet.js 'export function greet(name) { console.log(name); return "Hi, " + name; }'
git add greet.js
git commit -m "change greeting output"
5. Run exactly: $env:CODEARTS_CLI_AK="local-placeholder"; $env:CODEARTS_CLI_SK="local-placeholder"; codearts models. If several external provider/model IDs are available, ask me to choose; never persist placeholders.
6. Run codearts debug skill from the empty directory and require user-root locations.
7. Run exactly: codearts run -m <external-model-id> --format json "Explicitly use the code-review skill to review HEAD against fixed point HEAD~1. The spec source is SPEC.md and the standards source is CONTRIBUTING.md. Run the required git diff and git log checks. Report the Standards and Spec axes separately, including the documented console.log violation and the greeting mismatch. Do not modify files."
8. Pass only with completed Skill "code-review" and at least two completed task events and a two-axis report identifying both regressions.
9. Removal must delete $userRoot/skills/code-review; if the source checkout was created here and is not shared, also delete the exact isolated .tmp/mattpocock-skills-6654f6b. Preserve the skills and .tmp parents, package.json, codearts_cli.json, unrelated Skills, and credentials.
```

## Manual installation

Run from the clean directory for the chosen scope:

```powershell
$source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"
git clone --filter=blob:none https://github.com/mattpocock/skills.git $source
git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76
if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }
$projectTarget = Join-Path (Get-Location) ".codeartsdoer\skills"
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$userTarget = Join-Path $userRoot "skills"
$target = $projectTarget # For user scope, change this line to $target = $userTarget
New-Item -ItemType Directory -Force -Path $target | Out-Null
$names = @("code-review")
if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }
Copy-Item -LiteralPath (Join-Path $source "skills\engineering\code-review") -Destination (Join-Path $target "code-review") -Recurse
```

The pinned source is content-only: no dependencies or lifecycle scripts.

Before verification, create the complete fixture from the Agent prompt in an isolated directory:

```powershell
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
@'
# Standards

Production code must not call console.log.
'@ | Set-Content CONTRIBUTING.md
@'
# Greeting spec

The greet(name) function returns exactly Hello, <name> and produces no logging side effect.
'@ | Set-Content SPEC.md
Set-Content greet.js 'export function greet(name) { return "Hello, " + name; }'
git add .
git commit -m "baseline greeting"
Set-Content greet.js 'export function greet(name) { console.log(name); return "Hi, " + name; }'
git add greet.js
git commit -m "change greeting output"
```

## CodeArts model and environment

Use a configured external model; this run used `mimo/mimo-v2.5`:

```powershell
$env:CODEARTS_CLI_AK = "local-placeholder"
$env:CODEARTS_CLI_SK = "local-placeholder"
codearts debug skill
codearts run -m "mimo/mimo-v2.5" --format json "Explicitly use the code-review skill to review HEAD against fixed point HEAD~1. The spec source is SPEC.md and the standards source is CONTRIBUTING.md. Run the required git diff and git log checks. Report the Standards and Spec axes separately, including the documented console.log violation and the greeting mismatch. Do not modify files."
Remove-Item Env:CODEARTS_CLI_AK,Env:CODEARTS_CLI_SK -ErrorAction SilentlyContinue
```

The placeholders only satisfy the CLI's local environment check; model authentication comes from the selected external-model configuration. Never persist them or place real AK/SK values in command history.

## Verification and success criteria

Require the expected locations, completed Skill "code-review", and at least two completed task events and a two-axis report identifying both regressions. A plausible answer without completed `skill` events is not a pass.

## Usage

```text
Explicitly use the code-review skill to review HEAD against fixed point HEAD~1. The spec source is SPEC.md and the standards source is CONTRIBUTING.md. Run the required git diff and git log checks. Report the Standards and Spec axes separately, including the documented console.log violation and the greeting mismatch. Do not modify files.
```

## Update

Audit a new tag/commit, then repeat discovery, invocation, second-project reproduction, and rollback in isolation before replacing the pinned directories. Do not track `main` directly.

## Uninstall

Delete only the `code-review` directories in the chosen scope. Preserve the parent, unrelated Skills, `package.json`, and `codearts_cli.json`. Resolve exact absolute paths first, then rerun discovery.

## Verified version and result

| Item | Value |
| --- | --- |
| Status | **Works** |
| Upstream | [mattpocock/skills](https://github.com/mattpocock/skills) |
| Release / commit | after v1.2.3, `6654f6b60cd9d5be8b54c6fafe44346dabeb3b76` |
| License | MIT |
| CodeArts / OS | CLI 26.8.1 / Windows 11 |
| Model | `mimo/mimo-v2.5` |
| Scopes | project (two isolated projects) and user |
| Date | 2026-08-28 |

## Known limitations

Requires a Git repository, a resolvable fixed point, and explicit standards/spec sources. Each run produced 2–4 completed `task` subagent events and was slower than a single-model review. Remote PRs and real issue trackers were not tested.

## Security

The pinned source is copied as content only; still audit every target directory before installation. Placeholder AK/SK values are not credentials and must remain process-local. Project scope affects collaborators; user scope affects all sessions not shadowed by a same-named project Skill.

## Evidence and sources

- [2026-08-28 verification](../../research/2026-08-28.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned upstream source](https://github.com/mattpocock/skills/tree/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/code-review)
