# Use andrej-karpathy in CodeArts CLI

[简体中文](README.md)

Install `andrej-karpathy` so the Agent states assumptions before coding, keeps changes surgical, and turns work into verifiable success criteria.

## Choose an installation scope

Use project scope at `<project>/.codeartsdoer` for one repository or user scope at `~/.codeartsdoer` across projects. Choose one. CodeArts documents project precedence for same-name Skills; stop instead of overwriting an existing Skill, vendor checkout, or other-scope collision.

## Ask an Agent to install it

### Project-scope prompt

```text
Install and verify project-scope andrej-karpathy in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v18.2.0, commit 3bc6d8121ed9eeac844e6a652989251f883cb72a, copy source skills/andrej-karpathy, target <project>/.codeartsdoer/skills/andrej-karpathy, and isolated vendor directory <project>/.codeartsdoer/vendor/agentic-awesome-andrej-karpathy. From the project root run codearts --version, git --version, and codearts models, then let me select a real <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-andrej-karpathy', and $target=Join-Path $root 'skills\andrej-karpathy'. Stop if $source, $target, or ~/.codeartsdoer/skills/andrej-karpathy exists. Do not modify package.json, codearts_cli.json, credentials, plugins, or other Skills.
From the project root run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 3bc6d8121ed9eeac844e6a652989251f883cb72a; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/andrej-karpathy/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 3bc6d8121ed9eeac844e6a652989251f883cb72a. Require git -C $source rev-parse HEAD to equal that commit, then Copy-Item -LiteralPath (Join-Path $source 'skills\andrej-karpathy') -Destination $target -Recurse.
Run codearts debug skill and require the sole name=andrej-karpathy location to be $target/SKILL.md. Replace <model>, then run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name andrej-karpathy and use no other tool. Do not access files, commands, or the network. Apply the skill to this complete synthetic task: Add email validation to src/signup.js; validation must reject blank or missing-at-sign input before submit; existing valid submit behavior must remain unchanged; only src/signup.js and test/signup.test.js may change; the existing test command is npm test. Return explicit assumptions, the simplest surgical approach, a three-step verb-first plan where every step has a verification check, exact in-scope and out-of-scope files, and measurable success criteria. Do not implement code and do not invent repository facts.' Pass only on exit 0, exactly one name=andrej-karpathy/status=completed event resolved from $target, no other tool event, and a final answer containing assumptions, minimal approach, three verified steps, exact file scope, and success criteria. Report the commit, absolute path, and event. Remove only exact $target and $source, then confirm the old path is absent from codearts debug skill. Never remove all of .codeartsdoer or root configuration.
```

### User-scope prompt

```text
Install and verify user-scope andrej-karpathy for the current Windows user. Pin https://github.com/sickn33/agentic-awesome-skills.git release v18.2.0, commit 3bc6d8121ed9eeac844e6a652989251f883cb72a, copy source skills/andrej-karpathy, target ~/.codeartsdoer/skills/andrej-karpathy, and isolated vendor directory ~/.codeartsdoer/vendor/agentic-awesome-andrej-karpathy. Run codearts --version, git --version, and codearts models and let me select a real <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-andrej-karpathy', and $target=Join-Path $root 'skills\andrej-karpathy'. Stop if $source, $target, or .codeartsdoer/skills/andrej-karpathy in the fresh consumer exists. Do not modify the user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 3bc6d8121ed9eeac844e6a652989251f883cb72a; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/andrej-karpathy/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 3bc6d8121ed9eeac844e6a652989251f883cb72a. Require HEAD to match exactly, then Copy-Item -LiteralPath (Join-Path $source 'skills\andrej-karpathy') -Destination $target -Recurse. From a fresh consumer without a project-level override, run codearts debug skill and require the sole name=andrej-karpathy location to be $target/SKILL.md. Replace <model>, then run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name andrej-karpathy and use no other tool. Do not access files, commands, or the network. Apply the skill to this complete synthetic task: Add email validation to src/signup.js; validation must reject blank or missing-at-sign input before submit; existing valid submit behavior must remain unchanged; only src/signup.js and test/signup.test.js may change; the existing test command is npm test. Return explicit assumptions, the simplest surgical approach, a three-step verb-first plan where every step has a verification check, exact in-scope and out-of-scope files, and measurable success criteria. Do not implement code and do not invent repository facts.' Pass only on exit 0, exactly one target name=andrej-karpathy/status=completed event, no other tool event, and all five required result categories. Report the commit, absolute path, and event. Remove only exact $target, $source, and the verified consumer, then confirm the old path is absent from codearts debug skill. Never remove the user root, root configuration, credentials, or other Skills.
```

## Manual Windows installation

First run `codearts --version`, `git --version`, and `codearts models`. The example defaults to project scope; only `$root` changes for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-andrej-karpathy'
$target=Join-Path $root 'skills\andrej-karpathy'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 3bc6d8121ed9eeac844e6a652989251f883cb72a
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/andrej-karpathy/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 3bc6d8121ed9eeac844e6a652989251f883cb72a
if((git -C $source rev-parse HEAD).Trim() -ne '3bc6d8121ed9eeac844e6a652989251f883cb72a'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\andrej-karpathy') -Destination $target -Recurse
```

## CodeArts configuration

Choose a working `provider/model` with `codearts models`. This content-only Skill has no runtime dependency and needs no `package.json` or `codearts_cli.json` change.

## Verification

Use `codearts debug skill` to check the sole absolute path, then run the full `codearts run` command from the prompt above. Require exactly one completed target Skill event, no other tool event, and all five planning result categories.

## Usage

```text
Use andrej-karpathy. Before coding, list assumptions and the minimum file scope, turn each step into a verifiable success criterion, and do not refactor adjacent code.
```

## Update

Before changing the pinned commit, re-audit the Skill and license and repeat two fresh projects, the user-scope consumer, the core invocation, and rollback.

## Uninstall

Remove only `$root/skills/andrej-karpathy` and `$root/vendor/agentic-awesome-andrej-karpathy`, then confirm the former absolute path is absent from `codearts debug skill`.

## Verified result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | Agentic Awesome `v18.2.0`; commit `3bc6d8121ed9eeac844e6a652989251f883cb72a` |
| Skill / SHA-256 | `andrej-karpathy` / `FA7CD608E04C663477C452689EAB11207E9CCDA92AB3F689F780539910330FE8` |
| License | Skill frontmatter: MIT |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scope | Two fresh projects plus a user-scope consumer without a project override; 2026-09-23 |

## Known limitations

This is a behavioral guardrail. It does not read repository facts, run tests, or replace project architecture rules by itself. The verification used a constrained synthetic planning task; real edits still require explicit authorization and repository-specific validation.

## Security

The pinned directory contains one 4,264-byte `SKILL.md` and no manifest, lockfile, script, binary, downloader, network call, credential, or telemetry. All six final main/publication invocations emitted only the target Skill event.

## Evidence and sources

- [English research](../../research/2026-09-23.en.md) · [中文](../../research/2026-09-23.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/3bc6d8121ed9eeac844e6a652989251f883cb72a/skills/andrej-karpathy)
- [Original project](https://github.com/multica-ai/andrej-karpathy-skills)
- [Agentic Awesome v18.2.0](https://github.com/sickn33/agentic-awesome-skills/releases/tag/v18.2.0)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
