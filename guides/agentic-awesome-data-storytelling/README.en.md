# Use data-storytelling in CodeArts CLI

[简体中文](README.md)

Install `data-storytelling` to turn supplied numbers into an executive narrative while keeping causal, forecast, and budget uncertainty explicit.

## Choose an installation scope

Use project `<project>/.codeartsdoer` for one repository or user `~/.codeartsdoer` for reuse. Choose one; project scope wins a same-name collision. Stop rather than overwrite any Skill or vendor target.

## Ask an Agent to install it

### Project-scope prompt

```text
Install and verify project-scope data-storytelling in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.3.0, commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, source skills/data-storytelling. Run codearts --version, git --version, and codearts models and let me choose <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-data-storytelling', and $target=Join-Path $root 'skills\data-storytelling'. Stop if $source, $target, or ~/.codeartsdoer/skills/data-storytelling exists. Do not modify package.json, codearts_cli.json, credentials, plugins, or other Skills.
Run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/data-storytelling/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require HEAD to equal that commit, then Copy-Item -LiteralPath (Join-Path $source 'skills\data-storytelling') -Destination $target -Recurse.
Run codearts debug skill and require the sole data-storytelling location to be $target/SKILL.md. Replace <model>, then run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name data-storytelling and use no other tool. Do not access files or the network. Turn only these facts into a concise executive data story: quarterly signups Q1=100, Q2=120, Q3=90; no causal data and no budget information were supplied. Include a headline, context, key insight with correct arithmetic, one evidence-bounded next action, and an uncertainty note. Do not invent causes, benchmarks, money, confidence intervals, or forecasts.' Pass only on exit 0, exactly one completed target Skill event, no other tool event, Q2 +20%, Q3 -25%, and no invented cause. Report commit, path, and event. Remove only exact $target and $source and confirm the path disappeared; never remove all of .codeartsdoer or root configuration.
```

### User-scope prompt

```text
Install and verify user-scope data-storytelling for the current Windows user from https://github.com/sickn33/agentic-awesome-skills.git v17.3.0 at 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, source skills/data-storytelling. Run codearts --version, git --version, and codearts models and let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-data-storytelling', and $target=Join-Path $root 'skills\data-storytelling'. Stop on either target or a same-name project Skill. Do not modify user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/data-storytelling/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require HEAD to match, then Copy-Item -LiteralPath (Join-Path $source 'skills\data-storytelling') -Destination $target -Recurse. In a fresh consumer without a project override, run codearts debug skill and require the sole data-storytelling location to be $target/SKILL.md. Replace <model>, then run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name data-storytelling and use no other tool. Do not access files or the network. Turn only these facts into a concise executive data story: quarterly signups Q1=100, Q2=120, Q3=90; no causal data and no budget information were supplied. Include a headline, context, key insight with correct arithmetic, one evidence-bounded next action, and an uncertainty note. Do not invent causes, benchmarks, money, confidence intervals, or forecasts.' Pass only on exit 0, exactly one completed event from $target, no other tool event, Q2 +20%, Q3 -25%, and no invented cause. Report commit, path, and event. Remove only exact $target, $source, and verified consumer and confirm the path disappeared; never delete the user root or other Skills.
```

## Manual Windows installation

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-data-storytelling'; $target=Join-Path $root 'skills\data-storytelling'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/data-storytelling/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\data-storytelling') -Destination $target -Recurse
```

## CodeArts configuration

Choose an available model with `codearts models`. No dependency or configuration merge is required.

## Verification

Use the exact debug and smoke test above. Require one completed target Skill event, correct arithmetic, and no invented cause, forecast, budget, or benchmark.

## Usage

```text
Use data-storytelling. Use only my data; separate observations, interpretations, and unknowns.
```

## Update

Re-audit the full Skill and license and rerun every scope and rollback before changing the commit.

## Uninstall

Remove only `$root/skills/data-storytelling` and `$root/vendor/agentic-awesome-data-storytelling`, then confirm the old path disappeared.

## Verified result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | `v17.3.0`; commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `data-storytelling` / `25B3D4B6538627B85FDE94A8DDAE5FDB6ECE78500FB8DFA8D3F026D0B6104488` |
| Content license | Original AAS non-code content: CC BY 4.0 |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scope | Two fresh projects plus user scope; 2026-09-17 |

## Known limitations

Only a read-only narrative over three synthetic data points was tested; charts, decks, external benchmarks, and significance testing remain untested.

## Security

The pinned directory is one 13,586-byte `SKILL.md` with no dependencies, scripts, binaries, downloader, or telemetry. Every run emitted only the target Skill event.

## Evidence and sources

- [English research](../../research/2026-09-17.en.md) · [中文](../../research/2026-09-17.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/data-storytelling)
- [AAS content license](https://github.com/sickn33/agentic-awesome-skills/blob/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/LICENSE-CONTENT)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
