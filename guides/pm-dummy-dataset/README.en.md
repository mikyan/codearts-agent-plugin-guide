# PM Dummy Dataset Generation on CodeArts CLI

[简体中文](README.md)

Install the verified `dummy-dataset` skill from PM Skills to generate directly usable synthetic test data from columns, formats, and business constraints.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Repository-pinned versions, team sharing, or one project. Recommended by default. |
| User | `~/.codeartsdoer` | Repeated test-data generation across projects for the current user. |

Official CodeArts documentation gives a same-named project skill priority. Do not install in both scopes. Stop if either same-named skill or vendor directory exists; never overwrite it.

## Install with an agent

The prompts below include the verified installation, isolated write permission, real invocation, and rollback boundaries. Verification creates `dummy-commute-tasks.csv`; uninstalling the skill must not delete user-generated datasets.

### Project installation prompt

```text
Install and verify the dummy-dataset skill from phuryn/pm-skills for CodeArts CLI in the current project, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow exactly:
1. Install only to this project's .codeartsdoer/vendor/pm-skills-dummy-dataset and .codeartsdoer/skills/dummy-dataset. Do not modify ~/.codeartsdoer, credentials, the project's root package.json, the user's root package.json, or codearts_cli.json.
2. From the project root run codearts --version, git --version, and codearts models. If several models are available, ask me to select the exact provider/model ID.
3. Check both installation targets and dummy-commute-tasks.csv in the project root. Stop and report if any exists; never overwrite.
4. From the project root in PowerShell run exactly:
   $projectRoot = (Get-Location).Path
   $source = Join-Path $projectRoot ".codeartsdoer\vendor\pm-skills-dummy-dataset"
   $target = Join-Path $projectRoot ".codeartsdoer\skills\dummy-dataset"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/dummy-dataset"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\dummy-dataset") -Destination $target -Recurse
5. Run codearts debug skill and confirm that dummy-dataset resolves to this project's .codeartsdoer/skills/dummy-dataset/SKILL.md.
6. Create a one-run permission directory for non-interactive writing; never modify the user's persistent permission/global.json:
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-dummy-dataset-project-verify"
   if (Test-Path -LiteralPath $verifyRoot) { throw "Verification directory already exists; inspect it instead of overwriting." }
   $kernelData = Join-Path $verifyRoot "kernel-data"
   $permissionDir = Join-Path $kernelData "storage\permission"
   New-Item -ItemType Directory -Path $permissionDir -Force | Out-Null
   @'
   [
     { "permission": "edit", "pattern": "*", "action": "allow" },
     { "permission": "write", "pattern": "*", "action": "allow" },
     { "permission": "bash", "pattern": "*", "action": "deny" },
     { "permission": "read", "pattern": "*", "action": "deny" },
     { "permission": "webfetch", "pattern": "*", "action": "deny" },
     { "permission": "websearch", "pattern": "*", "action": "deny" },
     { "permission": "external_directory_write", "pattern": "*", "action": "deny" },
     { "permission": "external_directory_read", "pattern": "*", "action": "deny" },
     { "permission": "dotfile", "pattern": "*", "action": "deny" }
   ]
   '@ | Set-Content -LiteralPath (Join-Path $permissionDir "global.json") -Encoding utf8
7. Replace <selected model> with the model from step 2. Still from the project root, run the real executable installed by CodeArts 26.8.1:
   $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
   $codeartsExe = Join-Path $userRoot "installers\bin\codearts.exe"
   $env:SCENARIO = "codeartsdoer"
   $env:KERNEL_DATA_DIR = $kernelData
   $env:KERNEL_CONFIG_DIR = $userRoot
   $env:OPENCODE_CHANNEL = "latest"
   $env:OPENCODE_CONFIG = Join-Path $userRoot "codearts_cli.json"
   $env:OPENCODE_CONFIG_FILE = "codearts_cli.json,codearts_cli.jsonc"
   $env:OPENCODE_MODE = "tui"
   $env:PLUGIN_ENV = "hc"
   $env:NODE_TLS_REJECT_UNAUTHORIZED = "0"
   $env:OPENCODE_DISABLE_MODELS_FETCH = "1"
   $env:OPENCODE_DISABLE_AUTOUPDATE = "true"
   $env:OPENCODE_ALWAYS_NOTIFY_UPDATE = "false"
   $env:OMO_SEND_ANONYMOUS_TELEMETRY = "0"
   $prompt = "Call the skill tool exactly once with name dummy-dataset, then use the write tool exactly once to create dummy-commute-tasks.csv in the current workspace. Use no other tool, do not access the network, and do not read or execute files. Generate exactly 6 synthetic task records plus one CSV header. Columns must be task_id,title,commute_minutes,due_in_hours,needs_review,device. Use IDs T001 through T006 exactly once. commute_minutes must be integers from 5 through 60. due_in_hours must be an integer from 0 through 48 or blank. needs_review must be true exactly when due_in_hours is blank. device must be phone or laptop. Include at least one blank due_in_hours, at least one phone, and at least one laptop. Do not use real names, emails, or personal data. Finish by stating only that dummy-commute-tasks.csv was created."
   & $codeartsExe run -m "<selected model>" --auto --format json $prompt
8. Passing requires exactly one completed dummy-dataset skill event and one completed write event. The skill must resolve to the step 4 target. The CSV must contain one header and six T001-T006 rows satisfying every type, range, dependency, and no-real-personal-data constraint. A file without the events, or final text without the file, does not pass.
9. After recording the events and result, delete only $verifyRoot; do not delete dummy-commute-tasks.csv from the project root. Report the commit, source, target, completed events, resolved path, and constraint checks. Stop after any failure and do not claim success.
10. Removal may delete only .codeartsdoer/skills/dummy-dataset and .codeartsdoer/vendor/pm-skills-dummy-dataset. Never delete .codeartsdoer itself, any package.json, codearts_cli.json, another skill, or generated data.
```

### User installation prompt

```text
Install and verify the user-scoped dummy-dataset skill from phuryn/pm-skills for the current Windows user, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow exactly:
1. Set $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Install only to $userRoot/vendor/pm-skills-dummy-dataset and $userRoot/skills/dummy-dataset. Do not modify the user's root package.json, codearts_cli.json, persistent permission/global.json, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. If several models are available, ask me to select the exact provider/model ID.
3. Check both installation targets. Stop and report if either exists; never overwrite.
4. In PowerShell run exactly:
   $source = Join-Path $userRoot "vendor\pm-skills-dummy-dataset"
   $target = Join-Path $userRoot "skills\dummy-dataset"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/dummy-dataset"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\dummy-dataset") -Destination $target -Recurse
5. Create an isolated workspace without a project override and a complete temporary permission file:
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-dummy-dataset-user-verify"
   if (Test-Path -LiteralPath $verifyRoot) { throw "Verification directory already exists; inspect it instead of overwriting." }
   $workspace = Join-Path $verifyRoot "workspace"
   $kernelData = Join-Path $verifyRoot "kernel-data"
   $permissionDir = Join-Path $kernelData "storage\permission"
   New-Item -ItemType Directory -Path $workspace,$permissionDir -Force | Out-Null
   @'
   [
     { "permission": "edit", "pattern": "*", "action": "allow" },
     { "permission": "write", "pattern": "*", "action": "allow" },
     { "permission": "bash", "pattern": "*", "action": "deny" },
     { "permission": "read", "pattern": "*", "action": "deny" },
     { "permission": "webfetch", "pattern": "*", "action": "deny" },
     { "permission": "websearch", "pattern": "*", "action": "deny" },
     { "permission": "external_directory_write", "pattern": "*", "action": "deny" },
     { "permission": "external_directory_read", "pattern": "*", "action": "deny" },
     { "permission": "dotfile", "pattern": "*", "action": "deny" }
   ]
   '@ | Set-Content -LiteralPath (Join-Path $permissionDir "global.json") -Encoding utf8
   if (Test-Path -LiteralPath (Join-Path $workspace ".codeartsdoer\skills\dummy-dataset")) { throw "Project override exists." }
6. Change to $workspace, run codearts debug skill, and confirm dummy-dataset resolves to $userRoot/skills/dummy-dataset/SKILL.md rather than a project path.
7. In that workspace, replace <selected model> with step 2's model and run exactly:
   $codeartsExe = Join-Path $userRoot "installers\bin\codearts.exe"
   $env:SCENARIO = "codeartsdoer"
   $env:KERNEL_DATA_DIR = $kernelData
   $env:KERNEL_CONFIG_DIR = $userRoot
   $env:OPENCODE_CHANNEL = "latest"
   $env:OPENCODE_CONFIG = Join-Path $userRoot "codearts_cli.json"
   $env:OPENCODE_CONFIG_FILE = "codearts_cli.json,codearts_cli.jsonc"
   $env:OPENCODE_MODE = "tui"
   $env:PLUGIN_ENV = "hc"
   $env:NODE_TLS_REJECT_UNAUTHORIZED = "0"
   $env:OPENCODE_DISABLE_MODELS_FETCH = "1"
   $env:OPENCODE_DISABLE_AUTOUPDATE = "true"
   $env:OPENCODE_ALWAYS_NOTIFY_UPDATE = "false"
   $env:OMO_SEND_ANONYMOUS_TELEMETRY = "0"
   $prompt = "Call the skill tool exactly once with name dummy-dataset, then use the write tool exactly once to create dummy-commute-tasks.csv in the current workspace. Use no other tool, do not access the network, and do not read or execute files. Generate exactly 6 synthetic task records plus one CSV header. Columns must be task_id,title,commute_minutes,due_in_hours,needs_review,device. Use IDs T001 through T006 exactly once. commute_minutes must be integers from 5 through 60. due_in_hours must be an integer from 0 through 48 or blank. needs_review must be true exactly when due_in_hours is blank. device must be phone or laptop. Include at least one blank due_in_hours, at least one phone, and at least one laptop. Do not use real names, emails, or personal data. Finish by stating only that dummy-commute-tasks.csv was created."
   & $codeartsExe run -m "<selected model>" --auto --format json $prompt
8. Passing is identical to project scope: one completed target skill event, one completed write event, the exact user-scope source path, one CSV header plus six rows, and every CSV constraint. Record evidence, then delete only $verifyRoot; do not delete another temporary directory or user file.
9. Report the commit, source, target, completed events, resolved path, and constraint checks. Stop after any failure and do not claim success.
10. Removal may delete only $userRoot/skills/dummy-dataset and $userRoot/vendor/pm-skills-dummy-dataset. Never delete the user's root package.json, codearts_cli.json, persistent permissions, credentials, another skill, or generated data.
```

## Manual installation on Windows

Confirm the tools and model first:

```powershell
codearts --version
git --version
codearts models
```

From the project root, install project scope with:

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-dummy-dataset"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\dummy-dataset"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-execution/skills/dummy-dataset"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\dummy-dataset") -Destination $target -Recurse
```

For user scope, run:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-dummy-dataset"
$target = Join-Path $userRoot "skills\dummy-dataset"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-execution/skills/dummy-dataset"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\dummy-dataset") -Destination $target -Recurse
```

Both scopes copy only this skill and run no upstream script or dependency.

## CodeArts configuration

The skill itself requires no `codearts_cli.json` change. Install the CLI using the [official instructions](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0005.html), select a real `provider/model` with `codearts models`, and keep credentials only in local configuration or environment variables.

This skill writes files. Interactive use can approve each request under the [official permission model](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0006.html). In non-interactive mode, CLI 26.8.1 auto-rejected `ask` even with `--auto`. The prompts therefore use a removable `KERNEL_DATA_DIR` and complete temporary `global.json`, without changing persistent user permissions. The test calls `codearts.exe` directly and reproduces the official `codearts.cmd` process environment. `NODE_TLS_REJECT_UNAUTHORIZED=0` reflects that launcher version and should not be copied to unrelated tools.

## Verify

Use `codearts debug skill` to check the exact location, then run the prompt's complete write smoke test. Passing requires a completed skill event, completed write event, exact source path, and a CSV on disk satisfying every constraint. Afterward, `Import-Csv .\dummy-commute-tasks.csv` should return six rows.

## Use

```text
Call the skill tool with name dummy-dataset. Generate 50 CSV rows for order-import testing: order_id must be unique, amount must be 1.00-999.99, currency must be HKD or USD, and refund_reason must be non-empty whenever refunded=true. Use synthetic values only and include no real personal data.
```

Review generated scripts, SQL, and large datasets before executing or importing them. Realistic-looking data is not production data.

## Update

Review the new commit and `pm-execution/skills/dummy-dataset` first. Remove only the old targets in the selected scope, reinstall at the new fixed commit, and repeat full verification. A floating `main` is not a verified version.

## Remove

Project removal deletes only `.codeartsdoer/skills/dummy-dataset` and `.codeartsdoer/vendor/pm-skills-dummy-dataset`. User removal deletes only `~/.codeartsdoer/skills/dummy-dataset` and `~/.codeartsdoer/vendor/pm-skills-dummy-dataset`. Never delete the `.codeartsdoer` root, user configuration, another skill, or generated data.

## Verified versions and result

| Item | Verified value |
| --- | --- |
| Compatibility | **Works** |
| Upstream | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| Upstream version | v2.1.0 |
| Pinned source | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| Verified skill | `dummy-dataset` |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Test model | `mimo/mimo-v2.5` |
| Verified scopes | Project A, fresh project B, user |
| Last verified | 2026-09-09 |

All three scopes discovered and loaded the exact path, emitted one completed skill event and one completed write event, and produced CSVs that passed row, field, range, dependency, and no-personal-data checks. Installations, artifacts, and temporary permissions were precisely rolled back. User `package.json`, `codearts_cli.json`, and persistent permission hashes were unchanged.

## Known limitations

Only a six-row direct CSV was tested. JSON, SQL, Python generators, large datasets, relational integrity, statistical distributions, script execution, and database import were not tested. Synthetic data can still be biased, violate business edges, or resemble real identities and must be reviewed before use.

Coverage is limited to Windows 11, CLI 26.8.1, `mimo/mimo-v2.5`, the pinned commit, and the isolated-write flow above. Linux, macOS, other models, Claude Commands, and complete plugin orchestration were not tested.

## Security

- Pin the commit and copy only `pm-execution/skills/dummy-dataset`; it has no npm/Python dependency, lifecycle script, binary, or telemetry.
- Temporary permissions allow Write/Edit only for the test workspace and deny shell, reads, browsing, and external-directory access; remove the exact temporary directory afterward.
- Never execute model-generated Python, SQL, or shell before human review.
- Do not supply real personal data, credentials, or production records.

## Evidence and sources

- [2026-09-09 runtime record](../../research/2026-09-09.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI permissions](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0006.html)
- [Pinned skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-execution/skills/dummy-dataset/SKILL.md)
- [Upstream repository](https://github.com/phuryn/pm-skills)
