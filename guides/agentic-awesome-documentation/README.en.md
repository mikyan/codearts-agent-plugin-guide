# Use documentation with CodeArts CLI

[简体中文](README.md)

Install `documentation` to plan audiences, information architecture, ownership, and quality gates for README, API, architecture, operations, and release documentation.

## Choose a scope

Use `<project>/.codeartsdoer` for one repository or `~/.codeartsdoer` for cross-project reuse. Pick one; a same-named project Skill takes priority. Stop if the Skill or vendor target exists and never overwrite it.

## Copy-ready Agent prompts

### Project scope

```text
Install and verify project-scoped documentation in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.2.0, commit 2fce708d4f3871ccce3d705b4748371f94c730ec, directory skills/documentation.
Run codearts --version, git --version, and codearts models, then let me select a real <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-documentation', and $target=Join-Path $root 'skills\documentation'. Check $source, $target, and ~/.codeartsdoer/skills/documentation; stop on any collision. Create only $source and $target. Do not modify package.json, codearts_cli.json, credentials, plugins, or another Skill.
From the project root run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/documentation/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec. Require git -C $source rev-parse HEAD to equal that commit, then run Copy-Item -LiteralPath (Join-Path $source 'skills\documentation') -Destination $target -Recurse.
Run codearts debug skill; the sole active documentation location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name documentation. Use no other tool, do not access the network, and do not read or write files. Create a documentation plan for a synthetic local REST service that has a README but lacks API reference, architecture overview, troubleshooting, and release notes. The audience is new contributors and operators. Return exact sections AUDIENCES, INFORMATION ARCHITECTURE, DELIVERABLES, OWNERSHIP, QUALITY GATES, and OPEN QUESTIONS. Include link checking, executable example validation, review cadence, and state that no repository content was inspected.'
Pass only on exit 0, exactly one name=documentation/status=completed event from $target, no other tool event, and all six sections plus every requested criterion. Report commit, absolute paths, and events. Uninstall only exact $target and $source, then confirm the old path is absent with codearts debug skill. Never delete all of .codeartsdoer or root configuration.
```

### User scope

```text
Install and verify documentation for the current Windows user from https://github.com/sickn33/agentic-awesome-skills.git release v17.2.0, commit 2fce708d4f3871ccce3d705b4748371f94c730ec, directory skills/documentation.
Run codearts --version, git --version, and codearts models, then let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-documentation', and $target=Join-Path $root 'skills\documentation'. Confirm $source, $target, and .codeartsdoer/skills/documentation in a fresh consumer are absent; stop on collision. Do not change user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/documentation/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec; verify HEAD, then Copy-Item -LiteralPath (Join-Path $source 'skills\documentation') -Destination $target -Recurse.
Enter the fresh consumer with no project override. The sole codearts debug skill location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name documentation. Use no other tool, do not access the network, and do not read or write files. Create a documentation plan for a synthetic local REST service that has a README but lacks API reference, architecture overview, troubleshooting, and release notes. The audience is new contributors and operators. Return exact sections AUDIENCES, INFORMATION ARCHITECTURE, DELIVERABLES, OWNERSHIP, QUALITY GATES, and OPEN QUESTIONS. Include link checking, executable example validation, review cadence, and state that no repository content was inspected.'
Apply the project success criteria. Remove only exact $target, $source, and the verified consumer; never remove the user root, root configuration, credentials, or another Skill.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models`. This defaults to project scope; replace only `$root` for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-documentation'; $target=Join-Path $root 'skills\documentation'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/documentation/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec
if((git -C $source rev-parse HEAD).Trim() -ne '2fce708d4f3871ccce3d705b4748371f94c730ec'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\documentation') -Destination $target -Recurse
```

## CodeArts configuration

Choose an available `provider/model` with `codearts models`. Keep credentials only in documented user configuration. This Skill does not require changes to `package.json` or `codearts_cli.json`.

## Verification

Use `codearts debug skill` to confirm the sole source, then run the complete command in the Agent prompt. Success requires exit 0, exactly one completed `documentation` event from the target path, no other tool event, and all six sections including link checks, executable-example validation, review cadence, and a no-repository-inspection statement.

## Use

```text
Use documentation. Plan a Chinese-first README, API reference, architecture, operations, troubleshooting, and release documentation set for this repository. State audiences and evidence gaps first, then information architecture, ownership, and automatable quality gates.
```

## Update

Check the pin using `git -C $source rev-parse HEAD`. Before changing it, re-audit the license and all of `skills/documentation`, then repeat both project tests, user scope, and rollback.

## Uninstall

Resolve absolute paths, remove only `$root/skills/documentation` and `$root/vendor/agentic-awesome-documentation`, and confirm the old path is absent with `codearts debug skill`. Never remove all of `.codeartsdoer`, root configuration, or another Skill.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | release `v17.2.0`; commit `2fce708d4f3871ccce3d705b4748371f94c730ec` |
| Skill / SHA-256 | `documentation` / `DCF5CE03E82B0ACBABE2C2DDF3B8C217FB35989E931F9F211B1A9ADBE03A7FFC` |
| Content license | CC BY 4.0 (AAS `LICENSE-CONTENT`) |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scopes | two clean projects plus user scope; 2026-09-14 |

## Limitations

The test covered read-only planning, not generation or modification of real documentation. It did not validate the other composed Skills, documentation sites, or CI mentioned in the body. The three runs proposed different tools and layouts; implementation must follow the actual repository.

## Security

The pinned directory contains one 5,930-byte `SKILL.md` and no dependency, lockfile, lifecycle script, binary, downloader, or telemetry. All three runs emitted only the target Skill event with no file, shell, or network access.

## Evidence and sources

- [English research](../../research/2026-09-14.en.md) · [中文](../../research/2026-09-14.md)
- [Pinned Skill directory](https://github.com/sickn33/agentic-awesome-skills/tree/2fce708d4f3871ccce3d705b4748371f94c730ec/skills/documentation)
- [AAS content license](https://github.com/sickn33/agentic-awesome-skills/blob/2fce708d4f3871ccce3d705b4748371f94c730ec/LICENSE-CONTENT)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
