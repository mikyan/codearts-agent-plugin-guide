# Use cross-platform-contract-propagation-audit with CodeArts CLI

[简体中文](README.md)

Install `cross-platform-contract-propagation-audit` to trace a field, enum, flag, or API contract across storage, services, clients, analytics, and tests without changing them.

## Choose a scope

Use `<project>/.codeartsdoer` for one repository or `~/.codeartsdoer` for cross-project reuse. Pick one; a same-named project Skill takes priority. Stop on an existing Skill or vendor target.

## Copy-ready Agent prompts

### Project scope

```text
Install and verify project-scoped cross-platform-contract-propagation-audit in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.3.0, commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, directory skills/cross-platform-contract-propagation-audit.
From the project root run codearts --version, git --version, and codearts models, then let me select a real <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-cross-platform-contract-propagation-audit', and $target=Join-Path $root 'skills\cross-platform-contract-propagation-audit'. Check $source, $target, and ~/.codeartsdoer/skills/cross-platform-contract-propagation-audit; stop on any collision. Create only $source and $target. Do not modify package.json, codearts_cli.json, credentials, plugins, or another Skill.
Run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/cross-platform-contract-propagation-audit/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require git -C $source rev-parse HEAD to equal that commit, then run Copy-Item -LiteralPath (Join-Path $source 'skills\cross-platform-contract-propagation-audit') -Destination $target -Recurse.
Run codearts debug skill; the sole active same-named location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name cross-platform-contract-propagation-audit. Use no other tool, do not access the network, and do not read or write files. Audit only these synthetic facts for nullable can_complete: database and detail API contain it; list API omits it; Web hides the action when the field is missing; Android explicit-null behavior is untested; feature flag is evaluated in detail API only; click analytics omits capability and cohort; no runtime tests were executed. Return exact sections CONTRACT, PROPAGATION GRAPH, STATUS TABLE, STATE MATRIX, RELEASE GATES, VERDICT, and LIMITS. Mark the list projection and analytics gaps missing, Android and unexecuted cells unknown, and conclude blocked rather than complete.'
Pass only on exit 0, exactly one name=cross-platform-contract-propagation-audit/status=completed event from $target, no other tool event, and all seven sections marking list/analytics missing, Android and unexecuted cells unknown, and the verdict blocked. Report commit, absolute paths, and events. Uninstall only exact $target and $source, then confirm the old path is absent with codearts debug skill. Never delete all of .codeartsdoer or root configuration.
```

### User scope

```text
Install and verify cross-platform-contract-propagation-audit for the current Windows user. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.3.0, commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, directory skills/cross-platform-contract-propagation-audit.
Run codearts --version, git --version, and codearts models, then let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-cross-platform-contract-propagation-audit', and $target=Join-Path $root 'skills\cross-platform-contract-propagation-audit'. Confirm $source, $target, and the same-named project Skill in a fresh consumer are absent; stop on collision. Do not change user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/cross-platform-contract-propagation-audit/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; verify HEAD, then Copy-Item -LiteralPath (Join-Path $source 'skills\cross-platform-contract-propagation-audit') -Destination $target -Recurse.
Enter the fresh consumer with no project override. The sole codearts debug skill location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name cross-platform-contract-propagation-audit. Use no other tool, do not access the network, and do not read or write files. Audit only these synthetic facts for nullable can_complete: database and detail API contain it; list API omits it; Web hides the action when the field is missing; Android explicit-null behavior is untested; feature flag is evaluated in detail API only; click analytics omits capability and cohort; no runtime tests were executed. Return exact sections CONTRACT, PROPAGATION GRAPH, STATUS TABLE, STATE MATRIX, RELEASE GATES, VERDICT, and LIMITS. Mark the list projection and analytics gaps missing, Android and unexecuted cells unknown, and conclude blocked rather than complete.' Pass only on exit 0, exactly one name=cross-platform-contract-propagation-audit/status=completed event from $target, no other tool event, and all seven sections marking list/analytics missing, Android and unexecuted cells unknown, and verdict blocked. Report commit, absolute paths, and events. Remove only exact $target, $source, and the verified consumer, then confirm the old path is absent with codearts debug skill; never remove the user root, root configuration, credentials, or another Skill.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models`. This defaults to project scope; replace only `$root` for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-cross-platform-contract-propagation-audit'
$target=Join-Path $root 'skills\cross-platform-contract-propagation-audit'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/cross-platform-contract-propagation-audit/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\cross-platform-contract-propagation-audit') -Destination $target -Recurse
```

## CodeArts configuration

Choose an available `provider/model` with `codearts models`. Keep credentials only in documented user configuration. This Skill does not require changes to `package.json` or `codearts_cli.json`.

## Verification

Confirm the sole source with `codearts debug skill`, then run the full Agent-prompt command. Success requires exit 0, exactly one completed target event from the expected path, no other tool event, and a traceable status table, matrix, release gates, and blocked verdict.

## Use

```text
Use cross-platform-contract-propagation-audit. Read-only audit this field across the database, every API projection, clients, analytics events, and tests. Put missing, null, false, true, and unknown-enum states in a matrix; keep absent evidence unknown.
```

## Update

Check the pin with `git -C $source rev-parse HEAD`. Before changing it, re-audit the license and pinned directory, then repeat both project tests, user scope, and rollback.

## Uninstall

Remove only `$root/skills/cross-platform-contract-propagation-audit` and `$root/vendor/agentic-awesome-cross-platform-contract-propagation-audit`, then confirm the old path is absent with `codearts debug skill`. Never remove all of `.codeartsdoer`, root configuration, or another Skill.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | release `v17.3.0`; commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `cross-platform-contract-propagation-audit` / `EECA23D5CC381ED3CB3B816004DBF7401FA30C11EBEDF9D58A333B0A95415655` |
| Content license | CC BY 4.0 (AAS `LICENSE-CONTENT`) |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scopes | two clean projects plus user scope; 2026-09-16 |

## Limitations

Testing covered only a read-only synthetic audit of supplied facts. It did not inspect a real repository, generate code, run tests, or change a feature flag. Supply the business invariant and scope first; keep unstated mapper, client, and runtime facts `unknown`.

## Security

The pinned directory contains one 9,238-byte `SKILL.md` and no dependency, lockfile, lifecycle script, binary, downloader, or telemetry. All three runs emitted only the target Skill event with no file, shell, or network access.

## Evidence and sources

- [中文研究记录](../../research/2026-09-16.md) · [English](../../research/2026-09-16.en.md)
- [Pinned Skill directory](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/cross-platform-contract-propagation-audit)
- [AAS content license](https://github.com/sickn33/agentic-awesome-skills/blob/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/LICENSE-CONTENT)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
