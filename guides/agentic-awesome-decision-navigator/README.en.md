# Use decision-navigator with CodeArts CLI

[简体中文](README.md)

Install `decision-navigator` to narrow an overwhelming problem through one branching question at a time until concrete next steps are justified.

## Choose a scope

Use `<project>/.codeartsdoer` for one repository or `~/.codeartsdoer` for cross-project reuse. Pick one; a same-named project Skill takes priority. Stop on an existing Skill or vendor target.

## Copy-ready Agent prompts

### Project scope

```text
Install and verify project-scoped decision-navigator in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.3.0, commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, directory skills/decision-navigator.
From the project root run codearts --version, git --version, and codearts models, then let me select a real <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-decision-navigator', and $target=Join-Path $root 'skills\decision-navigator'. Check $source, $target, and ~/.codeartsdoer/skills/decision-navigator; stop on any collision. Create only $source and $target. Do not modify package.json, codearts_cli.json, credentials, plugins, or another Skill.
Run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/decision-navigator/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require git -C $source rev-parse HEAD to equal that commit, then run Copy-Item -LiteralPath (Join-Path $source 'skills\decision-navigator') -Destination $target -Recurse.
Run codearts debug skill; the sole active decision-navigator location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name decision-navigator. Use no other tool, do not access the network, and do not read or write files. Respond to this synthetic user message: "I feel stuck choosing what to build next. I have one weekend, no budget, and want something useful for my portfolio." Follow the first branching turn only: acknowledge the supplied constraints, ask exactly one useful question, give three to five mutually distinct option labels of two to six words, include a not sure option, and do not give action steps yet.'
Pass only on exit 0, exactly one name=decision-navigator/status=completed event from $target, no other tool event, and a final answer that acknowledges weekend/no-budget/portfolio constraints, asks one question, offers 3–5 short options including not sure, and gives no action steps. Report commit, absolute paths, and events. Uninstall only exact $target and $source, then confirm the old path is absent with codearts debug skill. Never delete all of .codeartsdoer or root configuration.
```

### User scope

```text
Install and verify decision-navigator for the current Windows user. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.3.0, commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, directory skills/decision-navigator.
Run codearts --version, git --version, and codearts models, then let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-decision-navigator', and $target=Join-Path $root 'skills\decision-navigator'. Confirm $source, $target, and .codeartsdoer/skills/decision-navigator in a fresh consumer are absent; stop on collision. Do not change user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/decision-navigator/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; verify HEAD, then Copy-Item -LiteralPath (Join-Path $source 'skills\decision-navigator') -Destination $target -Recurse.
Enter the fresh consumer with no project override. The sole codearts debug skill location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name decision-navigator. Use no other tool, do not access the network, and do not read or write files. Respond to this synthetic user message: "I feel stuck choosing what to build next. I have one weekend, no budget, and want something useful for my portfolio." Follow the first branching turn only: acknowledge the supplied constraints, ask exactly one useful question, give three to five mutually distinct option labels of two to six words, include a not sure option, and do not give action steps yet.' Pass only on exit 0, exactly one name=decision-navigator/status=completed event from $target, no other tool event, and a final answer that acknowledges weekend/no-budget/portfolio constraints, asks one question, offers 3–5 short options including not sure, and gives no action steps. Report commit, absolute paths, and events. Remove only exact $target, $source, and the verified consumer, then confirm the old path is absent with codearts debug skill; never remove the user root, root configuration, credentials, or another Skill.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models`. This defaults to project scope; replace only `$root` for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-decision-navigator'; $target=Join-Path $root 'skills\decision-navigator'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/decision-navigator/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\decision-navigator') -Destination $target -Recurse
```

## CodeArts configuration

Choose an available `provider/model` with `codearts models`. Keep credentials only in documented user configuration. This Skill does not require changes to `package.json` or `codearts_cli.json`.

## Verification

Confirm the sole source with `codearts debug skill`, then run the full Agent-prompt command. Success requires exit 0, one completed `decision-navigator` event from the target path, no other tool event, one question, 3–5 short options, and no premature action plan.

## Use

```text
Use decision-navigator. I feel stuck about what to do next. Reflect the constraints already stated, then ask one question at a time with 3–5 mutually exclusive short options and a not-sure choice.
```

## Update

Check the pin with `git -C $source rev-parse HEAD`. Before changing it, re-audit the license and all of `skills/decision-navigator`, then repeat both project tests, user scope, and rollback.

## Uninstall

Remove only `$root/skills/decision-navigator` and `$root/vendor/agentic-awesome-decision-navigator`, then confirm the old path is absent with `codearts debug skill`. Never remove all of `.codeartsdoer`, root configuration, or another Skill.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | release `v17.3.0`; commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `decision-navigator` / `8C1B76E06F12CE3682324D0BAA5A88810527DE40425210E5F51BC612DC99AED0` |
| Content license | CC BY 4.0 (AAS `LICENSE-CONTENT`) |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scopes | two clean projects plus user scope; 2026-09-16 |

## Limitations

Testing covered the first branching turn only. It did not complete a three-to-four-level conversation, produce final action steps, or cover high-stakes mental-health, legal, medical, or financial situations. Recommendations still depend on later user answers.

## Security

The pinned directory contains one 9,749-byte `SKILL.md` and no dependency, lockfile, lifecycle script, binary, downloader, or telemetry. All three runs emitted only the target Skill event with no file, shell, or network access.

## Evidence and sources

- [中文研究记录](../../research/2026-09-16.md) · [English](../../research/2026-09-16.en.md)
- [Pinned Skill directory](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/decision-navigator)
- [AAS content license](https://github.com/sickn33/agentic-awesome-skills/blob/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/LICENSE-CONTENT)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
