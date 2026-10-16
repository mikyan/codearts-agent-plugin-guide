# Use git-commit-message in CodeArts CLI

[简体中文](README.md)

Install the `git-commit-message` Skill so CodeArts can generate a concise Conventional Commit message from the real staged diff without staging, committing, or pushing anything.

## Choose an installation scope

Project scope under `<project>/.codeartsdoer` affects one repository. User scope under `~/.codeartsdoer` is available across the current user's projects. Choose one. CodeArts documents the project Skill as taking priority over a same-named user Skill; stop on any project, user, target-Skill, or vendor collision. This guide verified two fresh projects and a user consumer without a project override.

## Ask the Agent to install it

### Project-scope prompt

```text
Install and verify the project-scoped git-commit-message Skill in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git at release v19.7.0 and commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1. Copy the complete skills/git-commit-message directory (SKILL.md only) into <project>/.codeartsdoer/skills/git-commit-message, using <project>/.codeartsdoer/vendor/agentic-awesome-git-commit-message as the vendor checkout. Do not change package.json, codearts_cli.json, permissions, credentials, plugins, any other Skill, the worktree, or the index.
From the project root run codearts --version, git --version, node --version, and codearts models, then let me select a real <model>. Do not read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-git-commit-message', $target=Join-Path $root 'skills\git-commit-message', and $userTarget=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer\skills\git-commit-message'. Stop on any collision. Confirm child paths; run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/git-commit-message; require (git -C $source rev-parse HEAD).Trim() to equal a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1; run Copy-Item -LiteralPath (Join-Path $source 'skills\git-commit-message') -Destination $target -Recurse; require $target/SKILL.md SHA-256 6261A9FDD3CF103F279CD936BABF4C0CF607A0CF02E287573B0F37563632AC51.
Require an existing user-authorized staged diff; if git diff --cached is empty, stop and never stage a file yourself. Run codearts debug skill and require the only name=git-commit-message location to be $target/SKILL.md. Replace <model> and run verbatim: codearts run -m "<model>" --format json --auto 'Call the skill tool exactly once with name git-commit-message. Inspect the real staged change by running only git status --short, git diff --cached --stat, and git diff --cached. Do not stage, modify, commit, push, or access the network. Return one conventional commit message with a subject of at most 50 characters and a 1-3 line body explaining why. End with COMMIT_MESSAGE_FROM_STAGED_DIFF and NO_GIT_MUTATION.'
Pass only when exit code is 0, exactly one completed Skill event resolves to $target, exactly three completed Bash events are the listed read-only Git commands, the subject and body satisfy the requested format, both markers appear, and HEAD plus git diff --cached are identical before and after. Report the commit, absolute target, Skill call ID, and assertions. Recheck the paths, remove only $target and $source, rerun codearts debug skill, and require the former path to be absent. Never remove all of .codeartsdoer, the project, user profile, root configuration, another Skill, or a user file.
```

### User-scope prompt

```text
Install and verify the user-scoped git-commit-message Skill for the current Windows user. Pin https://github.com/sickn33/agentic-awesome-skills.git at v19.7.0 and commit a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1. Copy all of skills/git-commit-message (SKILL.md only) to ~/.codeartsdoer/skills/git-commit-message, with vendor checkout ~/.codeartsdoer/vendor/agentic-awesome-git-commit-message. Do not modify the user's package.json, codearts_cli.json, permissions, credentials, plugins, or another Skill.
Run codearts --version, git --version, node --version, and codearts models and let me select <model>. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-git-commit-message', $target=Join-Path $root 'skills\git-commit-message', $consumer=Join-Path (Get-Location) 'codearts-git-commit-message-consumer', and $projectTarget=Join-Path $consumer '.codeartsdoer\skills\git-commit-message'; stop if any exists. Create $consumer; inside it run git init, git config user.name 'CodeArts Verification', and git config user.email 'codearts-verification@example.invalid'. Create greeting.py with exactly `def greet(display_name):` then four spaces plus `return f"Hello {display_name}"`; run git add greeting.py and git commit -m baseline. Change only the return line to `return f"Hello {display_name or 'there'}"` and run only git add greeting.py; never push. Run New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/git-commit-message; require HEAD a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1; run Copy-Item -LiteralPath (Join-Path $source 'skills\git-commit-message') -Destination $target -Recurse; require $target/SKILL.md SHA-256 6261A9FDD3CF103F279CD936BABF4C0CF607A0CF02E287573B0F37563632AC51.
From $consumer run codearts debug skill and require the only name=git-commit-message location to be $target/SKILL.md. Replace <model> and run verbatim: codearts run -m "<model>" --format json --auto 'Call the skill tool exactly once with name git-commit-message. Inspect the real staged change by running only git status --short, git diff --cached --stat, and git diff --cached. Do not stage, modify, commit, push, or access the network. Return one conventional commit message with a subject of at most 50 characters and a 1-3 line body explaining why. End with COMMIT_MESSAGE_FROM_STAGED_DIFF and NO_GIT_MUTATION.' Pass only for exit 0, one completed Skill event from $target, exactly three completed Bash events for those read-only Git commands, a compliant subject and body, both markers, and unchanged HEAD plus staged diff. Report the call ID. Remove only $target, $source, and this exact consumer, then confirm removal from a directory without a project override. Never remove the user profile, all of .codeartsdoer, root configuration, another Skill, or an existing project.
```

## Manual installation on Windows

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer'
# User scope: $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-git-commit-message'
$target=Join-Path $root 'skills\git-commit-message'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.7.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/git-commit-message
if((git -C $source rev-parse HEAD).Trim() -ne 'a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\git-commit-message') -Destination $target -Recurse
if((Get-FileHash -Algorithm SHA256 -LiteralPath (Join-Path $target 'SKILL.md')).Hash -ne '6261A9FDD3CF103F279CD936BABF4C0CF607A0CF02E287573B0F37563632AC51'){throw 'Skill hash mismatch'}
```

## CodeArts configuration

Choose a real `provider/model` with `codearts models`. The Skill is Markdown-only and needs no package, Python, MCP, Hook, plugin, credential, network, or root-config edit. Verification lets the Agent run three read-only Git commands; inspect the directory and staged diff before using `--auto`, or approve each command interactively.

## Verify

Check the exact location with `codearts debug skill`, then run the prompt command. A plausible message is insufficient: require the completed Skill event, three read-only Git events, correct message, both markers, and unchanged HEAD/index.

## Use

```text
Use git-commit-message. Inspect my staged diff but do not change it. Propose one Conventional Commit subject (50 characters or fewer) and an optional 1-3 line body explaining why. Do not commit or push.
```

## Update

Pin and review the new release or commit, then repeat both fresh projects, the user consumer, the representative call, and exact rollback. Do not follow floating `main`.

## Remove

First use `[IO.Path]::GetFullPath()` to confirm both paths are children of `$root`. Then run exactly `Remove-Item -LiteralPath (Join-Path $root 'skills\git-commit-message') -Recurse -Force` and `Remove-Item -LiteralPath (Join-Path $root 'vendor\agentic-awesome-git-commit-message') -Recurse -Force`. Rerun `codearts debug skill` and require the old absolute path to be absent. Never delete all of `.codeartsdoer`, root configuration, another Skill, a Git worktree, or user files.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | Agentic Awesome `v19.7.0`; commit `a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1` |
| File / SHA-256 | `SKILL.md` / `6261A9FDD3CF103F279CD936BABF4C0CF607A0CF02E287573B0F37563632AC51` |
| License | MIT; source `alapha888/agent-skills-en` |
| Environment | CodeArts CLI 26.8.1; Windows 11 Build 26200; PowerShell 7.6.6; Git 2.53.0.windows.2; Node 24.14.1; npm 11.11.0; `mimo/mimo-v2.5` |
| Scope | Project A, fresh project B, and user consumer without a project override; 2026-10-16 |
| Core Skill calls | `call_e2c70223aa6c4d43b050afe0`, `call_db63b94e268c4f0ebce3aad7`, `call_9f7646ef15a7402497ee8a39` |

## Known limitations

Only one staged bug fix was tested. Mixed change types, an empty index, merge commits, and non-English subjects were not tested. The message still needs human review.

## Security

The pinned directory contains only `SKILL.md`. Each call ran only `git status` and two `git diff --cached` commands; it wrote no file and made no commit or push. All three user baseline hashes matched after rollback.

## Evidence and sources

- [中文研究记录](../../research/2026-10-16.md) · [English research](../../research/2026-10-16.en.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/a3072641b750a1fb2f6ce695a5f984ea3d4c3ec1/skills/git-commit-message)
- [Original repository](https://github.com/alapha888/agent-skills-en)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
