# Use code-review-excellence with CodeArts CLI

[简体中文](README.md)

Install `code-review-excellence` to review code with structured severities, suggested changes, test notes, and explicit evidence boundaries.

## Choose a scope

Use `<project>/.codeartsdoer` for one repository or `~/.codeartsdoer` for cross-project reuse. Pick one; a same-named project Skill takes priority. Stop if either target or a same-named Skill in the other scope exists.

## Copy-ready Agent prompts

### Project scope

```text
Install and verify project-scoped code-review-excellence in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.2.0, commit 2fce708d4f3871ccce3d705b4748371f94c730ec, directory skills/code-review-excellence.
Run codearts --version, git --version, and codearts models, then let me select a real <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-code-review-excellence', and $target=Join-Path $root 'skills\code-review-excellence'. Check $source, $target, and ~/.codeartsdoer/skills/code-review-excellence; stop on any collision. Create only $source and $target. Do not change package.json, codearts_cli.json, credentials, plugins, or another Skill.
From the project root run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/code-review-excellence/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'; git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec. Require git -C $source rev-parse HEAD to equal that commit, then run Copy-Item -LiteralPath (Join-Path $source 'skills\code-review-excellence') -Destination $target -Recurse.
Run codearts debug skill; the sole active code-review-excellence location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name code-review-excellence. Use no other tool, do not access the network, and do not read or write files. Review only this inline JavaScript: async function get(id){try{return await db.user.findUnique({where:{id}})}catch(e){return null}}. The contract requires database failures to remain distinguishable from a missing user. Return exact sections SUMMARY, BLOCKING, IMPORTANT, MINOR, SUGGESTED CHANGE, TEST NOTES, and QUESTIONS. Flag swallowed database errors as blocking, explain why null is ambiguous, suggest a typed or explicit error boundary, and do not invent repository context.'
Pass only on exit 0, exactly one name=code-review-excellence/status=completed event from $target, no other tool event, and seven requested sections or unambiguous semantic equivalents satisfying every requirement. Report commit, absolute paths, and events. Uninstall only exact $target and $source, then confirm the old path is absent with codearts debug skill. Never delete all of .codeartsdoer, root configuration, or another Skill.
```

### User scope

```text
Install and verify code-review-excellence for the current Windows user from https://github.com/sickn33/agentic-awesome-skills.git release v17.2.0, commit 2fce708d4f3871ccce3d705b4748371f94c730ec, directory skills/code-review-excellence.
Run codearts --version, git --version, and codearts models, then let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-code-review-excellence', and $target=Join-Path $root 'skills\code-review-excellence'. Confirm $source, $target, and .codeartsdoer/skills/code-review-excellence in a fresh consumer are absent; stop on collision. Do not change user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/code-review-excellence/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'; git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec; verify HEAD, then Copy-Item -LiteralPath (Join-Path $source 'skills\code-review-excellence') -Destination $target -Recurse.
Enter a fresh consumer with no project override. The sole codearts debug skill location must be $target/SKILL.md. Replace <model> and run the exact codearts run command from the project prompt. Apply the same success criteria. Remove only exact $target, $source, and the verified consumer; never remove the user root, root configuration, credentials, or another Skill.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models`. This defaults to project scope; replace only `$root` for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-code-review-excellence'; $target=Join-Path $root 'skills\code-review-excellence'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 2fce708d4f3871ccce3d705b4748371f94c730ec
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/code-review-excellence/' '/LICENSE' '/LICENSE-CONTENT' '/TERMS.md'
git -C $source checkout --detach 2fce708d4f3871ccce3d705b4748371f94c730ec
if((git -C $source rev-parse HEAD).Trim() -ne '2fce708d4f3871ccce3d705b4748371f94c730ec'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\code-review-excellence') -Destination $target -Recurse
```

## CodeArts configuration

Choose an available `provider/model` with `codearts models`. Keep credentials only in documented user configuration. This Skill needs no `package.json` or `codearts_cli.json` change.

## Verification

Use `codearts debug skill` to confirm the sole source, then run the complete command in the Agent prompt. Success requires exit 0, exactly one completed target Skill event, no other tool event, and a review that distinguishes database failure from a missing user.

## Use

```text
Use code-review-excellence. Review only this change, group findings as blocking, important, and minor, and give evidence, impact, suggested changes, and tests without inventing repository facts.
```

## Update

Check the pin with `git -C $source rev-parse HEAD`. Before changing it, audit the license and the full Skill directory, then repeat both project tests, user scope, and rollback.

## Uninstall

Resolve absolute paths, remove only `$root/skills/code-review-excellence` and `$root/vendor/agentic-awesome-code-review-excellence`, and confirm the old path is absent with `codearts debug skill`.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | release `v17.2.0`; commit `2fce708d4f3871ccce3d705b4748371f94c730ec` |
| Skill / SHA-256 | `code-review-excellence` / `B5141711AFADAD742F31E576BD45D6A68E9D259425B064BF1352BCB19DADF49A` |
| Content license | CC BY 4.0 (AAS `LICENSE-CONTENT`) |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scopes | two clean projects plus user scope; 2026-09-15 |

## Limitations

Testing covered read-only inline-code review, not a real repository, PR, test suite, or the detailed playbook. Heading wording and suggested changes varied across scopes; users must reconcile advice with project error types and caller contracts.

## Security

The pinned directory has two Markdown files totaling 15,670 bytes and no dependency, lockfile, lifecycle script, binary, downloader, or telemetry. All three runs emitted only the target Skill event.

## Evidence and sources

- [中文 research](../../research/2026-09-15.md) · [English](../../research/2026-09-15.en.md)
- [Pinned Skill directory](https://github.com/sickn33/agentic-awesome-skills/tree/2fce708d4f3871ccce3d705b4748371f94c730ec/skills/code-review-excellence)
- [AAS content license](https://github.com/sickn33/agentic-awesome-skills/blob/2fce708d4f3871ccce3d705b4748371f94c730ec/LICENSE-CONTENT)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
