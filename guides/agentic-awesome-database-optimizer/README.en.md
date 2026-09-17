# Use database-optimizer in CodeArts CLI

[简体中文](README.md)

Install `database-optimizer` to turn measured query plans and latency into a testable, reversible optimization proposal.

## Choose an installation scope

Use project `<project>/.codeartsdoer` for one repository or user `~/.codeartsdoer` for reuse. Choose one; project scope wins same-name collisions. Stop rather than overwrite a Skill or vendor target.

## Ask an Agent to install it

### Project-scope prompt

```text
Install and verify project-scope database-optimizer in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git v17.3.0, commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, source skills/database-optimizer. Run codearts --version, git --version, and codearts models and let me choose <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-database-optimizer', and $target=Join-Path $root 'skills\database-optimizer'. Stop if $source, $target, or ~/.codeartsdoer/skills/database-optimizer exists. Do not modify package.json, codearts_cli.json, credentials, plugins, or other Skills.
Run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/database-optimizer/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require HEAD to match, then Copy-Item -LiteralPath (Join-Path $source 'skills\database-optimizer') -Destination $target -Recurse.
Run codearts debug skill and require the sole database-optimizer location to be $target/SKILL.md. Replace <model>, then run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name database-optimizer and use no other tool. Do not access files or the network. Analyze only this PostgreSQL evidence: orders has 2,000,000 rows; query SELECT id,total FROM orders WHERE customer_id=$1 AND created_at >= $2 ORDER BY created_at DESC LIMIT 50; p95 is 800 ms; EXPLAIN shows a sequential scan; no relevant index exists; the database time is 650 ms of the 800 ms request. Recommend the first optimization, exact index column order, how to validate with EXPLAIN ANALYZE and before/after p95, rollback, and unknowns. Do not claim an unmeasured improvement.' Pass only on exit 0, exactly one completed target Skill event, no other tool event, `(customer_id, created_at DESC)`, EXPLAIN ANALYZE, before/after p95, DROP INDEX rollback, and no invented measured benefit. Report commit, path, and event. Remove only exact $target and $source and confirm the path disappeared.
```

### User-scope prompt

```text
Install and verify user-scope database-optimizer for the current Windows user from https://github.com/sickn33/agentic-awesome-skills.git v17.3.0 at 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, source skills/database-optimizer. Run codearts --version, git --version, and codearts models and let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-database-optimizer', and $target=Join-Path $root 'skills\database-optimizer'. Stop on either target or a same-name project Skill. Do not modify user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/database-optimizer/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require HEAD to match, then Copy-Item -LiteralPath (Join-Path $source 'skills\database-optimizer') -Destination $target -Recurse. In a fresh consumer without a project override, run codearts debug skill and require the sole database-optimizer location to be $target/SKILL.md. Replace <model>, then run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name database-optimizer and use no other tool. Do not access files or the network. Analyze only this PostgreSQL evidence: orders has 2,000,000 rows; query SELECT id,total FROM orders WHERE customer_id=$1 AND created_at >= $2 ORDER BY created_at DESC LIMIT 50; p95 is 800 ms; EXPLAIN shows a sequential scan; no relevant index exists; the database time is 650 ms of the 800 ms request. Recommend the first optimization, exact index column order, how to validate with EXPLAIN ANALYZE and before/after p95, rollback, and unknowns. Do not claim an unmeasured improvement.' Pass only on exit 0, exactly one completed event from $target, no other tool event, `(customer_id, created_at DESC)`, EXPLAIN ANALYZE, before/after p95, DROP INDEX rollback, and no invented measured benefit. Report commit, path, and event. Remove only exact $target, $source, and verified consumer and confirm the path disappeared; never delete the user root or other Skills.
```

## Manual Windows installation

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-database-optimizer'; $target=Join-Path $root 'skills\database-optimizer'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/database-optimizer/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\database-optimizer') -Destination $target -Recurse
```

## CodeArts configuration

Choose an available model with `codearts models`. The Skill has no runtime dependency; do not connect to a live database for this smoke test.

## Verification

Run the exact debug and synthetic test above. Require one completed target Skill event, the exact index order, measurement and rollback steps, and an explicit unmeasured-benefit boundary.

## Usage

```text
Use database-optimizer. Start from my plan and latency baseline; propose the smallest measurable, reversible change and do not guess the benefit.
```

## Update

Re-audit the full Skill and license and rerun every scope and rollback before changing the commit.

## Uninstall

Remove only `$root/skills/database-optimizer` and `$root/vendor/agentic-awesome-database-optimizer`, then confirm the old path disappeared.

## Verified result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | `v17.3.0`; commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `database-optimizer` / `9FF4B8C8D4726F9A388932B46D74B370FF8D417758704BF343F7DE1AD5F45917` |
| Content license | Original AAS non-code content: CC BY 4.0 |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scope | Two fresh projects plus user scope; 2026-09-17 |

## Known limitations

Only read-only advice over synthetic PostgreSQL evidence was tested; no database connection, index creation, or load test occurred.

## Security

The pinned directory is one 10,534-byte `SKILL.md` with no dependencies, scripts, binaries, downloader, or telemetry. Every run emitted only the target Skill event.

## Evidence and sources

- [English research](../../research/2026-09-17.en.md) · [中文](../../research/2026-09-17.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/database-optimizer)
- [AAS content license](https://github.com/sickn33/agentic-awesome-skills/blob/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/LICENSE-CONTENT)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
