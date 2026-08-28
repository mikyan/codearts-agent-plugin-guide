# Use resolving-merge-conflicts with CodeArts CLI

[中文](README.md)

Resolves an in-progress Git conflict by intent from both sides, then tests and completes the merge commit.

## Choose the installation scope

| Scope | Target | Use it for |
| --- | --- | --- |
| Project | `<project>/.codeartsdoer/skills` | Team-shared, repository-pinned use; recommended by default. |
| User | `~/.codeartsdoer/skills` | Reuse across projects. |

Project Skills override same-named user Skills. Stop on any collision in `resolving-merge-conflicts`.

## Let the Agent install it

### Project prompt

```text
Install and verify project-scoped Matt Pocock resolving-merge-conflicts at commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76.
1. Modify only .codeartsdoer/skills in the current project. Do not change ~/.codeartsdoer, codearts_cli.json, package.json, or credentials.
2. From the project root run exactly: $source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"; git clone --filter=blob:none https://github.com/mattpocock/skills.git $source; git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76; if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }. Do not run npm or upstream scripts.
3. Run exactly: $target = Join-Path (Get-Location) ".codeartsdoer\skills"; New-Item -ItemType Directory -Force -Path $target | Out-Null; $names = @("resolving-merge-conflicts"); if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }; then run: Copy-Item -LiteralPath (Join-Path $source "skills\engineering\resolving-merge-conflicts") -Destination (Join-Path $target "resolving-merge-conflicts") -Recurse. This copies skills/engineering/resolving-merge-conflicts -> .codeartsdoer/skills/resolving-merge-conflicts.
4. In the isolated work directory for this scope, run this complete fixture exactly, before CodeArts:
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
Set-Content package.json '{"type":"module"}'
Set-Content format.js 'export function format(item) { return item.name; }'
git add .
git commit -m "baseline formatter"
git checkout -b feature-uppercase
Set-Content format.js 'export function format(item) { return item.name.toUpperCase(); }'
Set-Content uppercase.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('uppercases name', () => assert.match(format({name:'box',status:'ready'}), /^BOX/));"
git add .
git commit -m "preserve uppercase name intent"
git checkout main
Set-Content format.js "export function format(item) { return item.name + ':' + item.status; }"
Set-Content status.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('includes status suffix', () => assert.match(format({name:'box',status:'ready'}), /:ready$/));"
git add .
git commit -m "preserve status suffix intent"
git merge feature-uppercase
if (-not (Test-Path .git\MERGE_HEAD)) { throw "Expected conflict" }
5. Set $env:CODEARTS_CLI_AK="local-placeholder" and $env:CODEARTS_CLI_SK="local-placeholder" only in this PowerShell process, then run codearts models. If several external provider/model IDs are available, ask me to choose. Never persist placeholders.
6. Run codearts debug skill and require every location to resolve under this project.
7. Run exactly: codearts run --auto -m <external-model-id> --format json "Explicitly use the resolving-merge-conflicts skill. This isolated repository is already in an in-progress merge conflict. Inspect both commits as primary sources, resolve format.js so it preserves uppercase-name and status-suffix intents, run node --test, stage the resolution, and finish the merge with a non-interactive commit. Never abort. Do not add dependencies."
8. Pass only with completed Skill "resolving-merge-conflicts" events and two tests pass, no unmerged files or MERGE_HEAD remain, HEAD has two parents, and format.js preserves both intents.
9. Report exact files and events. Removal must delete .codeartsdoer/skills/resolving-merge-conflicts; if the source checkout was created by this install and is not shared, also delete the exact .tmp/mattpocock-skills-6654f6b. Preserve the skills and .tmp parents, other Skills, user files, codearts_cli.json, package.json, and credentials.
```

### User prompt

```text
Install and verify user-scoped Matt Pocock resolving-merge-conflicts at commit 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76 for the current Windows user.
1. Use $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer" as the only install root. Do not modify any project .codeartsdoer, $userRoot/package.json, codearts_cli.json, or credentials.
2. From an empty directory without project Skills, run exactly: $source = Join-Path (Get-Location) ".tmp\mattpocock-skills-6654f6b"; git clone --filter=blob:none https://github.com/mattpocock/skills.git $source; git -C $source checkout 6654f6b60cd9d5be8b54c6fafe44346dabeb3b76; if ((git -C $source rev-parse HEAD).Trim() -ne "6654f6b60cd9d5be8b54c6fafe44346dabeb3b76") { throw "Commit mismatch" }. Do not run npm or upstream scripts.
3. Run exactly: $target = Join-Path $userRoot "skills"; New-Item -ItemType Directory -Force -Path $target | Out-Null; $names = @("resolving-merge-conflicts"); if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }; then run: Copy-Item -LiteralPath (Join-Path $source "skills\engineering\resolving-merge-conflicts") -Destination (Join-Path $target "resolving-merge-conflicts") -Recurse. This copies skills/engineering/resolving-merge-conflicts -> $userRoot/skills/resolving-merge-conflicts.
4. In that isolated directory with no project override, run this complete fixture exactly, before CodeArts:
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
Set-Content package.json '{"type":"module"}'
Set-Content format.js 'export function format(item) { return item.name; }'
git add .
git commit -m "baseline formatter"
git checkout -b feature-uppercase
Set-Content format.js 'export function format(item) { return item.name.toUpperCase(); }'
Set-Content uppercase.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('uppercases name', () => assert.match(format({name:'box',status:'ready'}), /^BOX/));"
git add .
git commit -m "preserve uppercase name intent"
git checkout main
Set-Content format.js "export function format(item) { return item.name + ':' + item.status; }"
Set-Content status.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('includes status suffix', () => assert.match(format({name:'box',status:'ready'}), /:ready$/));"
git add .
git commit -m "preserve status suffix intent"
git merge feature-uppercase
if (-not (Test-Path .git\MERGE_HEAD)) { throw "Expected conflict" }
5. Run exactly: $env:CODEARTS_CLI_AK="local-placeholder"; $env:CODEARTS_CLI_SK="local-placeholder"; codearts models. If several external provider/model IDs are available, ask me to choose; never persist placeholders.
6. Run codearts debug skill from the empty directory and require user-root locations.
7. Run exactly: codearts run --auto -m <external-model-id> --format json "Explicitly use the resolving-merge-conflicts skill. This isolated repository is already in an in-progress merge conflict. Inspect both commits as primary sources, resolve format.js so it preserves uppercase-name and status-suffix intents, run node --test, stage the resolution, and finish the merge with a non-interactive commit. Never abort. Do not add dependencies."
8. Pass only with completed Skill "resolving-merge-conflicts" and two tests pass, no unmerged files or MERGE_HEAD remain, HEAD has two parents, and format.js preserves both intents.
9. Removal must delete $userRoot/skills/resolving-merge-conflicts; if the source checkout was created here and is not shared, also delete the exact isolated .tmp/mattpocock-skills-6654f6b. Preserve the skills and .tmp parents, package.json, codearts_cli.json, unrelated Skills, and credentials.
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
$names = @("resolving-merge-conflicts")
if ($names | Where-Object { Test-Path -LiteralPath (Join-Path $target $_) }) { throw "Same-name Skill exists" }
Copy-Item -LiteralPath (Join-Path $source "skills\engineering\resolving-merge-conflicts") -Destination (Join-Path $target "resolving-merge-conflicts") -Recurse
```

The pinned source is content-only: no dependencies or lifecycle scripts.

Before verification, create the complete fixture from the Agent prompt in an isolated directory:

```powershell
git init -b main
git config user.email "verification@example.invalid"
git config user.name "CodeArts Verification"
Set-Content package.json '{"type":"module"}'
Set-Content format.js 'export function format(item) { return item.name; }'
git add .
git commit -m "baseline formatter"
git checkout -b feature-uppercase
Set-Content format.js 'export function format(item) { return item.name.toUpperCase(); }'
Set-Content uppercase.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('uppercases name', () => assert.match(format({name:'box',status:'ready'}), /^BOX/));"
git add .
git commit -m "preserve uppercase name intent"
git checkout main
Set-Content format.js "export function format(item) { return item.name + ':' + item.status; }"
Set-Content status.test.js "import test from 'node:test'; import assert from 'node:assert/strict'; import { format } from './format.js'; test('includes status suffix', () => assert.match(format({name:'box',status:'ready'}), /:ready$/));"
git add .
git commit -m "preserve status suffix intent"
git merge feature-uppercase
if (-not (Test-Path .git\MERGE_HEAD)) { throw "Expected conflict" }
```

## CodeArts model and environment

Use a configured external model; this run used `mimo/mimo-v2.5`:

```powershell
$env:CODEARTS_CLI_AK = "local-placeholder"
$env:CODEARTS_CLI_SK = "local-placeholder"
codearts debug skill
codearts run --auto -m "mimo/mimo-v2.5" --format json "Explicitly use the resolving-merge-conflicts skill. This isolated repository is already in an in-progress merge conflict. Inspect both commits as primary sources, resolve format.js so it preserves uppercase-name and status-suffix intents, run node --test, stage the resolution, and finish the merge with a non-interactive commit. Never abort. Do not add dependencies."
Remove-Item Env:CODEARTS_CLI_AK,Env:CODEARTS_CLI_SK -ErrorAction SilentlyContinue
```

The placeholders only satisfy the CLI's local environment check; model authentication comes from the selected external-model configuration. Never persist them or place real AK/SK values in command history.

## Verification and success criteria

Require the expected locations, completed Skill "resolving-merge-conflicts", and two tests pass, no unmerged files or MERGE_HEAD remain, HEAD has two parents, and format.js preserves both intents. A plausible answer without completed `skill` events is not a pass.

## Usage

```text
Explicitly use the resolving-merge-conflicts skill. This isolated repository is already in an in-progress merge conflict. Inspect both commits as primary sources, resolve format.js so it preserves uppercase-name and status-suffix intents, run node --test, stage the resolution, and finish the merge with a non-interactive commit. Never abort. Do not add dependencies.
```

## Update

Audit a new tag/commit, then repeat discovery, invocation, second-project reproduction, and rollback in isolation before replacing the pinned directories. Do not track `main` directly.

## Uninstall

Delete only the `resolving-merge-conflicts` directories in the chosen scope. Preserve the parent, unrelated Skills, `package.json`, and `codearts_cli.json`. Resolve exact absolute paths first, then rerun discovery.

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

This is a high-impact writing and Git-commit flow; use `--auto` only on an isolated or backed-up branch. In all three runs, `edit`/`write` was rejected first and CodeArts succeeded through completed Bash writes. Rebase, multi-file conflicts, and remote PRs were not tested.

## Security

The pinned source is copied as content only; still audit every target directory before installation. Placeholder AK/SK values are not credentials and must remain process-local. Project scope affects collaborators; user scope affects all sessions not shadowed by a same-named project Skill.

## Evidence and sources

- [2026-08-28 verification](../../research/2026-08-28.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned upstream source](https://github.com/mattpocock/skills/tree/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/resolving-merge-conflicts)
