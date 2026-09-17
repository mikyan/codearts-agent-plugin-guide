# Use error-detective in CodeArts CLI

[简体中文](README.md)

Install `error-detective` to turn cross-service logs into a timeline, correlation chain, falsifiable root-cause hypothesis, and recurrence query.

## Choose an installation scope

Use project `<project>/.codeartsdoer` for one repository or user `~/.codeartsdoer` for reuse. Choose one; project scope wins same-name collisions. Stop rather than overwrite a Skill or vendor target.

## Ask an Agent to install it

### Project-scope prompt

```text
Install and verify project-scope error-detective in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git v17.3.0, commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, source skills/error-detective. Run codearts --version, git --version, and codearts models and let me choose <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-error-detective', and $target=Join-Path $root 'skills\error-detective'. Stop if $source, $target, or ~/.codeartsdoer/skills/error-detective exists. Do not modify package.json, codearts_cli.json, credentials, plugins, or other Skills.
Run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/error-detective/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require HEAD to match, then Copy-Item -LiteralPath (Join-Path $source 'skills\error-detective') -Destination $target -Recurse.
Run codearts debug skill and require the sole error-detective location to be $target/SKILL.md. Replace <model>, then run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name error-detective and use no other tool. Do not access files or the network. Analyze only these logs: 10:00 deploy api v7 completed; 10:02 api request r-17 returned 500 with db timeout; 10:02 payments request r-17 returned 502 after api failure. Produce a timeline, correlation, root-cause hypothesis explicitly labeled as a hypothesis, counterfactual unknowns, one regex or monitoring query for recurrence, and immediate verification steps. Do not claim the deployment caused the timeout.' Pass only on exit 0, exactly one completed target Skill event, no other tool event, an explicit correlation-not-causation boundary, and unknown database state and deployment diff. Report commit, path, and event. Remove only exact $target and $source and confirm the path disappeared.
```

### User-scope prompt

```text
Install and verify user-scope error-detective for the current Windows user from https://github.com/sickn33/agentic-awesome-skills.git v17.3.0 at 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, source skills/error-detective. Run codearts --version, git --version, and codearts models and let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-error-detective', and $target=Join-Path $root 'skills\error-detective'. Stop on either target or a same-name project Skill. Do not modify user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/error-detective/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require HEAD to match, then Copy-Item -LiteralPath (Join-Path $source 'skills\error-detective') -Destination $target -Recurse. In a fresh consumer without a project override, run codearts debug skill and require the sole error-detective location to be $target/SKILL.md. Replace <model>, then run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name error-detective and use no other tool. Do not access files or the network. Analyze only these logs: 10:00 deploy api v7 completed; 10:02 api request r-17 returned 500 with db timeout; 10:02 payments request r-17 returned 502 after api failure. Produce a timeline, correlation, root-cause hypothesis explicitly labeled as a hypothesis, counterfactual unknowns, one regex or monitoring query for recurrence, and immediate verification steps. Do not claim the deployment caused the timeout.' Pass only on exit 0, exactly one completed event from $target, no other tool event, an explicit correlation-not-causation boundary, and unknown database state and deployment diff. Report commit, path, and event. Remove only exact $target, $source, and verified consumer and confirm the path disappeared; never delete the user root or other Skills.
```

## Manual Windows installation

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-error-detective'; $target=Join-Path $root 'skills\error-detective'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/error-detective/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\error-detective') -Destination $target -Recurse
```

## CodeArts configuration

Choose an available model with `codearts models`. The Skill has no runtime dependency. Redact sensitive data before supplying real logs.

## Verification

Run the exact debug and synthetic test above. Require one completed target Skill event, an accurate timeline and request chain, and a hypothesis that does not promote correlation to causation.

## Usage

```text
Use error-detective. From these redacted logs only, build a timeline and falsifiable root-cause hypothesis; list counterfactual unknowns and a recurrence query.
```

## Update

Re-audit the full Skill and license and rerun every scope and rollback before changing the commit.

## Uninstall

Remove only `$root/skills/error-detective` and `$root/vendor/agentic-awesome-error-detective`, then confirm the old path disappeared.

## Verified result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | `v17.3.0`; commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `error-detective` / `6BB4EBB968B61ED5CC9F44951F99350411D79ACBBDAF0F4BF9F124D9BFB2A3A0` |
| Content license | Original AAS non-code content: CC BY 4.0 |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scope | Two fresh projects plus user scope; 2026-09-17 |

## Known limitations

Only read-only analysis of three synthetic log lines was tested; no real log system, codebase, or monitoring backend was read. Adapt generated regexes and queries to the actual schema.

## Security

The pinned directory is one 2,166-byte `SKILL.md` with no dependencies, scripts, binaries, downloader, or telemetry. Every run emitted only the target Skill event.

## Evidence and sources

- [English research](../../research/2026-09-17.en.md) · [中文](../../research/2026-09-17.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/error-detective)
- [AAS content license](https://github.com/sickn33/agentic-awesome-skills/blob/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/LICENSE-CONTENT)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
