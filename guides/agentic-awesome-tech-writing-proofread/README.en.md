# Use tech-writing-proofread in CodeArts CLI

[简体中文](README.md)

Install `tech-writing-proofread` so CodeArts itemizes defects, suggestions, and reasons in English technical prose without rewriting the document.

## Choose an installation scope

Project scope affects one repository; user scope is available across the current user's projects. Choose one. A project Skill takes priority over a same-named user Skill; stop on every same-name Skill, target, or vendor collision. Two fresh projects and a user consumer without a project override were verified.

## Ask the Agent to install it

### Project-scope prompt

```text
Install and verify the project-scoped tech-writing-proofread Skill in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git at v19.7.0 and commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1. Copy the complete skills/tech-writing-proofread directory (SKILL.md only) into <project>/.codeartsdoer/skills/tech-writing-proofread, with vendor checkout <project>/.codeartsdoer/vendor/agentic-awesome-tech-writing-proofread. Do not modify package.json, codearts_cli.json, permissions, credentials, plugins, or another Skill.
Run codearts --version, git --version, node --version, and codearts models first and let me select <model>. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-tech-writing-proofread', $target=Join-Path $root 'skills\tech-writing-proofread', and $userTarget=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer\skills\tech-writing-proofread'. Stop if any exists. Confirm child paths; run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/tech-writing-proofread; require HEAD a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1; run Copy-Item -LiteralPath (Join-Path $source 'skills\tech-writing-proofread') -Destination $target -Recurse; require $target/SKILL.md SHA-256 830B485BE9EE9F505DCC42B7F96456EC916A38DA55A266E13C1FD5094EFD02D2.
Run codearts debug skill and require the only name=tech-writing-proofread location to equal $target/SKILL.md. Run verbatim with the selected model: codearts run -m "<model>" --format json 'Call the skill tool exactly once with name tech-writing-proofread and use no other tool. Proofread this exact synthetic sentence without rewriting the document: "The API return an unique request IDs, and retries is safe for every POST." Return only an itemized Original -> Suggestion -> Reason list. Correct subject agreement, the article before unique, plural consistency, and the unsupported absolute retry claim by changing it to conditional language; do not invent protocol facts. End with FOUR_ISSUES_ITEMIZED and NO_DOCUMENT_REWRITE.'
Pass only for exit 0, exactly one completed Skill event from $target, no other tool events, all four issue classes, no full rewrite, no universal safe-POST assertion, and both markers. Report the commit, location, and Skill call ID. Recheck paths, remove only $target and $source, and require the former path to disappear from diagnostics. Never remove all of .codeartsdoer, root configuration, or another Skill.
```

### User-scope prompt

```text
Install and verify the user-scoped tech-writing-proofread Skill. Pin https://github.com/sickn33/agentic-awesome-skills.git at v19.7.0 and commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1. Copy all of skills/tech-writing-proofread (SKILL.md only) to ~/.codeartsdoer/skills/tech-writing-proofread, with vendor checkout ~/.codeartsdoer/vendor/agentic-awesome-tech-writing-proofread. Do not modify the user's package.json, codearts_cli.json, permissions, credentials, plugins, or another Skill.
Run codearts --version, git --version, node --version, and codearts models and let me select <model>. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-tech-writing-proofread', $target=Join-Path $root 'skills\tech-writing-proofread', $consumer=Join-Path (Get-Location) 'codearts-tech-writing-proofread-consumer', and $projectTarget=Join-Path $consumer '.codeartsdoer\skills\tech-writing-proofread'; stop if any exists. Create the consumer. Run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/tech-writing-proofread; require HEAD a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1; run Copy-Item -LiteralPath (Join-Path $source 'skills\tech-writing-proofread') -Destination $target -Recurse; require $target/SKILL.md SHA-256 830B485BE9EE9F505DCC42B7F96456EC916A38DA55A266E13C1FD5094EFD02D2.
From the consumer run codearts debug skill and require the only name=tech-writing-proofread location to be $target/SKILL.md. Run verbatim: codearts run -m "<model>" --format json 'Call the skill tool exactly once with name tech-writing-proofread and use no other tool. Proofread this exact synthetic sentence without rewriting the document: "The API return an unique request IDs, and retries is safe for every POST." Return only an itemized Original -> Suggestion -> Reason list. Correct subject agreement, the article before unique, plural consistency, and the unsupported absolute retry claim by changing it to conditional language; do not invent protocol facts. End with FOUR_ISSUES_ITEMIZED and NO_DOCUMENT_REWRITE.' Pass only for exit 0, one completed Skill event from $target, no other tool events, all four issue classes, no full rewrite or universal retry claim, and both markers. Report the call ID. Remove only $target, $source, and that exact consumer, then confirm removal without a project override. Never remove the user profile, all of .codeartsdoer, root configuration, another Skill, or an existing project.
```

## Manual installation on Windows

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer'
# User scope: $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-tech-writing-proofread'
$target=Join-Path $root 'skills\tech-writing-proofread'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/tech-writing-proofread
if((git -C $source rev-parse HEAD).Trim() -ne 'a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\tech-writing-proofread') -Destination $target -Recurse
if((Get-FileHash -Algorithm SHA256 -LiteralPath (Join-Path $target 'SKILL.md')).Hash -ne '830B485BE9EE9F505DCC42B7F96456EC916A38DA55A266E13C1FD5094EFD02D2'){throw 'Skill hash mismatch'}
```

## CodeArts configuration

Choose `provider/model` with `codearts models`. This Markdown-only Skill needs no dependency, plugin, network, credential, or configuration edit. Verification does not need `--auto`.

## Verify

Check the absolute location, then run the prompt command. Require one completed Skill event, all four independent corrections, conditional POST-retry language, and both markers.

## Use

```text
Use tech-writing-proofread on the English text below. Return an itemized Original -> Suggestion -> Reason list. Preserve the author's voice; do not rewrite the whole document or invent technical facts.
```

## Update

Pin and review the new version, then repeat all three scopes and rollback. Do not follow floating `main`.

## Remove

First use `[IO.Path]::GetFullPath()` to confirm both paths are children of `$root`. Run exactly `Remove-Item -LiteralPath (Join-Path $root 'skills\tech-writing-proofread') -Recurse -Force` and `Remove-Item -LiteralPath (Join-Path $root 'vendor\agentic-awesome-tech-writing-proofread') -Recurse -Force`, then run `codearts debug skill` and require the old path to be absent. Never delete all of `.codeartsdoer`, root configuration, another Skill, a project, or user files.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | Agentic Awesome `v19.7.0`; commit `a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1` |
| File / SHA-256 | `SKILL.md` / `830B485BE9EE9F505DCC42B7F96456EC916A38DA55A266E13C1FD5094EFD02D2` |
| License | MIT; source `alapha888/agent-skills-en` |
| Environment | CodeArts CLI 26.8.1; Windows 11 Build 26200; PowerShell 7.6.6; Git 2.53.0.windows.2; Node 24.14.1; npm 11.11.0; `mimo/mimo-v2.5` |
| Scope | Project A, fresh project B, and user consumer without a project override; 2026-10-16 |
| Core Skill calls | `call_31ccce46b8e141cfa090f493`, `call_13e05a1332db4068b432ac5b`, `call_02b89ac003b94dcb821cc48f` |

## Known limitations

Only one English technical sentence was tested. Long documents, terminology files, cross-file consistency, Chinese proofreading, and automatic rewriting were not tested. Authors must still review product and protocol claims.

## Security

The pinned directory contains only `SKILL.md`; the calls emitted no other tool events and performed no write, network, credential, or telemetry action. User baseline hashes matched after rollback.

## Evidence and sources

- [中文研究记录](../../research/2026-10-16.md) · [English research](../../research/2026-10-16.en.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1/skills/tech-writing-proofread)
- [Original repository](https://github.com/alapha888/agent-skills-en)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
