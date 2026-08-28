# Use to-tickets with CodeArts CLI

[中文](README.md)

Splits an approved spec into tracer-bullet vertical tickets with blocking edges in a local tracker.

## Choose the installation scope

| Scope | Target | Use it for |
| --- | --- | --- |
| Project | `<project>/.codeartsdoer/skills` | Team-shared, repository-pinned use; recommended by default. |
| User | `~/.codeartsdoer/skills` | Reuse across projects. |

Project Skills override same-named user Skills. Stop on any collision in `to-tickets`.

## Let the Agent install it

### Project prompt

```text
Install and verify project-scoped Matt Pocock to-tickets at commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76.
1. Modify only .codeartsdoer/skills in the current project. Do not change ~/.codeartsdoer, codearts_cli.json, package.json, or credentials.
2. From the project root run exactly: $source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"; git clone --filter=blob:none https://github.com/mattpocock/skills.git $source; git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76; if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }. Do not run npm or upstream scripts.
3. Run exactly: $target = Join-Path (Get-Location) ".codeartsdoer\skills"; New-Item -ItemType Directory -Force -Path $target | Out-Null; $names = @("to-tickets"); if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }; then run: Copy-Item -LiteralPath (Join-Path $source "skills\engineering\to-tickets") -Destination (Join-Path $target "to-tickets") -Recurse. This copies skills/engineering/to-tickets -> .codeartsdoer/skills/to-tickets.
4. In the isolated work directory for this scope, run this complete fixture exactly, before CodeArts:
New-Item -ItemType Directory -Force docs/agents,specs | Out-Null
Set-Content docs/agents/issue-tracker.md "# Local Markdown tracker. Write one ticket per file under .scratch/<feature>/issues and never call a network API."
Set-Content specs/backup.md "# Local backup spec. Deliver dry-run backup, retain the latest five snapshots, and restore the latest snapshot. Each slice must be independently verifiable."
5. Set $env:CODEARTS_CLI_AK="local-placeholder" and $env:CODEARTS_CLI_SK="local-placeholder" only in this PowerShell process, then run codearts models. If several external provider/model IDs are available, ask me to choose. Never persist placeholders.
6. Run codearts debug skill and require every location to resolve under this project.
7. Run exactly: codearts run --auto -m <external-model-id> --format json "Explicitly use the to-tickets skill with specs/backup.md. The maintainer pre-approves exactly three tracer-bullet tickets: 01 dry-run backup (no blockers), 02 snapshot retention (blocked by 01), 03 restore latest snapshot (blocked by 02). Granularity and blocking edges are approved in this message, so publish now to .scratch/local-backup/issues/ as one Markdown file per ticket using the skill template. Do not access any external tracker."
8. Pass only with completed Skill "to-tickets" events and exactly three ready-for-agent files exist under .scratch/local-backup/issues with blocking chain 01→02→03.
9. Report exact files and events. Removal must delete .codeartsdoer/skills/to-tickets; if the source checkout was created by this install and is not shared, also delete the exact .tmp/mattpocock-skills-6654f6b. Preserve the skills and .tmp parents, other Skills, user files, codearts_cli.json, package.json, and credentials.
```

### User prompt

```text
Install and verify user-scoped Matt Pocock to-tickets at commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76 for the current Windows user.
1. Use $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer" as the only install root. Do not modify any project .codeartsdoer, $userRoot/package.json, codearts_cli.json, or credentials.
2. From an empty directory without project Skills, run exactly: $source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"; git clone --filter=blob:none https://github.com/mattpocock/skills.git $source; git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76; if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }. Do not run npm or upstream scripts.
3. Run exactly: $target = Join-Path $userRoot "skills"; New-Item -ItemType Directory -Force -Path $target | Out-Null; $names = @("to-tickets"); if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }; then run: Copy-Item -LiteralPath (Join-Path $source "skills\engineering\to-tickets") -Destination (Join-Path $target "to-tickets") -Recurse. This copies skills/engineering/to-tickets -> $userRoot/skills/to-tickets.
4. In that isolated directory with no project override, run this complete fixture exactly, before CodeArts:
New-Item -ItemType Directory -Force docs/agents,specs | Out-Null
Set-Content docs/agents/issue-tracker.md "# Local Markdown tracker. Write one ticket per file under .scratch/<feature>/issues and never call a network API."
Set-Content specs/backup.md "# Local backup spec. Deliver dry-run backup, retain the latest five snapshots, and restore the latest snapshot. Each slice must be independently verifiable."
5. Run exactly: $env:CODEARTS_CLI_AK="local-placeholder"; $env:CODEARTS_CLI_SK="local-placeholder"; codearts models. If several external provider/model IDs are available, ask me to choose; never persist placeholders.
6. Run codearts debug skill from the empty directory and require user-root locations.
7. Run exactly: codearts run --auto -m <external-model-id> --format json "Explicitly use the to-tickets skill with specs/backup.md. The maintainer pre-approves exactly three tracer-bullet tickets: 01 dry-run backup (no blockers), 02 snapshot retention (blocked by 01), 03 restore latest snapshot (blocked by 02). Granularity and blocking edges are approved in this message, so publish now to .scratch/local-backup/issues/ as one Markdown file per ticket using the skill template. Do not access any external tracker."
8. Pass only with completed Skill "to-tickets" and exactly three ready-for-agent files exist under .scratch/local-backup/issues with blocking chain 01→02→03.
9. Removal must delete $userRoot/skills/to-tickets; if the source checkout was created here and is not shared, also delete the exact isolated .tmp/mattpocock-skills-6654f6b. Preserve the skills and .tmp parents, package.json, codearts_cli.json, unrelated Skills, and credentials.
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
$names = @("to-tickets")
if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }
Copy-Item -LiteralPath (Join-Path $source "skills\engineering\to-tickets") -Destination (Join-Path $target "to-tickets") -Recurse
```

The pinned source is content-only: no dependencies or lifecycle scripts.

Before verification, create the complete fixture from the Agent prompt in an isolated directory:

```powershell
New-Item -ItemType Directory -Force docs/agents,specs | Out-Null
Set-Content docs/agents/issue-tracker.md "# Local Markdown tracker. Write one ticket per file under .scratch/<feature>/issues and never call a network API."
Set-Content specs/backup.md "# Local backup spec. Deliver dry-run backup, retain the latest five snapshots, and restore the latest snapshot. Each slice must be independently verifiable."
```

## CodeArts model and environment

Use a configured external model; this run used `mimo/mimo-v2.5`:

```powershell
$env:CODEARTS_CLI_AK = "local-placeholder"
$env:CODEARTS_CLI_SK = "local-placeholder"
codearts debug skill
codearts run --auto -m "mimo/mimo-v2.5" --format json "Explicitly use the to-tickets skill with specs/backup.md. The maintainer pre-approves exactly three tracer-bullet tickets: 01 dry-run backup (no blockers), 02 snapshot retention (blocked by 01), 03 restore latest snapshot (blocked by 02). Granularity and blocking edges are approved in this message, so publish now to .scratch/local-backup/issues/ as one Markdown file per ticket using the skill template. Do not access any external tracker."
Remove-Item Env:CODEARTS_CLI_AK,Env:CODEARTS_CLI_SK -ErrorAction SilentlyContinue
```

The placeholders only satisfy the CLI's local environment check; model authentication comes from the selected external-model configuration. Never persist them or place real AK/SK values in command history.

## Verification and success criteria

Require the expected locations, completed Skill "to-tickets", and exactly three ready-for-agent files exist under .scratch/local-backup/issues with blocking chain 01→02→03. A plausible answer without completed `skill` events is not a pass.

## Usage

```text
Explicitly use the to-tickets skill with specs/backup.md. The maintainer pre-approves exactly three tracer-bullet tickets: 01 dry-run backup (no blockers), 02 snapshot retention (blocked by 01), 03 restore latest snapshot (blocked by 02). Granularity and blocking edges are approved in this message, so publish now to .scratch/local-backup/issues/ as one Markdown file per ticket using the skill template. Do not access any external tracker.
```

## Update

Audit a new tag/commit, then repeat discovery, invocation, second-project reproduction, and rollback in isolation before replacing the pinned directories. Do not track `main` directly.

## Uninstall

Delete only the `to-tickets` directories in the chosen scope. Preserve the parent, unrelated Skills, `package.json`, and `codearts_cli.json`. Resolve exact absolute paths first, then rerun discovery.

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

The run covered a pre-approved three-ticket breakdown and the local Markdown tracker only. Interactive iteration, wide refactors, and GitHub/Linear native blocking were not tested. CodeArts used completed Bash writes after `write` rejection.

## Security

The pinned source is copied as content only; still audit every target directory before installation. Placeholder AK/SK values are not credentials and must remain process-local. Project scope affects collaborators; user scope affects all sessions not shadowed by a same-named project Skill.

## Evidence and sources

- [2026-08-28 verification](../../research/2026-08-28.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned upstream source](https://github.com/mattpocock/skills/tree/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/to-tickets)
