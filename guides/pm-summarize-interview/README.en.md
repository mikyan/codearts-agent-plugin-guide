# PM Interview Summaries on CodeArts CLI

[简体中文](README.md)

Install the verified `summarize-interview` skill from PM Skills to turn customer-interview transcripts into Markdown summaries with JTBD, satisfaction signals, key insights, and clear actions.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Research-repository pinning, team sharing, or one project. Recommended by default. |
| User | `~/.codeartsdoer` | Repeated interview synthesis across projects for the current user. |

Official CodeArts rules give a same-named project skill priority. Do not install in both scopes. Stop on a same-named skill or vendor directory; never overwrite it.

## Install with an agent

Verification creates `interview-summary-mei-2026-09-09.md`. It is user output and must not be deleted when the skill is removed.

### Project installation prompt

```text
Install and verify the summarize-interview skill from phuryn/pm-skills for CodeArts CLI in the current project, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow exactly:
1. Install only to this project's .codeartsdoer/vendor/pm-skills-summarize-interview and .codeartsdoer/skills/summarize-interview. Do not modify ~/.codeartsdoer, credentials, any root package.json, or codearts_cli.json.
2. From the project root run codearts --version, git --version, and codearts models. If several models are available, ask me to select the exact provider/model ID.
3. Check both installation targets and interview-summary-mei-2026-09-09.md in the project root. Stop and report if any exists; never overwrite.
4. From the project root in PowerShell run exactly:
   $projectRoot = (Get-Location).Path
   $source = Join-Path $projectRoot ".codeartsdoer\vendor\pm-skills-summarize-interview"
   $target = Join-Path $projectRoot ".codeartsdoer\skills\summarize-interview"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-product-discovery/skills/summarize-interview"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\summarize-interview") -Destination $target -Recurse
5. Run codearts debug skill and confirm summarize-interview resolves to this project's .codeartsdoer/skills/summarize-interview/SKILL.md.
6. Create a one-run permission directory for non-interactive writing without changing the user's persistent permission/global.json:
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-summarize-interview-project-verify"
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
7. Replace <selected model> with step 2's model and run the real CodeArts 26.8.1 executable from the project root:
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
   $prompt = "Call the skill tool exactly once with name summarize-interview, then use the write tool exactly once to create interview-summary-mei-2026-09-09.md in the current workspace. Use no other tool, do not access the network, and do not read other files. Summarize this complete synthetic transcript only: Interview 2026-09-09 10:00 HKT. Participant Mei, commuter undergraduate; interviewer product researcher. Mei plans with paper on the MTR because it opens instantly and needs no login. She likes crossing items off. She often misses deadlines when tutors change dates in chat. She tried a cloud planner but stopped because setup took 30 minutes and she worried about uploading course notes. She wants tomorrow's urgent items in under 10 seconds, offline, with clear keyboard labels. Satisfaction with paper: mixed. Quote: I need the next thing, not another system to maintain. Unknown: willingness to pay, laptop use, sync need. Action: researcher to test a paper prototype by 2026-09-16. Use the skill template, retain the quote exactly, use - for unavailable fields, separate evidence from inference, and do not invent facts. The saved file must include exact markers **Date**, **Participants**, **Background**, **Current Solution**, **What They Like About Current Solution**, **Problems With Current Solution**, **Key Insights**, **Action Items**, JTBD, mixed, I need the next thing, not another system to maintain., UNKNOWN, and 2026-09-16. Finish by stating only that interview-summary-mei-2026-09-09.md was created."
   & $codeartsExe run -m "<selected model>" --auto --format json $prompt
8. Passing requires exactly one completed summarize-interview skill event and one completed write event. The skill must resolve to the step 4 target. The file must contain every template field, JTBD, mixed satisfaction, the exact quote, all three UNKNOWN items, and 2026-09-16. It must add no person, fact, payment/sync conclusion, or action. A file without events, or text without a file, does not pass.
9. After recording evidence, delete only $verifyRoot, not the generated summary. Report the commit, source, target, events, resolved path, and factual-fidelity result. Stop after any failure and do not claim success.
10. Removal may delete only .codeartsdoer/skills/summarize-interview and .codeartsdoer/vendor/pm-skills-summarize-interview. Never delete .codeartsdoer itself, configuration, credentials, another skill, a transcript, or a summary.
```

### User installation prompt

```text
Install and verify the user-scoped summarize-interview skill from phuryn/pm-skills for the current Windows user, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow exactly:
1. Set $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Install only to $userRoot/vendor/pm-skills-summarize-interview and $userRoot/skills/summarize-interview. Do not modify package.json, codearts_cli.json, persistent permission/global.json, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. If several models are available, ask me to select the exact provider/model ID.
3. Check both installation targets. Stop if either exists; never overwrite.
4. In PowerShell run exactly:
   $source = Join-Path $userRoot "vendor\pm-skills-summarize-interview"
   $target = Join-Path $userRoot "skills\summarize-interview"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-product-discovery/skills/summarize-interview"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\summarize-interview") -Destination $target -Recurse
5. Create an isolated workspace without a project override and a complete temporary permission file:
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-summarize-interview-user-verify"
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
   if (Test-Path -LiteralPath (Join-Path $workspace ".codeartsdoer\skills\summarize-interview")) { throw "Project override exists." }
6. Change to $workspace, run codearts debug skill, and confirm the location is $userRoot/skills/summarize-interview/SKILL.md.
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
   $prompt = "Call the skill tool exactly once with name summarize-interview, then use the write tool exactly once to create interview-summary-mei-2026-09-09.md in the current workspace. Use no other tool, do not access the network, and do not read other files. Summarize this complete synthetic transcript only: Interview 2026-09-09 10:00 HKT. Participant Mei, commuter undergraduate; interviewer product researcher. Mei plans with paper on the MTR because it opens instantly and needs no login. She likes crossing items off. She often misses deadlines when tutors change dates in chat. She tried a cloud planner but stopped because setup took 30 minutes and she worried about uploading course notes. She wants tomorrow's urgent items in under 10 seconds, offline, with clear keyboard labels. Satisfaction with paper: mixed. Quote: I need the next thing, not another system to maintain. Unknown: willingness to pay, laptop use, sync need. Action: researcher to test a paper prototype by 2026-09-16. Use the skill template, retain the quote exactly, use - for unavailable fields, separate evidence from inference, and do not invent facts. The saved file must include exact markers **Date**, **Participants**, **Background**, **Current Solution**, **What They Like About Current Solution**, **Problems With Current Solution**, **Key Insights**, **Action Items**, JTBD, mixed, I need the next thing, not another system to maintain., UNKNOWN, and 2026-09-16. Finish by stating only that interview-summary-mei-2026-09-09.md was created."
   & $codeartsExe run -m "<selected model>" --auto --format json $prompt
8. Passing is identical to project scope: one completed target skill, one completed write, exact user-scope path, every template field, and all fidelity assertions. Record evidence, then delete only $verifyRoot, not another temporary directory or user file.
9. Report the commit, source, target, events, resolved path, and result. Stop after any failure and do not claim success.
10. Removal may delete only $userRoot/skills/summarize-interview and $userRoot/vendor/pm-skills-summarize-interview. Never delete user configuration, persistent permissions, credentials, another skill, a transcript, or a summary.
```

## Manual installation on Windows

```powershell
codearts --version
git --version
codearts models
```

From the project root:

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-summarize-interview"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\summarize-interview"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-product-discovery/skills/summarize-interview"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\summarize-interview") -Destination $target -Recurse
```

For user scope, run:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-summarize-interview"
$target = Join-Path $userRoot "skills\summarize-interview"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-product-discovery/skills/summarize-interview"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-product-discovery\skills\summarize-interview") -Destination $target -Recurse
```

Copy only this skill and run no upstream script or dependency.

## CodeArts configuration

The skill itself does not change `codearts_cli.json`. Install the CLI using the [official instructions](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0005.html), choose a real model with `codearts models`, and keep credentials in local configuration or environment variables.

Interactive file writes can be approved under the [official permission model](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0006.html). CLI 26.8.1 auto-rejected non-interactive `ask` even with `--auto`; the prompts therefore use a removable isolated `KERNEL_DATA_DIR`, a temporary permission JSON, and direct `codearts.exe` invocation with the official launcher environment. `NODE_TLS_REJECT_UNAUTHORIZED=0` reflects that launcher version and should not spread to unrelated tools.

## Verify

Confirm the exact source with `codearts debug skill`, then run the complete smoke test. Passing requires completed skill and write events, the file on disk, all template fields, and factual fidelity. Use a synthetic transcript before real interviews; real content requires organizational authorization and a defined data-handling boundary.

## Use

```text
Call the skill tool with name summarize-interview. Read the complete transcript I provide and summarize Current Solution, JTBD, satisfaction, Problems, Key Insights, and Action Items using the template. Preserve direct quotes verbatim, use - for missing information, label inferences separately, and invent no facts.
```

## Update

Review the new commit and `pm-product-discovery/skills/summarize-interview`, remove only the old targets in the chosen scope, reinstall at the new fixed commit, and repeat full verification. A floating `main` is not verified.

## Remove

Project removal deletes only `.codeartsdoer/skills/summarize-interview` and `.codeartsdoer/vendor/pm-skills-summarize-interview`. User removal deletes only `~/.codeartsdoer/skills/summarize-interview` and `~/.codeartsdoer/vendor/pm-skills-summarize-interview`. Never delete transcripts, summaries, configuration, or other skills.

## Verified versions and result

| Item | Verified value |
| --- | --- |
| Compatibility | **Works** |
| Upstream | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| Upstream version | v2.1.0 |
| Pinned source | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| Verified skill | `summarize-interview` |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Test model | `mimo/mimo-v2.5` |
| Verified scopes | Project A, fresh project B, user |
| Last verified | 2026-09-09 |

All three scopes discovered and loaded the exact path and emitted one completed skill plus one completed write. Every summary retained the quote, unknowns, JTBD, satisfaction, and action date without adding people or conclusions. Installations, artifacts, and temporary permissions were precisely rolled back; user configuration and persistent-permission hashes were unchanged.

## Known limitations

Only a short synthetic English transcript and Markdown output were tested. PDFs, audio transcription, long interviews, multiple speakers, Chinese/Cantonese transcripts, attachment reads, cross-interview synthesis, and other models were not tested. Summaries can omit tone and context and do not replace the source record or researcher review.

## Security

- Pin the commit and copy only `pm-product-discovery/skills/summarize-interview`; it has no dependency, script, binary, telemetry, or runtime network requirement.
- Real transcripts may contain personal data, trade secrets, or sensitive research. Obtain authorization, minimize content, and enforce retention rules.
- Isolated permissions allow Write/Edit only in the test workspace and deny shell, reads, browsing, and external-directory access; remove the exact temporary directory afterward.
- Verify every quote, date, name, and action against the source and label inferences.

## Evidence and sources

- [2026-09-09 runtime record](../../research/2026-09-09.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI permissions](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0006.html)
- [Pinned skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-product-discovery/skills/summarize-interview/SKILL.md)
- [Upstream repository](https://github.com/phuryn/pm-skills)
