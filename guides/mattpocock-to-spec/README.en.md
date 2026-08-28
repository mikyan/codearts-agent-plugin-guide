# Use to-spec with CodeArts CLI

[中文](README.md)

Synthesizes approved requirements into a complete spec and publishes it to a configured local Markdown tracker.

## Choose the installation scope

| Scope | Target | Use it for |
| --- | --- | --- |
| Project | `<project>/.codeartsdoer/skills` | Team-shared, repository-pinned use; recommended by default. |
| User | `~/.codeartsdoer/skills` | Reuse across projects. |

Project Skills override same-named user Skills. Stop on any collision in `to-spec`.

## Let the Agent install it

### Project prompt

```text
Install and verify project-scoped Matt Pocock to-spec at commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76.
1. Modify only .codeartsdoer/skills in the current project. Do not change ~/.codeartsdoer, codearts_cli.json, package.json, or credentials.
2. From the project root run exactly: $source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"; git clone --filter=blob:none https://github.com/mattpocock/skills.git $source; git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76; if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }. Do not run npm or upstream scripts.
3. Run exactly: $target = Join-Path (Get-Location) ".codeartsdoer\skills"; New-Item -ItemType Directory -Force -Path $target | Out-Null; $names = @("to-spec"); if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }; then run: Copy-Item -LiteralPath (Join-Path $source "skills\engineering\to-spec") -Destination (Join-Path $target "to-spec") -Recurse. This copies skills/engineering/to-spec -> .codeartsdoer/skills/to-spec.
4. In the isolated work directory for this scope, run this complete fixture exactly, before CodeArts:
New-Item -ItemType Directory -Force docs/agents,issues,src | Out-Null
Set-Content docs/agents/issue-tracker.md "# Local Markdown tracker. Use local files under issues only; never call a network API."
Set-Content APPROVED.md "# Approved local backup. Build local-only backup(source,destination), dry-run, keep five snapshots, restore latest. Cloud sync and encryption are out of scope."
Set-Content CONTEXT.md "# Domain Context. Snapshot: one immutable local backup capture."
Set-Content src/backup.js "export function backup(source, destination) { throw new Error('not implemented'); }"
5. Set $env:CODEARTS_CLI_AK="local-placeholder" and $env:CODEARTS_CLI_SK="local-placeholder" only in this PowerShell process, then run codearts models. If several external provider/model IDs are available, ask me to choose. Never persist placeholders.
6. Run codearts debug skill and require every location to resolve under this project.
7. Run exactly: codearts run --auto -m <external-model-id> --format json "Explicitly use the to-spec skill. The already-approved requirement is in APPROVED.md, the domain glossary is CONTEXT.md, the only test seam is the public backup(source,destination) interface, and that seam is already approved in this message. The configured tracker is local Markdown. Do not interview. Synthesize and publish the spec to issues/001-local-backup-spec.md, include every template section, and include Status: ready-for-agent. Do not access any external tracker."
8. Pass only with completed Skill "to-spec" events and issues/001-local-backup-spec.md exists with all seven template sections and ready-for-agent.
9. Report exact files and events. Removal must delete .codeartsdoer/skills/to-spec; if the source checkout was created by this install and is not shared, also delete the exact .tmp/mattpocock-skills-6654f6b. Preserve the skills and .tmp parents, other Skills, user files, codearts_cli.json, package.json, and credentials.
```

### User prompt

```text
Install and verify user-scoped Matt Pocock to-spec at commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76 for the current Windows user.
1. Use $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer" as the only install root. Do not modify any project .codeartsdoer, $userRoot/package.json, codearts_cli.json, or credentials.
2. From an empty directory without project Skills, run exactly: $source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"; git clone --filter=blob:none https://github.com/mattpocock/skills.git $source; git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76; if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }. Do not run npm or upstream scripts.
3. Run exactly: $target = Join-Path $userRoot "skills"; New-Item -ItemType Directory -Force -Path $target | Out-Null; $names = @("to-spec"); if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }; then run: Copy-Item -LiteralPath (Join-Path $source "skills\engineering\to-spec") -Destination (Join-Path $target "to-spec") -Recurse. This copies skills/engineering/to-spec -> $userRoot/skills/to-spec.
4. In that isolated directory with no project override, run this complete fixture exactly, before CodeArts:
New-Item -ItemType Directory -Force docs/agents,issues,src | Out-Null
Set-Content docs/agents/issue-tracker.md "# Local Markdown tracker. Use local files under issues only; never call a network API."
Set-Content APPROVED.md "# Approved local backup. Build local-only backup(source,destination), dry-run, keep five snapshots, restore latest. Cloud sync and encryption are out of scope."
Set-Content CONTEXT.md "# Domain Context. Snapshot: one immutable local backup capture."
Set-Content src/backup.js "export function backup(source, destination) { throw new Error('not implemented'); }"
5. Run exactly: $env:CODEARTS_CLI_AK="local-placeholder"; $env:CODEARTS_CLI_SK="local-placeholder"; codearts models. If several external provider/model IDs are available, ask me to choose; never persist placeholders.
6. Run codearts debug skill from the empty directory and require user-root locations.
7. Run exactly: codearts run --auto -m <external-model-id> --format json "Explicitly use the to-spec skill. The already-approved requirement is in APPROVED.md, the domain glossary is CONTEXT.md, the only test seam is the public backup(source,destination) interface, and that seam is already approved in this message. The configured tracker is local Markdown. Do not interview. Synthesize and publish the spec to issues/001-local-backup-spec.md, include every template section, and include Status: ready-for-agent. Do not access any external tracker."
8. Pass only with completed Skill "to-spec" and issues/001-local-backup-spec.md exists with all seven template sections and ready-for-agent.
9. Removal must delete $userRoot/skills/to-spec; if the source checkout was created here and is not shared, also delete the exact isolated .tmp/mattpocock-skills-6654f6b. Preserve the skills and .tmp parents, package.json, codearts_cli.json, unrelated Skills, and credentials.
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
$names = @("to-spec")
if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }
Copy-Item -LiteralPath (Join-Path $source "skills\engineering\to-spec") -Destination (Join-Path $target "to-spec") -Recurse
```

The pinned source is content-only: no dependencies or lifecycle scripts.

Before verification, create the complete fixture from the Agent prompt in an isolated directory:

```powershell
New-Item -ItemType Directory -Force docs/agents,issues,src | Out-Null
Set-Content docs/agents/issue-tracker.md "# Local Markdown tracker. Use local files under issues only; never call a network API."
Set-Content APPROVED.md "# Approved local backup. Build local-only backup(source,destination), dry-run, keep five snapshots, restore latest. Cloud sync and encryption are out of scope."
Set-Content CONTEXT.md "# Domain Context. Snapshot: one immutable local backup capture."
Set-Content src/backup.js "export function backup(source, destination) { throw new Error('not implemented'); }"
```

## CodeArts model and environment

Use a configured external model; this run used `mimo/mimo-v2.5`:

```powershell
$env:CODEARTS_CLI_AK = "local-placeholder"
$env:CODEARTS_CLI_SK = "local-placeholder"
codearts debug skill
codearts run --auto -m "mimo/mimo-v2.5" --format json "Explicitly use the to-spec skill. The already-approved requirement is in APPROVED.md, the domain glossary is CONTEXT.md, the only test seam is the public backup(source,destination) interface, and that seam is already approved in this message. The configured tracker is local Markdown. Do not interview. Synthesize and publish the spec to issues/001-local-backup-spec.md, include every template section, and include Status: ready-for-agent. Do not access any external tracker."
Remove-Item Env:CODEARTS_CLI_AK,Env:CODEARTS_CLI_SK -ErrorAction SilentlyContinue
```

The placeholders only satisfy the CLI's local environment check; model authentication comes from the selected external-model configuration. Never persist them or place real AK/SK values in command history.

## Verification and success criteria

Require the expected locations, completed Skill "to-spec", and issues/001-local-backup-spec.md exists with all seven template sections and ready-for-agent. A plausible answer without completed `skill` events is not a pass.

## Usage

```text
Explicitly use the to-spec skill. The already-approved requirement is in APPROVED.md, the domain glossary is CONTEXT.md, the only test seam is the public backup(source,destination) interface, and that seam is already approved in this message. The configured tracker is local Markdown. Do not interview. Synthesize and publish the spec to issues/001-local-backup-spec.md, include every template section, and include Status: ready-for-agent. Do not access any external tracker.
```

## Update

Audit a new tag/commit, then repeat discovery, invocation, second-project reproduction, and rollback in isolation before replacing the pinned directories. Do not track `main` directly.

## Uninstall

Delete only the `to-spec` directories in the chosen scope. Preserve the parent, unrelated Skills, `package.json`, and `codearts_cli.json`. Resolve exact absolute paths first, then rerun discovery.

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

Only the local Markdown tracker was tested, not GitHub or Linear. Supply approved requirements, the testing seam, domain vocabulary, and tracker convention first; this flow should not re-interview. All three runs completed through Bash writes after `write` rejection.

## Security

The pinned source is copied as content only; still audit every target directory before installation. Placeholder AK/SK values are not credentials and must remain process-local. Project scope affects collaborators; user scope affects all sessions not shadowed by a same-named project Skill.

## Evidence and sources

- [2026-08-28 verification](../../research/2026-08-28.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned upstream source](https://github.com/mattpocock/skills/tree/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/to-spec)
