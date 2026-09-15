# Use testing-patterns with CodeArts CLI

[简体中文](README.md)

Install `testing-patterns` to design Jest tests around behavior, boundary values, and small override-friendly factories.

## Choose a scope

Use `<project>/.codeartsdoer` for one repository or `~/.codeartsdoer` across projects. Pick one; project scope wins on a same name. Stop on any Skill or vendor collision.

## Copy-ready Agent prompts

### Project scope

```text
Install and verify project-scoped testing-patterns in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.2.0, commit 2fce708d4f3871ccce3d705b4748371f94c730ec, directory skills/testing-patterns.
Run codearts --version, git --version, and codearts models, then let me select a real <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-testing-patterns', and $target=Join-Path $root 'skills\testing-patterns'. Check $source, $target, and ~/.codeartsdoer/skills/testing-patterns; stop on collision. Create only $source and $target. Do not change package.json, codearts_cli.json, credentials, plugins, or another Skill.
From the project root run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/testing-patterns/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'; git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec. Require git -C $source rev-parse HEAD to equal that commit, then run Copy-Item -LiteralPath (Join-Path $source 'skills\testing-patterns') -Destination $target -Recurse.
Run codearts debug skill; the sole active testing-patterns location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name testing-patterns. Use no other tool, do not access the network, and do not read or write files. Design Jest tests for a pure function shipping(total, member): members pay 0; non-members pay 5 when total is below 50 and 0 otherwise. Return exact sections BEHAVIORS, TEST CASES, FACTORY, BOUNDARIES, ANTI-PATTERNS, and LIMITS. Cover totals 49.99 and 50, both member states, preserve behavior-focused assertions, use a small input factory with overrides, and state the tests were not executed.'
Pass only on exit 0, exactly one name=testing-patterns/status=completed event from $target, no other tool event, all six sections, both boundary values and member states, an override-friendly factory, and a no-execution statement. Report commit, paths, and events. Uninstall only exact $target and $source and confirm removal with codearts debug skill. Never delete all of .codeartsdoer or root configuration.
```

### User scope

```text
Install and verify testing-patterns for the current Windows user from https://github.com/sickn33/agentic-awesome-skills.git release v17.2.0, commit 2fce708d4f3871ccce3d705b4748371f94c730ec, directory skills/testing-patterns.
Run codearts --version, git --version, and codearts models, then let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-testing-patterns', and $target=Join-Path $root 'skills\testing-patterns'. Confirm $source, $target, and .codeartsdoer/skills/testing-patterns in a fresh consumer are absent; stop on collision. Do not change user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/testing-patterns/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'; git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec; verify HEAD, then Copy-Item -LiteralPath (Join-Path $source 'skills\testing-patterns') -Destination $target -Recurse.
Enter a fresh consumer with no project override. The sole codearts debug skill location must be $target/SKILL.md. Run the exact smoke test from the project prompt with the selected <model> and apply the same success criteria. Remove only exact $target, $source, and the verified consumer; never remove the user root, root configuration, credentials, or another Skill.
```

## Manual Windows installation

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-testing-patterns'; $target=Join-Path $root 'skills\testing-patterns'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/testing-patterns/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'
git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec
if((git -C $source rev-parse HEAD).Trim() -ne '2fce708d4f3871ccce3d705b4748371f94c730ec'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\testing-patterns') -Destination $target -Recurse
```

## CodeArts configuration

Run `codearts --version`, `git --version`, and `codearts models`; select an available `provider/model`. No root configuration edit is required.

## Verification

Confirm the sole source with `codearts debug skill`, then run the complete smoke test. Require one completed target Skill event, no extra tools, all boundaries and member states, an override-friendly factory, and a no-execution statement.

## Use

```text
Use testing-patterns. Design behavior-focused Jest tests for this pure function. List business behaviors and boundaries first, then minimal test cases and an override-friendly data factory, and state which tests have not run.
```

## Update

Check `git -C $source rev-parse HEAD`; audit the license and complete directory before changing the pin, then repeat both projects, user scope, and rollback.

## Uninstall

Remove only `$root/skills/testing-patterns` and `$root/vendor/agentic-awesome-testing-patterns`, then confirm the old path is absent.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | `v17.2.0` / `2fce708d4f3871ccce3d705b4748371f94c730ec` |
| Skill / SHA-256 | `testing-patterns` / `E71011053D6603A4A538ABBC546C14F3699D3CE0B5E27C8DE0E9994FCA4EA1E0` |
| Content license | CC BY 4.0 |
| Environment and scopes | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5`; two projects plus user; 2026-09-15 |

## Limitations

Testing covered read-only design for a pure function; it did not create files, run Jest, or validate project configuration. The Skill examples favor Jest and React Native; other frameworks need adapted syntax and fresh validation.

## Security

The pinned directory has one 6,603-byte `SKILL.md` and no dependency, script, binary, downloader, or telemetry. All three calls emitted only the target Skill event.

## Evidence and sources

- [中文 research](../../research/2026-09-15.md) · [English](../../research/2026-09-15.en.md)
- [Pinned Skill directory](https://github.com/sickn33/agentic-awesome-skills/tree/2fce708d4f3871ccce3d705b4748371f94c730ec/skills/testing-patterns)
- [Content license](https://github.com/sickn33/agentic-awesome-skills/blob/2fce708d4f3871ccce3d705b4748371f94c730ec/LICENSE-CONTENT)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
