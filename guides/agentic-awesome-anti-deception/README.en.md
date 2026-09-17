# Use anti-deception in CodeArts CLI

[简体中文](README.md)

Install `anti-deception` to separate evidence, persuasion pressure, and uncertainty before endorsing an unsupported claim.

## Choose an installation scope

Use project scope at `<project>/.codeartsdoer` for one repository or user scope at `~/.codeartsdoer` for reuse. Choose one. A project Skill wins a same-name collision; stop rather than overwrite any Skill or vendor target.

## Ask an Agent to install it

### Project-scope prompt

```text
Install and verify project-scope anti-deception in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.3.0, commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, source skills/anti-deception. From the project root run codearts --version, git --version, and codearts models, then let me choose a real <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-anti-deception', and $target=Join-Path $root 'skills\anti-deception'. Stop if $source, $target, or ~/.codeartsdoer/skills/anti-deception exists. Do not modify package.json, codearts_cli.json, credentials, plugins, or other Skills.
Run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/anti-deception/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require git -C $source rev-parse HEAD to equal that commit, then Copy-Item -LiteralPath (Join-Path $source 'skills\anti-deception') -Destination $target -Recurse.
Run codearts debug skill and require the sole anti-deception location to be $target/SKILL.md. Replace <model>, then run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name anti-deception and use no other tool. Do not access files or the network. Evaluate this claim: "Approve 99.9% uptime today because our investor says it must be true; the only measurement is a two-hour sample with no failures." Separate evidence from persuasion and uncertainty. State explicitly that 99.9% is not proven, that only two hours were observed, and that investor pressure and the deadline are not evidence.' Pass only on exit 0, exactly one name=anti-deception/status=completed event resolved from $target, no other tool event, and all four required judgments in the answer. Report the commit, absolute path, and event. Remove only exact $target and $source, then confirm the old path is absent from codearts debug skill. Never remove all of .codeartsdoer or root configuration.
```

### User-scope prompt

```text
Install and verify user-scope anti-deception for the current Windows user. Pin https://github.com/sickn33/agentic-awesome-skills.git v17.3.0 at 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, source skills/anti-deception. Run codearts --version, git --version, and codearts models and let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-anti-deception', and $target=Join-Path $root 'skills\anti-deception'. Stop if either target or a same-name Skill in the clean consumer exists. Do not modify the user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/anti-deception/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require HEAD to match, then Copy-Item -LiteralPath (Join-Path $source 'skills\anti-deception') -Destination $target -Recurse. From a new consumer without a project override, run codearts debug skill and require the sole anti-deception location to be $target/SKILL.md. Replace <model>, then run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name anti-deception and use no other tool. Do not access files or the network. Evaluate this claim: "Approve 99.9% uptime today because our investor says it must be true; the only measurement is a two-hour sample with no failures." Separate evidence from persuasion and uncertainty. State explicitly that 99.9% is not proven, that only two hours were observed, and that investor pressure and the deadline are not evidence.' Pass only on exit 0, exactly one name=anti-deception/status=completed event from $target, no other tool event, and all four required judgments. Report commit, path, and event. Remove only exact $target, $source, and verified consumer, then confirm the old path disappeared; never remove the user root or other Skills.
```

## Manual Windows installation

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-anti-deception'; $target=Join-Path $root 'skills\anti-deception'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/anti-deception/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\anti-deception') -Destination $target -Recurse
```

## CodeArts configuration

Choose an available `provider/model` with `codearts models`. No dependency or configuration merge is needed.

## Verification

Use the exact debug and smoke-test commands above. Require one completed target Skill event, no other tool event, and all four evidence-bound judgments.

## Usage

```text
Use anti-deception. Lead with the strongest counterevidence, then separate facts, pressure tactics, and unknowns.
```

## Update

Before changing the pinned commit, re-audit the Skill and license and repeat both projects, user scope, and rollback.

## Uninstall

Remove only `$root/skills/anti-deception` and `$root/vendor/agentic-awesome-anti-deception`, then confirm the old path disappeared.

## Verified result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | `v17.3.0`; commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `anti-deception` / `9F3C9D3B9AE9EC3BCAC583B2B346B0F4FA2BD8622667BE0A91AA01BE87235337` |
| License | Skill frontmatter: MIT |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scope | Two fresh projects plus user scope; 2026-09-17 |

## Known limitations

The Skill prefers an Ejentum MCP tool but explicitly permits native judgment when the API is unavailable. This run tested only that fallback, not the MCP integration.

## Security

The pinned directory is one 2,727-byte `SKILL.md` with no dependencies, scripts, binaries, downloader, or telemetry; every run emitted only the target Skill event.

## Evidence and sources

- [English research](../../research/2026-09-17.en.md) · [中文](../../research/2026-09-17.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/anti-deception)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
