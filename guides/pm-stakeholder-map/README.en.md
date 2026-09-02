# PM Stakeholder Map on CodeArts CLI

[简体中文](README.md)

Install the verified `stakeholder-map` skill from PM Skills to Map stakeholders by power and interest and produce a communication plan with frequency, channel, and message.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Repository-pinned versions, team sharing, or one project. Recommended by default. |
| User | `~/.codeartsdoer` | Repeated use across projects for the current user. |

Official CodeArts documentation gives a same-named project skill priority. Do not install in both scopes. Stop on any same-name collision; never overwrite it.

## Install with an agent

### Project installation prompt

```text
Install and verify the stakeholder-map skill from phuryn/pm-skills for CodeArts CLI in the current project, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow exactly:
1. Modify only this project's .codeartsdoer. Do not modify ~/.codeartsdoer, credentials, global software, or other project files.
2. Run codearts --version, git --version, and codearts models. If several models are available, ask me to select a provider/model ID.
3. Check .codeartsdoer/vendor/pm-skills-stakeholder-map and .codeartsdoer/skills/stakeholder-map. Stop and report if either exists; never overwrite.
4. From the project root in PowerShell, run exactly:
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-stakeholder-map"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\stakeholder-map"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/stakeholder-map"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\stakeholder-map") -Destination $target -Recurse
5. Run codearts debug skill and confirm stakeholder-map resolves under this project's .codeartsdoer/skills/stakeholder-map/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly:
   codearts run --format json --model "<selected model>" "Call the skill tool exactly once with name stakeholder-map and use no other tool. Do not access the network or write files. Map these stakeholders for a university pilot: Provost high power/low interest, Student Success Director high/high, Student Union low/high, IT Helpdesk low/low. Provide frequency, channel, key message, neglect risk, and one conflict-alignment action. Use exact markers MANAGE-CLOSELY, KEEP-SATISFIED, KEEP-INFORMED, MONITOR, COMMUNICATION-PLAN, and CONFLICTS."
7. Passing criteria: There must be one completed `stakeholder-map` skill event resolving to the selected native directory. The four roles must land in the correct quadrants, and the plan must include frequency, channel, key message, neglect risk, and a conflict-alignment action.
8. Report the commit, source, target, completed event, resolved path, and result. Removal may delete only .codeartsdoer/skills/stakeholder-map and .codeartsdoer/vendor/pm-skills-stakeholder-map. Never delete .codeartsdoer itself, package.json, codearts_cli.json, or another skill. Stop after any failure and do not claim success.
```

### User installation prompt

```text
Install and verify the user-scoped stakeholder-map skill from phuryn/pm-skills for the current Windows user, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow exactly:
1. In PowerShell set $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Modify only $userRoot/vendor/pm-skills-stakeholder-map and $userRoot/skills/stakeholder-map. Do not modify $userRoot/package.json, codearts_cli.json, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. If several models are available, ask me to select a provider/model ID.
3. Check the two exact targets above. Stop and report if either exists; never overwrite.
4. In the same PowerShell session run exactly:
   $source = Join-Path $userRoot "vendor\pm-skills-stakeholder-map"
   $target = Join-Path $userRoot "skills\stakeholder-map"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/stakeholder-map"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\stakeholder-map") -Destination $target -Recurse
5. Change to a clean directory with no same-named project skill. Run codearts debug skill and confirm the location is ~/.codeartsdoer/skills/stakeholder-map/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly from that clean directory:
   codearts run --format json --model "<selected model>" "Call the skill tool exactly once with name stakeholder-map and use no other tool. Do not access the network or write files. Map these stakeholders for a university pilot: Provost high power/low interest, Student Success Director high/high, Student Union low/high, IT Helpdesk low/low. Provide frequency, channel, key message, neglect risk, and one conflict-alignment action. Use exact markers MANAGE-CLOSELY, KEEP-SATISFIED, KEEP-INFORMED, MONITOR, COMMUNICATION-PLAN, and CONFLICTS."
7. Passing criteria: There must be one completed `stakeholder-map` skill event resolving to the selected native directory. The four roles must land in the correct quadrants, and the plan must include frequency, channel, key message, neglect risk, and a conflict-alignment action.
8. Report the commit, completed event, resolved path, and result. Removal may delete only $userRoot/skills/stakeholder-map and $userRoot/vendor/pm-skills-stakeholder-map. Never delete the user-root package.json, codearts_cli.json, credentials, or another skill. Stop after any failure and do not claim success.
```

## Install manually on Windows

Confirm the tools and intended model:

```powershell
codearts --version
git --version
codearts models
```

For project scope, run from the project root:

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-stakeholder-map"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\stakeholder-map"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-execution/skills/stakeholder-map"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\stakeholder-map") -Destination $target -Recurse
```

For user scope, run:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-stakeholder-map"
$target = Join-Path $userRoot "skills\stakeholder-map"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-execution/skills/stakeholder-map"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\stakeholder-map") -Destination $target -Recurse
```

Both scopes copy only the selected skill and run no upstream Python validator, installer, or dependency.

## CodeArts configuration

This skill requires no `codearts_cli.json` change. Install CodeArts CLI with the [official guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html), then choose a real `provider/model` ID with `codearts models`. For custom models, follow the [official configuration example](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html) and keep keys in local configuration or environment variables.

In this custom-provider test, CodeArts CLI 26.8.1 required `CODEARTS_CLI_AK` and `CODEARTS_CLI_SK` to exist in the process. Non-secret, non-persisted placeholders passed that preflight while the Provider's own key authenticated the model. This is observed behavior, not a compatibility guarantee. Use valid credentials from the [official AK/SK guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html) for Huawei Cloud-hosted models.

## Verify

Check the resolved source path:

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills | Where-Object { $_.name -eq "stakeholder-map" } | Select-Object name, location
```

Then replace the model ID and perform the real call:

```powershell
codearts run --format json --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name stakeholder-map and use no other tool. Do not access the network or write files. Map these stakeholders for a university pilot: Provost high power/low interest, Student Success Director high/high, Student Union low/high, IT Helpdesk low/low. Provide frequency, channel, key message, neglect risk, and one conflict-alignment action. Use exact markers MANAGE-CLOSELY, KEEP-SATISFIED, KEEP-INFORMED, MONITOR, COMMUNICATION-PLAN, and CONFLICTS."
```

There must be one completed `stakeholder-map` skill event resolving to the selected native directory. The four roles must land in the correct quadrants, and the plan must include frequency, channel, key message, neglect risk, and a conflict-alignment action. A plausible final answer without the completed target `skill` event is not a pass.

## Use

```text
Call the skill tool with name stakeholder-map. Map these roles on the power × interest grid and create an actionable communication plan.
```

## Update

Review a new commit and the selected skill directory first. Remove only the old targets in the chosen scope, reinstall the pinned new commit, and repeat full verification. Do not treat floating `main` as verified.

## Remove

Project targets only:

- `.codeartsdoer/skills/stakeholder-map`
- `.codeartsdoer/vendor/pm-skills-stakeholder-map`

User targets only:

- `~/.codeartsdoer/skills/stakeholder-map`
- `~/.codeartsdoer/vendor/pm-skills-stakeholder-map`

Do not delete the `.codeartsdoer` root, user-root `package.json`, `codearts_cli.json`, credentials, or unrelated skills.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Works** |
| Upstream | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| Upstream version | v2.1.0 |
| Pinned source | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| Verified skill | `stakeholder-map` |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model | `mimo/mimo-v2.5` |
| Verified scopes | Project A, fresh project B, and user |
| Last verified | 2026-09-02 |

All three scopes passed installation, exact discovery, target-path loading, representative behavior, rollback, and post-removal diagnostics. The user-root `package.json` and `codearts_cli.json` SHA-256 hashes were unchanged.

## Known limitations

Only user-supplied roles and ratings were verified; inferred power relationships in a real organization still need human confirmation.

Only Windows 11, CodeArts CLI 26.8.1, `mimo/mimo-v2.5`, the pinned commit, and the representative flow above were tested. Linux, macOS, other models, Claude `/commands`, and full PM plugin orchestration were not tested.

## Security

- Pin the commit and copy only `pm-execution/skills/stakeholder-map`; do not run upstream `validate_plugins.py`, tests, or other plugin content.
- The test prompt supplies complete local facts and forbids network and file writes; `--auto` is unnecessary.
- Skill instructions influence agent behavior; review `SKILL.md` before use in a sensitive project.
- Installation and removal do not modify credentials, the user-root manifest, or CodeArts model configuration.

## Evidence and sources

- [2026-09-02 verification](../../research/2026-09-02.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-execution/skills/stakeholder-map/SKILL.md)
- [Upstream repository](https://github.com/phuryn/pm-skills)