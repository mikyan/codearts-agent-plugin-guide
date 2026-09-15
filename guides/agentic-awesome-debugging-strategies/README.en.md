# Use debugging-strategies with CodeArts CLI

[简体中文](README.md)

Install `debugging-strategies` to structure diagnosis around reproduction, observations, hypotheses, controlled experiments, root cause, and verification.

## Choose a scope

Use `<project>/.codeartsdoer` for one repository or `~/.codeartsdoer` across projects. Pick one; project scope wins on a same name. Stop on any Skill or vendor collision.

## Copy-ready Agent prompts

### Project scope

```text
Install and verify project-scoped debugging-strategies in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.2.0, commit 2fce708d4f3871ccce3d705b4748371f94c730ec, directory skills/debugging-strategies.
Run codearts --version, git --version, and codearts models, then let me select a real <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-debugging-strategies', and $target=Join-Path $root 'skills\debugging-strategies'. Check $source, $target, and ~/.codeartsdoer/skills/debugging-strategies; stop on collision. Create only $source and $target. Do not change package.json, codearts_cli.json, credentials, plugins, or another Skill.
From the project root run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/debugging-strategies/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'; git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec. Require git -C $source rev-parse HEAD to equal that commit, then run Copy-Item -LiteralPath (Join-Path $source 'skills\debugging-strategies') -Destination $target -Recurse.
Run codearts debug skill; the sole active debugging-strategies location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name debugging-strategies. Use no other tool, do not access the network, and do not read or write files. Analyze only this synthetic evidence: formatPrice(12.30) returns "$12.3" on three identical runs; trace shows it concatenates "$" + amount.toString(); expected is always two decimal places; no fix has been applied. Return exact sections REPRODUCTION, OBSERVATIONS, HYPOTHESES, CONTROLLED EXPERIMENT, ROOT CAUSE, FIX VERIFICATION, and LIMITS. Identify toString formatting as the leading root cause, propose a controlled test using 12, 12.3, and 12.345, and do not claim the test ran.'
Pass only on exit 0, exactly one name=debugging-strategies/status=completed event from $target, no other tool event, all seven sections and requirements, and an explicit statement that the experiment was not run. Report commit, paths, and events. Uninstall only exact $target and $source and verify removal with codearts debug skill. Never delete all of .codeartsdoer or root configuration.
```

### User scope

```text
Install and verify debugging-strategies for the current Windows user from https://github.com/sickn33/agentic-awesome-skills.git release v17.2.0, commit 2fce708d4f3871ccce3d705b4748371f94c730ec, directory skills/debugging-strategies.
Run codearts --version, git --version, and codearts models, then let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-debugging-strategies', and $target=Join-Path $root 'skills\debugging-strategies'. Confirm $source, $target, and .codeartsdoer/skills/debugging-strategies in a fresh consumer are absent; stop on collision. Do not change user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/debugging-strategies/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'; git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec; verify HEAD, then Copy-Item -LiteralPath (Join-Path $source 'skills\debugging-strategies') -Destination $target -Recurse.
Enter a fresh consumer with no project override. The sole codearts debug skill location must be $target/SKILL.md. Run the exact smoke test from the project prompt with the selected <model> and apply the same success criteria. Remove only exact $target, $source, and the verified consumer; never remove the user root, root configuration, credentials, or another Skill.
```

## Manual Windows installation

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-debugging-strategies'; $target=Join-Path $root 'skills\debugging-strategies'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/debugging-strategies/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'
git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec
if((git -C $source rev-parse HEAD).Trim() -ne '2fce708d4f3871ccce3d705b4748371f94c730ec'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\debugging-strategies') -Destination $target -Recurse
```

## CodeArts configuration

Run `codearts --version`, `git --version`, and `codearts models`; select an available `provider/model`. No package or CodeArts JSON edit is required.

## Verification

Confirm the sole source with `codearts debug skill`, then run the complete smoke test. Require one completed target Skill event, no extra tools, the root-cause chain, three controlled inputs, and a no-execution statement.

## Use

```text
Use debugging-strategies. Based only on my reproduction, logs, and trace, separate observations, hypotheses, controlled experiments, root cause, and fix verification. Mark unknowns as evidence gaps and never claim an unrun test passed.
```

## Update

Check `git -C $source rev-parse HEAD`; audit the license and complete directory before changing the pin, then repeat both projects, user scope, and rollback.

## Uninstall

Remove only `$root/skills/debugging-strategies` and `$root/vendor/agentic-awesome-debugging-strategies`, then confirm the old path is absent.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | `v17.2.0` / `2fce708d4f3871ccce3d705b4748371f94c730ec` |
| Skill / SHA-256 | `debugging-strategies` / `0AA1A6597A9B97EBEF24054C6DB8083D65640358EA0EB8F228AB8D0718247365` |
| Content license | CC BY 4.0 |
| Environment and scopes | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5`; two projects plus user; 2026-09-15 |

## Limitations

The test analyzed synthetic inline evidence only; it did not read real logs, run experiments, edit code, or open the detailed playbook. Any `toFixed(2)` change still needs monetary-precision and localization review.

## Security

The pinned directory has two Markdown files totaling 14,047 bytes and no dependency, script, binary, downloader, or telemetry. All three calls emitted only the target Skill event.

## Evidence and sources

- [中文 research](../../research/2026-09-15.md) · [English](../../research/2026-09-15.en.md)
- [Pinned Skill directory](https://github.com/sickn33/agentic-awesome-skills/tree/2fce708d4f3871ccce3d705b4748371f94c730ec/skills/debugging-strategies)
- [Content license](https://github.com/sickn33/agentic-awesome-skills/blob/2fce708d4f3871ccce3d705b4748371f94c730ec/LICENSE-CONTENT)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
