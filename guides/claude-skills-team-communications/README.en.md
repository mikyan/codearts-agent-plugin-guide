# Use team-communications with CodeArts CLI

[简体中文](README.md)

Install `team-communications` to compress team facts into a clear Progress, Plans, Problems update.

## Choose a scope

Use `<project>/.codeartsdoer` for a team pin or `~/.codeartsdoer` for current-user reuse. Choose one and stop if either scope already has the skill.

## Ask Agent to install it

### Project prompt

```text
Install and verify project-scoped team-communications from alirezarezvani/claude-skills at commit aa8d778811a557a2c28ccadda4cf3d0bd028a4cc.
1. Create only .codeartsdoer/vendor/claude-skills-team-communications and .codeartsdoer/skills/team-communications. Do not modify user files, package.json, codearts_cli.json, credentials, or other skills. Run codearts --version, git --version, and codearts models and ask me for an available model; stop on missing credentials or a project/user collision.
2. From the project root run in PowerShell:
   $s=Join-Path (Get-Location) '.codeartsdoer\vendor\claude-skills-team-communications'; $t=Join-Path (Get-Location) '.codeartsdoer\skills\team-communications'
   New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/alirezarezvani/claude-skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/project-management/skills/team-communications/' '/LICENSE'
   git -C $s checkout --detach aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
   if((git -C $s rev-parse HEAD).Trim() -ne 'aa8d778811a557a2c28ccadda4cf3d0bd028a4cc'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'project-management\skills\team-communications') -Destination $t -Recurse
3. The only debug-skill match must be $t/SKILL.md. Replace <model> and run verbatim: codearts run --format json --sandbox --model "<model>" "Verification contract: call the skill tool exactly once with name team-communications, then use the read tool exactly once on the loaded skill's references/3p-updates.md. No other tool is allowed. Based only on those sources, write a three-line 3P update headed Platform Team (Aug 10–16): Progress shipped the Windows installer to 100% and cut setup from 20 to 8 minutes; Plans add rollback telemetry next week; Problems two legacy proxies block automatic updates. Use the exact labels Progress:, Plans:, Problems:."
4. Pass only on exit 0, one completed skill event, one completed read, no other tools, and all labels/facts. Report evidence. Remove only $t and $s; never delete the whole root or unrelated configuration.
```

### User prompt

```text
Install and verify user-scoped team-communications from commit aa8d778811a557a2c28ccadda4cf3d0bd028a4cc.
1. Create only ~/.codeartsdoer/vendor/claude-skills-team-communications and ~/.codeartsdoer/skills/team-communications; do not alter root package.json, codearts_cli.json, credentials, plugins, or projects. Run version/models checks and stop on missing credentials or collisions.
2. In PowerShell run:
   $u=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'; $s=Join-Path $u 'vendor\claude-skills-team-communications'; $t=Join-Path $u 'skills\team-communications'; $c=Join-Path ([IO.Path]::GetTempPath()) 'codearts-team-comms-aa8d778-smoke'
   if((Test-Path $s)-or(Test-Path $t)-or(Test-Path $c)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent),$c -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/alirezarezvani/claude-skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/project-management/skills/team-communications/' '/LICENSE'
   git -C $s checkout --detach aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
   if((git -C $s rev-parse HEAD).Trim() -ne 'aa8d778811a557a2c28ccadda4cf3d0bd028a4cc'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'project-management\skills\team-communications') -Destination $t -Recurse; Set-Location $c
3. Confirm the user source with debug skill. Replace <model> and run: codearts run --format json --sandbox --model "<model>" "Verification contract: call the skill tool exactly once with name team-communications, then use the read tool exactly once on the loaded skill's references/3p-updates.md. No other tool is allowed. Based only on those sources, write a three-line 3P update headed Platform Team (Aug 10–16): Progress shipped the Windows installer to 100% and cut setup from 20 to 8 minutes; Plans add rollback telemetry next week; Problems two legacy proxies block automatic updates. Use the exact labels Progress:, Plans:, Problems:." Pass only with one completed skill, one completed read, no other tools, and all labels/facts. Remove only $t, $s, and verified $c; never delete the user root or unrelated files.
```

## Manual Windows installation

Run the version and model checks, choose one scope, and execute the matching prompt's exact pinned checkout and full-directory copy.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # User: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$s=Join-Path $root 'vendor\claude-skills-team-communications'; $t=Join-Path $root 'skills\team-communications'
if((Test-Path $s)-or(Test-Path $t)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
git clone --filter=blob:none --no-checkout https://github.com/alirezarezvani/claude-skills.git $s
git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/project-management/skills/team-communications/' '/LICENSE'
git -C $s checkout --detach aa8d778811a557a2c28ccadda4cf3d0bd028a4cc
Copy-Item -LiteralPath (Join-Path $s 'project-management\skills\team-communications') -Destination $t -Recurse
```

## CodeArts configuration

Configure credentials through official mechanisms and never write them to the repository or output.

## Verification

Both the completed skill event and the relative-reference read are required.

## Use

```text
Use team-communications. Turn these facts into an executive 3P weekly update without inventing facts: <facts>
```

## Update

Check with `git -C $s rev-parse HEAD`; re-audit and revalidate upgrades.

## Remove

Remove only `skills/team-communications` and `vendor/claude-skills-team-communications` from the selected scope and confirm absence.

## Verified compatibility

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | release `v2.9.0`; commit `aa8d778811a557a2c28ccadda4cf3d0bd028a4cc` |
| Skill / SHA-256 | `team-communications` / `50594ECF84B29CB6C35C46C536EA9B21F47A3ED929D76F6F8B3BA43FABFA6A53` |
| License | MIT |
| Environment/scopes | CodeArts 26.8.1; Windows 11 Build 26200; MiMo; two projects + user; 2026-08-20 |

## Known limitations

Only the 3P flow and relative reference were tested.

## Security

The pinned directory has five text files totaling 14,181 bytes and executes no npm, script, runtime network, credential, or telemetry step. Sanitize internal facts before use.

## Evidence and sources

- [中文研究](../../research/2026-08-20.md) · [English](../../research/2026-08-20.en.md)
- [Pinned commit](https://github.com/alirezarezvani/claude-skills/tree/aa8d778811a557a2c28ccadda4cf3d0bd028a4cc/project-management/skills/team-communications)
- [MIT License](https://github.com/alirezarezvani/claude-skills/blob/aa8d778811a557a2c28ccadda4cf3d0bd028a4cc/LICENSE)
- [Official CodeArts Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
