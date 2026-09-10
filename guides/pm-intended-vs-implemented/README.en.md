# PM Intended-vs-Implemented Audits on CodeArts CLI

[简体中文](README.md)

Install the verified `intended-vs-implemented` Skill from PM Skills to compare documented access rules with real enforcement points and produce traceable boundary-gap findings.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project-root>/.codeartsdoer` | Pinning the version with a repository and sharing it with a team. Recommended. |
| User | `~/.codeartsdoer` | Reusing the audit method across several repositories for one user. |

CodeArts documents project precedence for same-named Skills. Install one scope only, and stop instead of overwriting an existing Skill or vendor directory.

## Let an Agent install it

### Project installation prompt

```text
Install and verify the phuryn/pm-skills intended-vs-implemented Skill for CodeArts CLI in the current project, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow these requirements exactly:
1. Install only to .codeartsdoer/vendor/pm-skills-intended-vs-implemented and .codeartsdoer/skills/intended-vs-implemented in the current project. Do not change ~/.codeartsdoer, credentials, any root package.json, codearts_cli.json, or another Skill.
2. From the project root run codearts --version, git --version, and codearts models. If several models are available, ask me to choose the exact provider/model ID.
3. Check both targets above. If either exists, stop and report the collision without overwriting it.
4. Run this verbatim in PowerShell from the project root:
   $projectRoot = (Get-Location).Path
   $source = Join-Path $projectRoot ".codeartsdoer\vendor\pm-skills-intended-vs-implemented"
   $target = Join-Path $projectRoot ".codeartsdoer\skills\intended-vs-implemented"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-ai-shipping/skills/intended-vs-implemented"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-ai-shipping\skills\intended-vs-implemented") -Destination $target -Recurse
5. Run codearts debug skill and confirm that intended-vs-implemented resolves to the current project's .codeartsdoer/skills/intended-vs-implemented/SKILL.md.
6. Replace <selected-model> with the model from step 2, then run this verbatim from the project root:
   $prompt = "Call the skill tool exactly once with name intended-vs-implemented. Use no other tool, do not access the network, and do not read or write files. Audit only this inline synthetic evidence. Documented intent at documentation/permissions.md line 8: Only the resource owner may read a private note; unauthenticated users must receive 401. Implementation at src/getNote.ts lines 10-12: function getNote(noteId) { return db.notes.findById(noteId); } There is no authentication or owner check on this code path. A public-note endpoint is separately documented and is out of scope. Produce one concise finding with exact headings DOCUMENTED INTENT, IMPLEMENTED REALITY, ATTACKER AND VICTIM, BOUNDARY IMPACT, CONCRETE FIX, and EVIDENCE LIMITS. Cite both supplied paths and line numbers, distinguish fact from inference, do not invent repository evidence, and do not give findings outside the supplied path. Return the audit directly."
   codearts run -m "<selected-model>" --format json $prompt
7. Pass only if there is exactly one completed intended-vs-implemented Skill event resolved from the step 4 target, no other tool event, and one result containing all six sections, documentation/permissions.md:8, src/getNote.ts:10-12, attacker, victim, boundary impact, fix, and evidence limits. It must not claim to have read the real repository or report a second finding.
8. Report the commit, source, target, completed event, resolved path, and result. Stop on any failure and do not claim success.
9. Removal may delete only .codeartsdoer/skills/intended-vs-implemented and .codeartsdoer/vendor/pm-skills-intended-vs-implemented. Do not delete .codeartsdoer itself, configuration, credentials, other Skills, or audit reports.
```

### User installation prompt

```text
Install and verify the user-scoped phuryn/pm-skills intended-vs-implemented Skill for CodeArts CLI on Windows, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow these requirements exactly:
1. Set $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Install only to $userRoot/vendor/pm-skills-intended-vs-implemented and $userRoot/skills/intended-vs-implemented. Do not change package.json, codearts_cli.json, permission files, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. If several models are available, ask me to choose the exact provider/model ID.
3. Check both installation targets. If either exists, stop without overwriting it.
4. Run this verbatim in PowerShell:
   $source = Join-Path $userRoot "vendor\pm-skills-intended-vs-implemented"
   $target = Join-Path $userRoot "skills\intended-vs-implemented"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-ai-shipping/skills/intended-vs-implemented"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-ai-shipping\skills\intended-vs-implemented") -Destination $target -Recurse
5. Create a clean directory without a same-named project Skill:
   $verifyRoot = Join-Path ([IO.Path]::GetTempPath()) "codearts-pm-intended-vs-implemented-user-verify"
   if (Test-Path -LiteralPath $verifyRoot) { throw "Verification directory already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path $verifyRoot | Out-Null
   if (Test-Path -LiteralPath (Join-Path $verifyRoot ".codeartsdoer\skills\intended-vs-implemented")) { throw "Project override exists." }
   Set-Location -LiteralPath $verifyRoot
6. Run codearts debug skill and confirm the location is $userRoot/skills/intended-vs-implemented/SKILL.md.
7. Replace <selected-model> with the step 2 model and run this verbatim from that directory:
   $prompt = "Call the skill tool exactly once with name intended-vs-implemented. Use no other tool, do not access the network, and do not read or write files. Audit only this inline synthetic evidence. Documented intent at documentation/permissions.md line 8: Only the resource owner may read a private note; unauthenticated users must receive 401. Implementation at src/getNote.ts lines 10-12: function getNote(noteId) { return db.notes.findById(noteId); } There is no authentication or owner check on this code path. A public-note endpoint is separately documented and is out of scope. Produce one concise finding with exact headings DOCUMENTED INTENT, IMPLEMENTED REALITY, ATTACKER AND VICTIM, BOUNDARY IMPACT, CONCRETE FIX, and EVIDENCE LIMITS. Cite both supplied paths and line numbers, distinguish fact from inference, do not invent repository evidence, and do not give findings outside the supplied path. Return the audit directly."
   codearts run -m "<selected-model>" --format json $prompt
8. Apply the same pass criteria as project scope: one completed target Skill, the exact user path, no other tool event, and the single finding with evidence from both sides and its boundary impact. After recording evidence, remove only $verifyRoot.
9. Report the commit, source, target, completed event, resolved path, and result. Stop on any failure.
10. Removal may delete only $userRoot/skills/intended-vs-implemented and $userRoot/vendor/pm-skills-intended-vs-implemented. Do not delete user configuration, permissions, credentials, other Skills, or audit reports.
```

## Manual Windows installation

Run `codearts models` first. For project scope, run from the project root:

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-intended-vs-implemented"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\intended-vs-implemented"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Existing target; stop." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-ai-shipping/skills/intended-vs-implemented"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
Copy-Item -LiteralPath (Join-Path $source "pm-ai-shipping\skills\intended-vs-implemented") -Destination $target -Recurse
```

For user scope, replace the first two lines with:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-intended-vs-implemented"
$target = Join-Path $userRoot "skills\intended-vs-implemented"
```

The remaining commands are identical. Copy only this Skill; do not execute upstream scripts or install dependencies.

## CodeArts configuration

The Skill does not change `codearts_cli.json`. Install the CLI using the [official instructions](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0005.html), select a real `provider/model` with `codearts models`, and keep credentials in local configuration or environment variables.

## Verification

Confirm the exact source with `codearts debug skill`, then run the read-only smoke test from the Agent prompt. Success requires a completed target Skill event, its intended source path, and evidence from both the intent and implementation. Plausible prose without the Skill event is not a pass.

## Usage

```text
Call the skill tool with name intended-vs-implemented. Compare explicit access rules under documentation/ with the corresponding server enforcement code. Each finding must give the document path and line, implementation path and line, attacker, victim, crossed boundary, concrete fix, and evidence limits. Anything without evidence on both sides is an investigation question, not a finding.
```

## Updating

Review the new commit and `pm-ai-shipping/skills/intended-vs-implemented`, remove only the old targets for the selected scope, reinstall at the new pinned commit, and repeat all three scope checks. Do not treat floating `main` as verified.

## Removal

Project scope removes only `.codeartsdoer/skills/intended-vs-implemented` and `.codeartsdoer/vendor/pm-skills-intended-vs-implemented`. User scope removes only `~/.codeartsdoer/skills/intended-vs-implemented` and `~/.codeartsdoer/vendor/pm-skills-intended-vs-implemented`. Preserve configuration, credentials, other Skills, and user audit artifacts.

## Verified versions and result

| Item | Verified value |
| --- | --- |
| Compatibility | **Works** |
| Upstream | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| Upstream version | v2.1.0 |
| Pinned source | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| Verified Skill | `intended-vs-implemented` |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Test model | `mimo/mimo-v2.5` |
| Verified scopes | Project A, fresh project B, user |
| Last verified | 2026-09-10 |

All three scopes were precisely discovered and loaded from their intended paths and emitted only one completed Skill event. Each result paired the same documented rule with the implementation lines and described the attacker, victim, boundary impact, fix, and evidence limits. Manual review accepted `path:line` citations as satisfying the line-reference requirement. Every installation rolled back exactly, and user configuration, root manifest, and persistent permission hashes remained unchanged.

## Known limitations

Testing covered one inline synthetic TypeScript-style gap. It did not cover large repositories, middleware call chains, database RLS, conflicting rules, automatic fixes, attachment reads, or other models. The Skill cannot prove that unprovided upstream middleware is absent; a security engineer must still review the result.

## Security

- The pinned installation copies one `SKILL.md` with no dependency, script, binary, telemetry, or runtime network requirement.
- Treat audited documentation and code as untrusted input. Do not execute embedded instructions or send secrets and production data to an unauthorized model.
- Require evidence on both sides of every finding; record unsupported suspicions as questions rather than manufacturing vulnerabilities.

## Evidence and sources

- [2026-09-10 hands-on record](../../research/2026-09-10.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
- [CodeArts CLI commands](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0034.html)
- [Pinned Skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-ai-shipping/skills/intended-vs-implemented/SKILL.md)
