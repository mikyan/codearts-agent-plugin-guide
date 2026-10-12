# Use what-could-break in CodeArts CLI

[中文](README.md)

Install the `what-could-break` Skill so the Agent searches beyond a diff for real consumers of a shared or multi-file change and runs real code to prove whether the change is safe.

## Choose an installation scope

Project scope under `<project>/.codeartsdoer` affects only that repository. User scope under `~/.codeartsdoer` affects every project. Choose one. CodeArts documents project Skills as taking precedence over same-named user Skills; stop instead of overwriting if the Skill, vendor, or same name in the other scope already exists.

## Ask the Agent to install it

The prompts below create a `codearts-what-could-break-smoke` directory whose complete contents are specified. Stop if it or any target exists; never run this smoke test over real same-named files.

### Project-scope prompt

```text
Install and verify the project-scoped what-could-break Skill in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v19.4.0 and commit 9f2c979c0841ad3f24cc42e38f9f7cb377034558. Copy the complete skills/what-could-break directory (SKILL.md and references/LICENSE-pstack.txt) to <project>/.codeartsdoer/skills/what-could-break and keep the sparse clone at <project>/.codeartsdoer/vendor/agentic-awesome-what-could-break. From the project root, first run codearts --version, git --version, node --version, and codearts models, then ask me to choose a real available <model>. Never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-what-could-break', $target=Join-Path $root 'skills\what-could-break', and $smoke=Join-Path (Get-Location) 'codearts-what-could-break-smoke'. Stop if $source, $target, $smoke, or ~/.codeartsdoer/skills/what-could-break exists. Do not modify package.json, codearts_cli.json, credentials, plugins, permission files, other Skills, or existing project files.
From the project root run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/what-could-break; require git -C $source rev-parse HEAD to equal 9f2c979c0841ad3f24cc42e38f9f7cb377034558 exactly; Copy-Item -LiteralPath (Join-Path $source 'skills\what-could-break') -Destination $target -Recurse; require both $target/SKILL.md and $target/references/LICENSE-pstack.txt.
Create new $smoke, $smoke/src, and $smoke/clients directories. Create only these six files with exactly equivalent contents:
1. $smoke/package.json: {"type":"module"}
2. $smoke/src/settings.mjs: export function serializeSettings(input) { newline two spaces then return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total }); newline }
3. $smoke/src/proposed-settings.mjs: export function serializeSettings(input) { newline two spaces then return JSON.stringify({ theme: input.theme }); newline }
4. $smoke/clients/mobile.mjs: export function renderLegacyTotal(payload) { newline two spaces then const parsed = JSON.parse(payload); newline two spaces then if (typeof parsed.legacy_total !== 'number') throw new Error('legacy_total missing'); newline two spaces then return `Total: ${parsed.legacy_total}`; newline }
5. $smoke/verify.mjs: import { serializeSettings } from './src/proposed-settings.mjs'; newline import { renderLegacyTotal } from './clients/mobile.mjs'; newline try { newline two spaces then renderLegacyTotal(serializeSettings({ theme: 'dark', legacy_total: 42 })); newline two spaces then console.error('unexpected compatibility'); newline two spaces then process.exit(1); newline } catch (error) { newline two spaces then console.log(`expected incompatibility: ${error.message}`); newline }
6. $smoke/proposed.diff: --- a/src/settings.mjs newline +++ b/src/settings.mjs newline @@ newline -  return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total }); newline +  return JSON.stringify({ theme: input.theme });
Run codearts debug skill and require the sole name=what-could-break location to be $target/SKILL.md. Replace <model> with the selected provider/model ID and, from the project root, run exactly: codearts run -m "<model>" --format json --auto 'Call the skill tool exactly once with name what-could-break. Then inspect only the codearts-what-could-break-smoke directory using read-only Read, Glob, Grep, and Bash commands. Do not edit or create files, do not access the network, and do not inspect paths outside that directory. Review codearts-what-could-break-smoke/proposed.diff, find the concrete consumer that would break if legacy_total is removed, and run node codearts-what-could-break-smoke/verify.mjs exactly once as the real-code proof. Report the relevant file:line, the proof command and exit result, the evidence rung, and whether the proposed change is safe. Do not propose or apply a fix. End with the literal markers REAL_CODE_PROOF_RUN and CHANGE_UNSAFE.' Pass only with exit code 0, exactly one completed target Skill event, read-only search events, exactly one completed Bash event, and a final answer naming codearts-what-could-break-smoke/clients/mobile.mjs:3 or :3-4, node codearts-what-could-break-smoke/verify.mjs, output expected incompatibility: legacy_total missing, exit code 0, Rung 4, an unsafe verdict, and both markers. Report the commit, absolute path, and events. Before uninstalling, require $target and $source to be inside $root and $smoke to equal the exact current-project path. Delete only those three exact targets, then require the old path to disappear from codearts debug skill. Never delete the whole .codeartsdoer directory, project root, user root, root configuration, or any other file.
```

### User-scope prompt

```text
Install and verify what-could-break for the current Windows user. Pin https://github.com/sickn33/agentic-awesome-skills.git release v19.4.0 and commit 9f2c979c0841ad3f24cc42e38f9f7cb377034558. Copy the complete skills/what-could-break directory (SKILL.md and references/LICENSE-pstack.txt) to ~/.codeartsdoer/skills/what-could-break and keep the sparse clone at ~/.codeartsdoer/vendor/agentic-awesome-what-could-break. First run codearts --version, git --version, node --version, and codearts models, then ask me to choose a real available <model>. Never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-what-could-break', and $target=Join-Path $root 'skills\what-could-break'. Create a new empty `codearts-what-could-break-consumer` under the current directory, set $consumer=Join-Path (Get-Location) 'codearts-what-could-break-consumer', and $smoke=Join-Path $consumer 'codearts-what-could-break-smoke'. Stop if $source, $target, $consumer, or $smoke exists. Do not modify user package.json, codearts_cli.json, credentials, plugins, permission files, or other project configuration.
Run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent),$consumer -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/what-could-break; require git -C $source rev-parse HEAD to equal 9f2c979c0841ad3f24cc42e38f9f7cb377034558 exactly; Copy-Item -LiteralPath (Join-Path $source 'skills\what-could-break') -Destination $target -Recurse; require both $target/SKILL.md and $target/references/LICENSE-pstack.txt.
Create new $smoke, $smoke/src, and $smoke/clients directories. Create only these six files with exactly equivalent contents:
1. $smoke/package.json: {"type":"module"}
2. $smoke/src/settings.mjs: export function serializeSettings(input) { newline two spaces then return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total }); newline }
3. $smoke/src/proposed-settings.mjs: export function serializeSettings(input) { newline two spaces then return JSON.stringify({ theme: input.theme }); newline }
4. $smoke/clients/mobile.mjs: export function renderLegacyTotal(payload) { newline two spaces then const parsed = JSON.parse(payload); newline two spaces then if (typeof parsed.legacy_total !== 'number') throw new Error('legacy_total missing'); newline two spaces then return `Total: ${parsed.legacy_total}`; newline }
5. $smoke/verify.mjs: import { serializeSettings } from './src/proposed-settings.mjs'; newline import { renderLegacyTotal } from './clients/mobile.mjs'; newline try { newline two spaces then renderLegacyTotal(serializeSettings({ theme: 'dark', legacy_total: 42 })); newline two spaces then console.error('unexpected compatibility'); newline two spaces then process.exit(1); newline } catch (error) { newline two spaces then console.log(`expected incompatibility: ${error.message}`); newline }
6. $smoke/proposed.diff: --- a/src/settings.mjs newline +++ b/src/settings.mjs newline @@ newline -  return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total }); newline +  return JSON.stringify({ theme: input.theme });
Confirm that $consumer has no project Skill of the same name. Enter $consumer, run codearts debug skill, and require the sole name=what-could-break location to be $target/SKILL.md. Replace <model> with the chosen provider/model ID and run exactly from $consumer: codearts run -m "<model>" --format json --auto 'Call the skill tool exactly once with name what-could-break. Then inspect only the codearts-what-could-break-smoke directory using read-only Read, Glob, Grep, and Bash commands. Do not edit or create files, do not access the network, and do not inspect paths outside that directory. Review codearts-what-could-break-smoke/proposed.diff, find the concrete consumer that would break if legacy_total is removed, and run node codearts-what-could-break-smoke/verify.mjs exactly once as the real-code proof. Report the relevant file:line, the proof command and exit result, the evidence rung, and whether the proposed change is safe. Do not propose or apply a fix. End with the literal markers REAL_CODE_PROOF_RUN and CHANGE_UNSAFE.' Pass only with exit code 0, exactly one completed Skill event from $target, read-only search events, exactly one completed Bash event, and an answer naming codearts-what-could-break-smoke/clients/mobile.mjs:3 or :3-4, the fixed Node command, expected incompatibility: legacy_total missing, exit code 0, Rung 4, an unsafe verdict, and both markers. Report the commit, absolute path, and events. Verify absolute paths before uninstalling; delete only exact $target, $source, and the newly created $consumer, then run codearts debug skill from a directory without a project override and require the old path to disappear. Do not delete the user root, root configuration, credentials, or other Skills.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, `node --version`, and `codearts models` first. This example uses project scope; replace only `$root` for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # User scope: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-what-could-break'
$target=Join-Path $root 'skills\what-could-break'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/what-could-break
if((git -C $source rev-parse HEAD).Trim() -ne '9f2c979c0841ad3f24cc42e38f9f7cb377034558'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\what-could-break') -Destination $target -Recurse
if(-not (Test-Path (Join-Path $target 'references\LICENSE-pstack.txt'))){throw 'Incomplete copy'}
```

## CodeArts configuration

Use `codearts models` to select a real available `provider/model`. The Skill itself has no dependency and needs no change to `package.json`, `codearts_cli.json`, or permission files. Its representative invocation needs read-only search and one local Node proof, so use `--auto` only in an audited isolated workspace.

## Verify

Create `codearts-what-could-break-smoke` in the project or user-scope consumer root with these complete six-file contents:

```text
# package.json
{"type":"module"}

# src/settings.mjs
export function serializeSettings(input) {
  return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total });
}

# src/proposed-settings.mjs
export function serializeSettings(input) {
  return JSON.stringify({ theme: input.theme });
}

# clients/mobile.mjs
export function renderLegacyTotal(payload) {
  const parsed = JSON.parse(payload);
  if (typeof parsed.legacy_total !== 'number') throw new Error('legacy_total missing');
  return `Total: ${parsed.legacy_total}`;
}

# verify.mjs
import { serializeSettings } from './src/proposed-settings.mjs';
import { renderLegacyTotal } from './clients/mobile.mjs';
try {
  renderLegacyTotal(serializeSettings({ theme: 'dark', legacy_total: 42 }));
  console.error('unexpected compatibility');
  process.exit(1);
} catch (error) {
  console.log(`expected incompatibility: ${error.message}`);
}

# proposed.diff
--- a/src/settings.mjs
+++ b/src/settings.mjs
@@
-  return JSON.stringify({ theme: input.theme, legacy_total: input.legacy_total });
+  return JSON.stringify({ theme: input.theme });
```

Use `codearts debug skill` to confirm the sole absolute path, then run the exact `codearts run` command from the selected scope's Agent prompt at the project or consumer root. Success requires the target Skill event, a real `node codearts-what-could-break-smoke/verify.mjs` event, a concrete consumer location, actual output and exit code, Rung 4, and an unsafe verdict. A plausible text-only answer does not pass.

## Use

```text
Use what-could-break. Review this shared-interface change beyond its diff: identify real consumers and boundaries, then run the single most discriminating real-code proof. Separate observations, inferences, and unproven claims; report the command, exit code, and evidence rung. Do not modify code unless I explicitly ask next.
```

## Update

Before changing the pinned commit, review the Skill, bundled license, and every execution recommendation again, then repeat two fresh projects, a clean user consumer, the real Node proof, and rollback.

## Uninstall

Delete only `$root/skills/what-could-break` and `$root/vendor/agentic-awesome-what-could-break`. Remove the smoke fixture only after confirming it is the exact `codearts-what-could-break-smoke` path created by this guide. Finally use `codearts debug skill` to confirm the old absolute path is gone.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | Agentic Awesome `v19.4.0`; commit `9f2c979c0841ad3f24cc42e38f9f7cb377034558` |
| Files / SHA-256 | `SKILL.md` / `320611171BB21423CB6157D9504F5E377DD92742CFDBF2A7BE66BAF995982EC2`; `references/LICENSE-pstack.txt` / `0B179DE8C206D95074651DC54810D1278CBE3C7EDEF85F6D3EB09C7BD2C2F02F` |
| License | Skill frontmatter: MIT from `stas4000/what-could-break`; bundled pstack notice: MIT |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; Node 24.14.1; `mimo/mimo-v2.5` |
| Scope | Two fresh projects plus a user consumer without a project override; three main, three publication, and three exact guide-layout calls; 2026-10-12 |

## Known limitations

The Skill supplies a risk-discovery and proof method; it cannot guarantee that every consumer was found. Testing covered one Node ESM compatibility fixture, not large monorepos, compiled languages, remote services, databases, or production traffic. Constrain Bash/Read/Glob/Grep permissions to the authorized workspace. A search with no match is not proof that no risk exists.

## Security

The pinned directory contains only the Markdown Skill and an MIT notice: no manifest, lockfile, script, binary, downloader, runtime network access, credential, or telemetry. The proof script ran only in an isolated directory, accessed no network or production data, and made no project changes. All nine passing calls loaded the intended Skill and ran the fixed proof; user-root configuration and permission hashes were unchanged after rollback. One excluded experiment launched from the project subdirectory and proved that project Skills are not loaded upward, so the published command explicitly starts at the project root.

## Evidence and sources

- [中文研究记录](../../research/2026-10-12.md) · [English research](../../research/2026-10-12.en.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/9f2c979c0841ad3f24cc42e38f9f7cb377034558/skills/what-could-break)
- [Agentic Awesome v19.4.0](https://github.com/sickn33/agentic-awesome-skills/releases/tag/v19.4.0)
- [Original repository](https://github.com/stas4000/what-could-break)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
