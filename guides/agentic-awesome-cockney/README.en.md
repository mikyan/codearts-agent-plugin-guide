# Use cockney in CodeArts CLI

[简体中文](README.md)

Install the `cockney` Skill so CodeArts Agent can describe the smallest correct change in terse East End prose while switching back to clear plain English for security and destructive risks.

## Choose an installation scope

Project scope under `<project>/.codeartsdoer` affects one repository. User scope under `~/.codeartsdoer` is available across the current user's projects. Choose one. CodeArts documents project scope as taking priority over a same-named user Skill; stop on any same-name project, user, target-Skill, or vendor collision instead of overwriting it. This guide verified two fresh projects and a user-scope consumer without a project override.

## Ask the Agent to install it

### Project-scope prompt

```text
Install and verify the project-scoped cockney Skill in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git at release v19.7.0 and commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1. The complete copy source is skills/cockney (SKILL.md only), the vendor target is <project>/.codeartsdoer/vendor/agentic-awesome-cockney, and the native CodeArts target is <project>/.codeartsdoer/skills/cockney. Do not modify package.json, codearts_cli.json, permissions, credentials, plugins, or any other Skill.
From the project root, first run codearts --version, git --version, node --version, and codearts models, then let me select a real <model>. Do not read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-cockney', $target=Join-Path $root 'skills\cockney', and $userTarget=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer\skills\cockney'. If $source, $target, or $userTarget exists, stop and report the collision; do not overwrite it. Confirm that the absolute $source and $target paths are children of $root. From the project root run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/cockney; require (git -C $source rev-parse HEAD).Trim() to equal a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1 exactly; Copy-Item -LiteralPath (Join-Path $source 'skills\cockney') -Destination $target -Recurse; require $target/SKILL.md to exist with SHA-256 22093F5D83E02E9F69F8E98E94A91332F62140DAEF22A5A04E924A2F57872C91.
Run codearts debug skill from the project root. The only name=cockney location must be exactly $target/SKILL.md. Replace <model> with the selected provider/model ID and run verbatim: codearts run -m "<model>" --format json 'Call the skill tool exactly once with name cockney and use no other tool. In full mode, diagnose this supplied synthetic fact in at most four sentences: greeting in email.py uses user.name without a null fallback, so new users see Hello None; the minimal fix is Hello there and the email tests pass. Preserve the identifiers email.py, user.name, Hello None, and Hello there exactly. Then add one separate plain-English security warning sentence: deploy.sh drops the users table and requires backup confirmation. Do not invent edits or test commands. End with COCKNEY_MODE_PROVED and RISK_WARNING_PLAIN.'
Pass only if the command exits 0, emits exactly one completed Skill event from $target and no other tool events, preserves all four exact identifiers, uses Cockney for the engineering explanation, uses plain English for the security warning, and ends with both literal markers. Report the commit, absolute target, Skill call ID, and final assertions. After verification, confirm again that $target and $source are children of $root, remove only those two exact directories, then rerun codearts debug skill and require the former absolute path to be absent. Never remove all of .codeartsdoer, the project root, the user profile, root configuration, or any other file.
```

### User-scope prompt

```text
Install and verify the user-scoped cockney Skill for the current Windows user. Pin https://github.com/sickn33/agentic-awesome-skills.git at release v19.7.0 and commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1. The complete copy source is skills/cockney (SKILL.md only), the vendor target is ~/.codeartsdoer/vendor/agentic-awesome-cockney, and the native CodeArts target is ~/.codeartsdoer/skills/cockney. Do not modify the user package.json, codearts_cli.json, permissions, credentials, plugins, or any other Skill.
First run codearts --version, git --version, node --version, and codearts models, then let me select a real <model>. Do not read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-cockney', $target=Join-Path $root 'skills\cockney', and $consumer=Join-Path (Get-Location) 'codearts-cockney-consumer'. If $source, $target, or $consumer exists, stop and report the collision. Create the fresh $consumer and confirm it has no .codeartsdoer/skills/cockney; do not create a project-scoped same-name Skill there. Confirm that the absolute $source and $target paths are children of $root. Run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent),$consumer -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/cockney; require (git -C $source rev-parse HEAD).Trim() to equal a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1 exactly; Copy-Item -LiteralPath (Join-Path $source 'skills\cockney') -Destination $target -Recurse; require $target/SKILL.md to exist with SHA-256 22093F5D83E02E9F69F8E98E94A91332F62140DAEF22A5A04E924A2F57872C91.
Enter $consumer and run codearts debug skill. The only name=cockney location must be exactly $target/SKILL.md. Replace <model> with the selected provider/model ID and run verbatim: codearts run -m "<model>" --format json 'Call the skill tool exactly once with name cockney and use no other tool. In full mode, diagnose this supplied synthetic fact in at most four sentences: greeting in email.py uses user.name without a null fallback, so new users see Hello None; the minimal fix is Hello there and the email tests pass. Preserve the identifiers email.py, user.name, Hello None, and Hello there exactly. Then add one separate plain-English security warning sentence: deploy.sh drops the users table and requires backup confirmation. Do not invent edits or test commands. End with COCKNEY_MODE_PROVED and RISK_WARNING_PLAIN.'
Pass only if the command exits 0, emits exactly one completed Skill event from $target and no other tool events, preserves all four exact identifiers, uses Cockney for the engineering explanation, uses plain English for the security warning, and ends with both literal markers. Report the commit, absolute target, Skill call ID, and final assertions. After verification, check the exact paths and remove only $target, $source, and the exact $consumer created by this prompt. From a directory without a project override, rerun codearts debug skill and require the former path to be absent. Never remove the user profile, all of .codeartsdoer, root configuration, another Skill, or any existing project.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, `node --version`, and `codearts models` first. This example defaults to project scope; use the commented user root for user scope. Stop if either target exists.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer'
# User scope: $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-cockney'
$target=Join-Path $root 'skills\cockney'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/cockney
if((git -C $source rev-parse HEAD).Trim() -ne 'a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\cockney') -Destination $target -Recurse
if((Get-FileHash -Algorithm SHA256 -LiteralPath (Join-Path $target 'SKILL.md')).Hash -ne '22093F5D83E02E9F69F8E98E94A91332F62140DAEF22A5A04E924A2F57872C91'){throw 'Skill hash mismatch'}
```

For user-scope verification, start CodeArts from an independent consumer directory without `.codeartsdoer/skills/cockney`, so a project override cannot shadow the user Skill.

## CodeArts configuration

Use `codearts models` to choose a working `provider/model`. This Skill is one Markdown file and needs no npm or Python dependency, MCP server, Hook, plugin, credential, network call, or changes to `package.json`, `codearts_cli.json`, or permissions. Do not use `--auto`.

## Verify

Run `codearts debug skill` first and confirm the only `cockney` entry has the expected absolute `location`. Then run the exact `codearts run` command in the selected Agent prompt. Success requires exit 0; exactly one completed `skill` event from the intended path; no other tool events; exact preservation of `email.py`, `user.name`, `Hello None`, and `Hello there`; Cockney prose for the engineering explanation; plain English for the `deploy.sh`/`users` risk warning; and the final `COCKNEY_MODE_PROVED` and `RISK_WARNING_PLAIN` markers.

## Use

```text
Use cockney in full mode. Review this small bug and recommend the minimum correct fix. Keep identifiers, paths, commands, errors, and numbers exact. Switch to plain English for any security, destructive, irreversible, ordered, or clarifying content. Do not make changes unless I explicitly ask.
```

Say `speak proper`, `stop cockney`, `normal mode`, or `/cockney off` to stop the style. This installation contains only the Skill text, not the upstream Claude Hook, so cross-session persistence is unverified; restate the mode in a new session.

## Update

Before changing the release or commit, review `skills/cockney/SKILL.md` and the license source again, then repeat the two fresh projects, user-scope consumer without an override, representative invocation, and exact rollback. Do not replace the pin with floating `main`.

## Uninstall

At either scope, remove only the selected `$root/skills/cockney` and `$root/vendor/agentic-awesome-cockney`. Remove a consumer only after confirming it is the exact directory this guide created. Run `codearts debug skill` afterward and confirm the old absolute path is gone. Do not remove all of `.codeartsdoer`, the user profile, root configuration, or another Skill.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | Agentic Awesome `v19.7.0`; commit `a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1` |
| File / SHA-256 | `SKILL.md` / `22093F5D83E02E9F69F8E98E94A91332F62140DAEF22A5A04E924A2F57872C91` |
| License | MIT in Skill frontmatter; source `Kadajett/cockney` |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; PowerShell 7.6.6; Git 2.53.0.windows.2; Node 24.14.1; npm 11.11.0; `mimo/mimo-v2.5` |
| Scope | Project A, fresh project B, and a user-scope consumer without a project override; 2026-10-15 |
| Core Skill calls | `call_e840a82f253b4b6195059777`, `call_46f03d37038e430b9a524b96`, `call_ad2a52cadd894e609ea07e98` |

## Known limitations

The verification covered text style, minimal-fix explanation, exact identifier preservation, and risk-warning switching; it did not ask the Agent to modify code. The Skill cannot guarantee that every model retains the voice in a long session. The upstream Claude Code Hook for cross-turn state was neither installed nor tested. Cockney prose can reduce readability for non-English users or assistive technology; use it carefully in team documentation, user-facing copy, and formal incident communication.

## Security

The pinned directory contains only `SKILL.md`: no manifest, lockfile, installer, binary, downloader, runtime network access, credential, or telemetry. Each representative call emitted one completed Skill event and ran no command or file mutation. All risk sentences returned to plain English as required. After removal, all six Skill/vendor targets across project A, project B, and user scope were absent, and the user-root `package.json`, `codearts_cli.json`, and persistent permission-file SHA-256 hashes matched baseline.

## Evidence and sources

- [中文研究记录](../../research/2026-10-15.md) · [English](../../research/2026-10-15.en.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1/skills/cockney)
- [Agentic Awesome v19.7.0](https://github.com/sickn33/agentic-awesome-skills/releases/tag/v19.7.0)
- [Origin repository](https://github.com/Kadajett/cockney)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
