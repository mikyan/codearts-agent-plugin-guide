# Use deep-research-framework in CodeArts CLI

[简体中文](README.md)

Install `deep-research-framework` so CodeArts defines the question and source tiers before separating facts, inferences, and uncertainty in a conclusion-first outline.

## Choose an installation scope

Project scope affects one repository; user scope is available across the current user's projects. Choose one. Project scope takes priority over a same-named user Skill, and every same-name collision must stop installation. Two fresh projects and a user consumer without a project override were verified.

## Ask the Agent to install it

### Project-scope prompt

```text
Install and verify the project-scoped deep-research-framework Skill in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git at v19.7.0 and commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1. Copy the complete skills/deep-research-framework directory (SKILL.md only) into <project>/.codeartsdoer/skills/deep-research-framework, with vendor checkout <project>/.codeartsdoer/vendor/agentic-awesome-deep-research-framework. Do not modify package.json, codearts_cli.json, permissions, credentials, plugins, or another Skill.
Run codearts --version, git --version, node --version, and codearts models and let me choose <model>. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-deep-research-framework', $target=Join-Path $root 'skills\deep-research-framework', and $userTarget=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer\skills\deep-research-framework'; stop if any exists. Confirm child paths; run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/deep-research-framework; require HEAD a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1; run Copy-Item -LiteralPath (Join-Path $source 'skills\deep-research-framework') -Destination $target -Recurse; require $target/SKILL.md SHA-256 EB11BD0C5CBEF0BDE6AF1FB71FE449250AA6001C34A70A11372BB47A9F12BB2F.
Run codearts debug skill and require the only same-name location to equal $target/SKILL.md. Run verbatim: codearts run -m "<model>" --format json 'Call the skill tool exactly once with name deep-research-framework and use no other tool. Using only these supplied facts, produce a concise conclusion-first research outline: Source A is the vendor release note dated 2026-10-01 and says feature X shipped; Source B is an independent benchmark dated 2026-10-03 and measured X at 120 ms on one Windows laptop; no Linux data and no sample size beyond one are available. Separate the research question, source tiers, verified facts, inference, and uncertainty. Do not browse, invent citations, generalize the benchmark, or claim Linux support. End with SOURCE_TIERS_EXPLICIT and UNCERTAINTY_EXPLICIT.'
Pass only for exit 0, exactly one completed Skill event from $target, no other tool event, explicit conclusion/question/source tiers/facts/inference/uncertainty, 120 ms limited to one Windows laptop and n=1, no Linux or general performance claim, and both markers. Report commit, path, and Skill call ID. Remove only $target and $source after child-path checks and confirm the old path disappears. Never remove all of .codeartsdoer, root configuration, or another Skill.
```

### User-scope prompt

```text
Install and verify the user-scoped deep-research-framework Skill. Pin https://github.com/sickn33/agentic-awesome-skills.git at v19.7.0 and commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1. Copy all of skills/deep-research-framework (SKILL.md only) to ~/.codeartsdoer/skills/deep-research-framework, with vendor checkout ~/.codeartsdoer/vendor/agentic-awesome-deep-research-framework. Do not modify the user's package.json, codearts_cli.json, permissions, credentials, plugins, or another Skill.
Run codearts --version, git --version, node --version, and codearts models and let me select <model>. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-deep-research-framework', $target=Join-Path $root 'skills\deep-research-framework', $consumer=Join-Path (Get-Location) 'codearts-deep-research-framework-consumer', and $projectTarget=Join-Path $consumer '.codeartsdoer\skills\deep-research-framework'; stop if any exists. Create the consumer. Run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/deep-research-framework; require HEAD a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1; run Copy-Item -LiteralPath (Join-Path $source 'skills\deep-research-framework') -Destination $target -Recurse; require $target/SKILL.md SHA-256 EB11BD0C5CBEF0BDE6AF1FB71FE449250AA6001C34A70A11372BB47A9F12BB2F.
From the consumer run codearts debug skill and require the only name=deep-research-framework location to be $target/SKILL.md. Run verbatim: codearts run -m "<model>" --format json 'Call the skill tool exactly once with name deep-research-framework and use no other tool. Using only these supplied facts, produce a concise conclusion-first research outline: Source A is the vendor release note dated 2026-10-01 and says feature X shipped; Source B is an independent benchmark dated 2026-10-03 and measured X at 120 ms on one Windows laptop; no Linux data and no sample size beyond one are available. Separate the research question, source tiers, verified facts, inference, and uncertainty. Do not browse, invent citations, generalize the benchmark, or claim Linux support. End with SOURCE_TIERS_EXPLICIT and UNCERTAINTY_EXPLICIT.' Pass only for exit 0, one completed Skill event from $target, no other tool event, explicit conclusion/question/source tiers/facts/inference/uncertainty, 120 ms limited to one Windows laptop and n=1, no Linux or general performance claim, and both markers. Report the call ID. Remove only $target, $source, and that exact consumer, then confirm removal without a project override. Never remove the user profile, all of .codeartsdoer, root configuration, another Skill, or an existing project.
```

## Manual installation on Windows

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer'
# User scope: $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-deep-research-framework'
$target=Join-Path $root 'skills\deep-research-framework'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/deep-research-framework
if((git -C $source rev-parse HEAD).Trim() -ne 'a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\deep-research-framework') -Destination $target -Recurse
if((Get-FileHash -Algorithm SHA256 -LiteralPath (Join-Path $target 'SKILL.md')).Hash -ne 'EB11BD0C5CBEF0BDE6AF1FB71FE449250AA6001C34A70A11372BB47A9F12BB2F'){throw 'Skill hash mismatch'}
```

## CodeArts configuration

Choose a model with `codearts models`. This Markdown-only Skill needs no dependency, plugin, credential, or configuration edit. The reproducible smoke test forbids the network and needs no `--auto`; real online research requires separate authorization and source review.

## Verify

Check the absolute location, then run the prompt command. Require one completed Skill event and a real separation of facts, inference, and uncertainty; a prose restatement is insufficient.

## Use

```text
Use deep-research-framework. First define the research question and source tiers. Cross-check primary claims where evidence permits, label every inference, and finish with explicit unknowns and invalidation conditions. Never invent a citation.
```

## Update

Pin and review the new release/commit, then repeat all three scopes and exact rollback. Do not follow floating `main`.

## Remove

First use `[IO.Path]::GetFullPath()` to confirm both paths are children of `$root`. Run exactly `Remove-Item -LiteralPath (Join-Path $root 'skills\deep-research-framework') -Recurse -Force` and `Remove-Item -LiteralPath (Join-Path $root 'vendor\agentic-awesome-deep-research-framework') -Recurse -Force`, then run `codearts debug skill` and require the old path to disappear. Never delete all of `.codeartsdoer`, root configuration, another Skill, a project, or user files.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | Agentic Awesome `v19.7.0`; commit `a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1` |
| File / SHA-256 | `SKILL.md` / `EB11BD0C5CBEF0BDE6AF1FB71FE449250AA6001C34A70A11372BB47A9F12BB2F` |
| License | MIT; source `alapha888/agent-skills-en` |
| Environment | CodeArts CLI 26.8.1; Windows 11 Build 26200; PowerShell 7.6.6; Git 2.53.0.windows.2; Node 24.14.1; npm 11.11.0; `mimo/mimo-v2.5` |
| Scope | Project A, fresh project B, and user consumer without a project override; 2026-10-16 |
| Core Skill calls | `call_f830a512792d47199e9bac1c`, `call_66fe7dcb9f2245f09b31d015`, `call_97762e6bd1764e8d953523ff` |

## Known limitations

The test used two supplied facts with networking disabled. It proves report structure and uncertainty discipline, not live retrieval, citation availability, paid databases, long reports, or expert conclusions. The Skill cannot guarantee a second independent source exists.

## Security

The pinned directory contains only `SKILL.md`; the calls used no other tool, network, write, credential, or telemetry action. User baseline hashes matched after rollback.

## Evidence and sources

- [中文研究记录](../../research/2026-10-16.md) · [English research](../../research/2026-10-16.en.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1/skills/deep-research-framework)
- [Original repository](https://github.com/alapha888/agent-skills-en)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
