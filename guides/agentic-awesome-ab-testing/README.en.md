# Use ab-testing with CodeArts CLI

[简体中文](README.md)

Install `ab-testing` to design A/B tests with reproducible sample sizes, hypotheses, primary metrics, and guardrails.

## Choose a scope

Choose exactly one: project scope at `<project>/.codeartsdoer` for a repository pin, or user scope at `~/.codeartsdoer` for cross-project reuse by the current Windows user. CLI 26.8.1 locally disagreed with the documented collision priority, so stop if either scope already contains `ab-testing`.

## Ask Agent to install it

### Project prompt

```text
Install and verify project-scoped ab-testing from sickn33/agentic-awesome-skills at commit e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5.
1. Create only <project>/.codeartsdoer/vendor/agentic-awesome-ab-testing and <project>/.codeartsdoer/skills/ab-testing. Do not change ~/.codeartsdoer, package.json, codearts_cli.json, credentials, or other skills.
2. Run codearts --version, git --version, and codearts models; ask me to select an available provider/model. Stop on missing credentials without reading secrets.
3. Check both targets and ~/.codeartsdoer/skills/ab-testing; stop on any collision.
4. From the project root run in PowerShell:
   $source=Join-Path (Get-Location) '.codeartsdoer\vendor\agentic-awesome-ab-testing'; $target=Join-Path (Get-Location) '.codeartsdoer\skills\ab-testing'
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
   git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
   git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/plugins/agentic-awesome-skills-claude/skills/ab-testing/' '/LICENSE'
   git -C $source checkout --detach e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
   if((git -C $source rev-parse HEAD).Trim() -ne 'e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $source 'plugins\agentic-awesome-skills-claude\skills\ab-testing') -Destination $target -Recurse
5. Run codearts debug skill; the only ab-testing location must be the project target SKILL.md.
6. Replace <model> and run verbatim: codearts run --format json --sandbox --model "<model>" "Verification contract: call the skill tool exactly once with name ab-testing. No other tool is allowed. Using the loaded skill, design a signup-page A/B test with 5% baseline, 20% relative MDE, 2,000 eligible visitors/day, and 50/50 split. State exactly 18,000 per variant and 18 days, one hypothesis, one primary metric, and one guardrail metric."
7. Pass only on exit 0, exactly one completed ab-testing skill event, no other tools, and every requested figure and section.
8. Report commit, source, event, result, and removal list. Remove only target and source; never delete all .codeartsdoer, package.json, ProjectSkillStatus.txt, or another skill. Stop truthfully on failure.
```

### User prompt

```text
Install and verify user-scoped ab-testing for the current Windows user from commit e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5.
1. Create only ~/.codeartsdoer/vendor/agentic-awesome-ab-testing and ~/.codeartsdoer/skills/ab-testing. Do not change the root package.json, codearts_cli.json, credentials, plugins, or projects.
2. Run codearts --version, git --version, and codearts models and ask me to select an available model. Stop on missing credentials. Confirm both targets and the consumer's project skill path are absent; stop on collision.
3. Run in PowerShell:
   $u=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'; $source=Join-Path $u 'vendor\agentic-awesome-ab-testing'; $target=Join-Path $u 'skills\ab-testing'; $consumer=Join-Path ([IO.Path]::GetTempPath()) 'codearts-ab-testing-e2b6ad1-smoke'
   if((Test-Path $source)-or(Test-Path $target)-or(Test-Path $consumer)){throw 'Collision'}
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent),$consumer -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
   git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
   git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/plugins/agentic-awesome-skills-claude/skills/ab-testing/' '/LICENSE'
   git -C $source checkout --detach e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
   if((git -C $source rev-parse HEAD).Trim() -ne 'e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $source 'plugins\agentic-awesome-skills-claude\skills\ab-testing') -Destination $target -Recurse; Set-Location $consumer
4. Run codearts debug skill; the only match must be the user target. Replace <model> and run: codearts run --format json --sandbox --model "<model>" "Verification contract: call the skill tool exactly once with name ab-testing. No other tool is allowed. Using the loaded skill, design a signup-page A/B test with 5% baseline, 20% relative MDE, 2,000 eligible visitors/day, and 50/50 split. State exactly 18,000 per variant and 18 days, one hypothesis, one primary metric, and one guardrail metric."
5. Pass only on exit 0, exactly one completed ab-testing skill event, no other tools, and every requested figure and section. Remove only $target, $source, and the verified $consumer. Never delete the user root, package.json, codearts_cli.json, credentials, or another skill.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models`. Set `$root` to the project or user `.codeartsdoer`, choose one, check both scopes for collisions, then execute the exact clone, sparse-checkout, commit check, and copy commands from the matching prompt.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # User: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-ab-testing'; $target=Join-Path $root 'skills\ab-testing'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force|Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/plugins/agentic-awesome-skills-claude/skills/ab-testing/' '/LICENSE'
git -C $source checkout --detach e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5
Copy-Item -LiteralPath (Join-Path $source 'plugins\agentic-awesome-skills-claude\skills\ab-testing') -Destination $target -Recurse
```

## CodeArts configuration

Choose an available model with `codearts models`; configure credentials from official documentation and never store them here.

## Verification

Run `codearts debug skill`, then the prompt's real `codearts run` command. Its tool-event and content assertions are mandatory.

## Use

```text
Use ab-testing. Baseline conversion is 8%; clarify MDE, traffic, and guardrails before proposing the experiment.
```

## Update

Check the pin with `git -C $source rev-parse HEAD`. Re-audit and repeat all three environments for any new commit.

## Remove

Remove only `<scope>/.codeartsdoer/skills/ab-testing` and `<scope>/.codeartsdoer/vendor/agentic-awesome-ab-testing`, then confirm it is absent from `codearts debug skill`.

## Verified compatibility

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | release `v15.15.0`; commit `e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5` |
| Skill / SHA-256 | `ab-testing` / `4594DD8DE675AC8B383032E58D8954BCAA45E2EC26CC637E3817A7C381C6E8AF` |
| License | MIT |
| Environment | CodeArts CLI 26.8.1; Windows 11 Build 26200; `mimo/mimo-v2.5` |
| Scopes | Two clean projects plus user scope; 2026-08-20 |

## Known limitations

Only this skill was tested. Its sample-size table does not replace professional statistical review. Trust `codearts debug skill` for collision resolution.

## Security

The pinned directory contains four text files totaling 34,491 bytes and runs no npm, lifecycle script, binary, runtime network, credential, or telemetry step.

## Evidence and sources

- [中文研究记录](../../research/2026-08-20.md) · [English](../../research/2026-08-20.en.md)
- [Pinned commit](https://github.com/sickn33/agentic-awesome-skills/tree/e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5/plugins/agentic-awesome-skills-claude/skills/ab-testing)
- [MIT License](https://github.com/sickn33/agentic-awesome-skills/blob/e2b6ad15c704e47819b0e2393d04b42a9dcf4fc5/LICENSE)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
