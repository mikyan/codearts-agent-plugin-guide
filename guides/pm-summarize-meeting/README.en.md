# PM Meeting Summaries on CodeArts CLI

[简体中文](README.md)

Install the verified `summarize-meeting` Skill from PM Skills to turn meeting transcripts into structured summaries with clear participants, decisions, actions, and open questions.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project-root>/.codeartsdoer` | Pinning a version with a meeting-materials repository and sharing it with a team. Recommended. |
| User | `~/.codeartsdoer` | Summarizing meetings across several projects for one user. |

CodeArts documents project precedence for same-named Skills. Install one scope only, and stop instead of overwriting an existing Skill or vendor directory.

## Let an Agent install it

### Project installation prompt

```text
Install and verify the phuryn/pm-skills summarize-meeting Skill for CodeArts CLI in the current project, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow these requirements exactly:
1. Install only to .codeartsdoer/vendor/pm-skills-summarize-meeting and .codeartsdoer/skills/summarize-meeting in the current project. Do not change ~/.codeartsdoer, credentials, any root package.json, codearts_cli.json, or another Skill.
2. From the project root run codearts --version, git --version, and codearts models. If several models are available, ask me to choose the exact provider/model ID.
3. Check both installation targets. If either exists, stop and report the collision without overwriting it.
4. Run this verbatim in PowerShell from the project root:
   $projectRoot = (Get-Location).Path
   $source = Join-Path $projectRoot ".codeartsdoer\vendor\pm-skills-summarize-meeting"
   $target = Join-Path $projectRoot ".codeartsdoer\skills\summarize-meeting"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/summarize-meeting"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\summarize-meeting") -Destination $target -Recurse
5. Run codearts debug skill and confirm that summarize-meeting resolves to the current project's .codeartsdoer/skills/summarize-meeting/SKILL.md.
6. Replace <selected-model> with the model from step 2, then run this verbatim from the project root:
   $prompt = "Call the skill tool exactly once with name summarize-meeting. Use no other tool, do not access the network, and do not read or write files. Summarize only this complete synthetic transcript: Meeting on 2026-09-10 from 09:00 to 09:20 HKT. Participants: Mei, product researcher; Arun, engineer; Sofia, accessibility tester. Topic: CommuteCalm pilot readiness. Mei reported five students completed the paper-prototype test, but demand is not validated. Arun said missing-time review is ready and encrypted sync is blocked on an external security review. Sofia found keyboard labels missing on two screens. Decision: pilot stays local-only and starts after keyboard labels are fixed. Actions: Arun fixes keyboard labels by 2026-09-12; Sofia retests by 2026-09-13; Mei recruits 15 more pilot users by 2026-09-16. Open question: whether students want paid sync. Produce sections for meeting summary, date and time, participants, topic, summary, action items, decisions made, and open questions. Preserve owners and dates, state demand is not validated, and do not invent facts. Return the summary directly."
   codearts run -m "<selected-model>" --format json $prompt
7. Pass only if there is exactly one completed summarize-meeting Skill event resolved from the step 4 target, no other tool event, and a result that preserves the date and time, all three participants and roles, three action owners and dates, both decisions, unvalidated demand, and the paid-sync open question. Chinese or English section headings are acceptable. It must not add people, actions, or conclusions.
8. Report the commit, source, target, completed event, resolved path, and factual fidelity. Stop on any failure and do not claim success.
9. Removal may delete only .codeartsdoer/skills/summarize-meeting and .codeartsdoer/vendor/pm-skills-summarize-meeting. Do not delete .codeartsdoer itself, configuration, credentials, other Skills, transcripts, or saved summaries.
```

### User installation prompt

```text
Install and verify the user-scoped phuryn/pm-skills summarize-meeting Skill for CodeArts CLI on Windows, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow these requirements exactly:
1. Set $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Install only to $userRoot/vendor/pm-skills-summarize-meeting and $userRoot/skills/summarize-meeting. Do not change package.json, codearts_cli.json, permission files, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. If several models are available, ask me to choose the exact provider/model ID.
3. Check both installation targets. If either exists, stop without overwriting it.
4. Run this verbatim in PowerShell:
   $source = Join-Path $userRoot "vendor\pm-skills-summarize-meeting"
   $target = Join-Path $userRoot "skills\summarize-meeting"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-execution/skills/summarize-meeting"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\summarize-meeting") -Destination $target -Recurse
5. Create a clean directory without a same-named project Skill:
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-summarize-meeting-user-verify"
   if (Test-Path -LiteralPath $verifyRoot) { throw "Verification directory already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path $verifyRoot | Out-Null
   if (Test-Path -LiteralPath (Join-Path $verifyRoot ".codeartsdoer\skills\summarize-meeting")) { throw "Project override exists." }
   Set-Location -LiteralPath $verifyRoot
6. Run codearts debug skill and confirm the location is $userRoot/skills/summarize-meeting/SKILL.md.
7. Replace <selected-model> with the step 2 model and run this verbatim from that directory:
   $prompt = "Call the skill tool exactly once with name summarize-meeting. Use no other tool, do not access the network, and do not read or write files. Summarize only this complete synthetic transcript: Meeting on 2026-09-10 from 09:00 to 09:20 HKT. Participants: Mei, product researcher; Arun, engineer; Sofia, accessibility tester. Topic: CommuteCalm pilot readiness. Mei reported five students completed the paper-prototype test, but demand is not validated. Arun said missing-time review is ready and encrypted sync is blocked on an external security review. Sofia found keyboard labels missing on two screens. Decision: pilot stays local-only and starts after keyboard labels are fixed. Actions: Arun fixes keyboard labels by 2026-09-12; Sofia retests by 2026-09-13; Mei recruits 15 more pilot users by 2026-09-16. Open question: whether students want paid sync. Produce sections for meeting summary, date and time, participants, topic, summary, action items, decisions made, and open questions. Preserve owners and dates, state demand is not validated, and do not invent facts. Return the summary directly."
   codearts run -m "<selected-model>" --format json $prompt
8. Apply the same pass criteria as project scope: one completed target Skill, the exact user path, no other tool event, and a faithful summary of every participant, action, decision, and open question. After recording evidence, remove only $verifyRoot.
9. Report the commit, source, target, completed event, resolved path, and result. Stop on any failure.
10. Removal may delete only $userRoot/skills/summarize-meeting and $userRoot/vendor/pm-skills-summarize-meeting. Do not delete user configuration, permissions, credentials, other Skills, transcripts, or saved summaries.
```

## Manual Windows installation

Run `codearts models` first. For project scope, run from the project root:

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-summarize-meeting"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\summarize-meeting"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Existing target; stop." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-execution/skills/summarize-meeting"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
Copy-Item -LiteralPath (Join-Path $source "pm-execution\skills\summarize-meeting") -Destination $target -Recurse
```

For user scope, replace the first two lines with:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-summarize-meeting"
$target = Join-Path $userRoot "skills\summarize-meeting"
```

The remaining commands are identical. Copy only this Skill; do not execute upstream scripts or install dependencies.

## CodeArts configuration

The Skill does not change `codearts_cli.json`. Install the CLI using the [official instructions](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0005.html), select a real `provider/model` with `codearts models`, and keep credentials in local configuration or environment variables.

## Verification

Confirm the exact source with `codearts debug skill`, then run the read-only smoke test from the Agent prompt. Success requires the completed target Skill event, its intended path, and a factually faithful summary. CodeArts may produce Chinese headings according to model language, so verify semantic fields instead of English labels alone.

## Usage

```text
Call the skill tool with name summarize-meeting. Summarize only the complete transcript I provide. Include date and time, participants and roles, topic, discussion summary, an action-item table, decisions, and open questions. Preserve every owner and deadline. Write UNKNOWN for missing information and do not invent facts. Return the summary directly without reading or writing files.
```

If a file is needed, save the reviewed final text as Markdown separately; this verification did not authorize the Skill to write files.

## Updating

Review the new commit and `pm-execution/skills/summarize-meeting`, remove only the old targets for the selected scope, reinstall at the new pinned commit, and repeat full verification. Do not treat floating `main` as verified.

## Removal

Project scope removes only `.codeartsdoer/skills/summarize-meeting` and `.codeartsdoer/vendor/pm-skills-summarize-meeting`. User scope removes only `~/.codeartsdoer/skills/summarize-meeting` and `~/.codeartsdoer/vendor/pm-skills-summarize-meeting`. Preserve transcripts, summaries, configuration, and other Skills.

## Verified versions and result

| Item | Verified value |
| --- | --- |
| Compatibility | **Works** |
| Upstream | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| Upstream version | v2.1.0 |
| Pinned source | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| Verified Skill | `summarize-meeting` |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Test model | `mimo/mimo-v2.5` |
| Verified scopes | Project A, fresh project B, user |
| Last verified | 2026-09-10 |

All three scopes were precisely discovered and loaded from their intended paths and emitted only one completed Skill event. Every summary preserved the meeting time, all three participants, all three action owners and dates, both decisions, unvalidated demand, and the paid-sync open question. Two Chinese outputs passed manual semantic review. Every installation rolled back exactly, and user configuration, root manifest, and persistent permission hashes remained unchanged.

## Known limitations

Testing covered one short synthetic English transcript and direct text output. It did not cover audio transcription, long meetings, overlapping speakers, attachment reads, file writes, multi-meeting synthesis, real personal data, Cantonese transcripts, or other models. A summary may omit tone and context and does not replace the source record or participant confirmation.

## Security

- The pinned installation copies one `SKILL.md` with no dependency, script, binary, telemetry, or runtime network requirement.
- Real meeting records may contain personal data, trade secrets, or sensitive decisions. Obtain authorization, minimize content, and follow retention rules.
- Check names, dates, decisions, and actions against the transcript; never restate an inference as a decision.

## Evidence and sources

- [2026-09-10 hands-on record](../../research/2026-09-10.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI commands](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0034.html)
- [Pinned Skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-execution/skills/summarize-meeting/SKILL.md)
