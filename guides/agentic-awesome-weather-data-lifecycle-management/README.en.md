# Use weather-data-lifecycle-management in CodeArts CLI

[中文](README.md)

Install the `weather-data-lifecycle-management` Skill so the Agent assigns ownership and safe cleanup events to temporary weather data, interactive-session files, shared caches, and user exports before cleanup is implemented.

## Choose an installation scope

Project scope under `<project>/.codeartsdoer` affects only that repository. User scope under `~/.codeartsdoer` affects every project. Choose one. CodeArts documents project Skills as taking precedence over same-named user Skills; stop instead of overwriting if the Skill, vendor, or same name in the other scope already exists.

## Ask the Agent to install it

### Project-scope prompt

```text
Install and verify the project-scoped weather-data-lifecycle-management Skill in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v18.4.0 and commit 7b534bc15d833baf3bc98b3ca4fb23eda48342bb. Copy from skills/weather-data-lifecycle-management to <project>/.codeartsdoer/skills/weather-data-lifecycle-management and keep the clone in <project>/.codeartsdoer/vendor/agentic-awesome-weather-data-lifecycle-management. From the project root, first run codearts --version, git --version, and codearts models, then ask me to choose a real available <model>. Never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-weather-data-lifecycle-management', and $target=Join-Path $root 'skills\weather-data-lifecycle-management'. Stop if $source, $target, or ~/.codeartsdoer/skills/weather-data-lifecycle-management exists. Do not modify package.json, codearts_cli.json, credentials, plugins, permission files, or other Skills.
From the project root run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout --branch v18.4.0 --single-branch https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source sparse-checkout init --cone; git -C $source sparse-checkout set skills/weather-data-lifecycle-management; git -C $source checkout v18.4.0; require git -C $source rev-parse HEAD to equal 7b534bc15d833baf3bc98b3ca4fb23eda48342bb exactly; Copy-Item -LiteralPath (Join-Path $source 'skills\weather-data-lifecycle-management') -Destination $target -Recurse.
Run codearts debug skill and require the single name=weather-data-lifecycle-management location to be $target/SKILL.md. Replace <model> with the chosen provider/model ID and run exactly: codearts run --format json -m "<model>" 'Call the skill tool exactly once with name weather-data-lifecycle-management and use no other tool. Do not access or modify files, run commands, or access the network. Design the lifecycle for this synthetic desktop flow: worker creates request directory R with partial.grib2 and view.nc; a viewer may outlive the worker; validated cache entry C is shared by two viewers; final user export E is outside R; viewer construction can fail; cancellation can race with viewer close. Return an ownership table and exact events: worker cleans partials and R on failure, transfers view.nc ownership only after successful viewer creation, final consumer close performs one idempotent cleanup after handles close, shared cache C is never request cleanup, and export E is never automatically deleted. Explicitly refuse deletion when ownership is unknown and do not claim cleanup ran.' Pass only when the exit code is 0, exactly one name=weather-data-lifecycle-management/status=completed event comes from $target, no other tool event occurs, and the final answer includes the ownership table, failure cleanup, post-success ownership transfer, final-close cleanup, cache/export boundaries, and unknown-owner refusal. Report the commit, absolute path, and event. Uninstall only exact $target and $source, then run codearts debug skill and require the old path to be absent. Never delete the entire .codeartsdoer directory, project root, user root, or root configuration.
```

### User-scope prompt

```text
Install and verify weather-data-lifecycle-management for the current Windows user. Pin https://github.com/sickn33/agentic-awesome-skills.git release v18.4.0 and commit 7b534bc15d833baf3bc98b3ca4fb23eda48342bb. Copy from skills/weather-data-lifecycle-management to ~/.codeartsdoer/skills/weather-data-lifecycle-management and clone into ~/.codeartsdoer/vendor/agentic-awesome-weather-data-lifecycle-management. First run codearts --version, git --version, and codearts models, then ask me to choose a real available <model>. Never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-weather-data-lifecycle-management', and $target=Join-Path $root 'skills\weather-data-lifecycle-management'. Stop if $source, $target, or .codeartsdoer/skills/weather-data-lifecycle-management in the fresh verification consumer exists. Do not modify user package.json, codearts_cli.json, credentials, plugins, permission files, or project configuration.
Run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout --branch v18.4.0 --single-branch https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source sparse-checkout init --cone; git -C $source sparse-checkout set skills/weather-data-lifecycle-management; git -C $source checkout v18.4.0; require HEAD to equal 7b534bc15d833baf3bc98b3ca4fb23eda48342bb; Copy-Item -LiteralPath (Join-Path $source 'skills\weather-data-lifecycle-management') -Destination $target -Recurse. Enter a fresh consumer with no project Skill of the same name. Run codearts debug skill and require the sole name=weather-data-lifecycle-management location to be $target/SKILL.md. Replace <model> and run exactly: codearts run --format json -m "<model>" 'Call the skill tool exactly once with name weather-data-lifecycle-management and use no other tool. Do not access or modify files, run commands, or access the network. Design the lifecycle for this synthetic desktop flow: worker creates request directory R with partial.grib2 and view.nc; a viewer may outlive the worker; validated cache entry C is shared by two viewers; final user export E is outside R; viewer construction can fail; cancellation can race with viewer close. Return an ownership table and exact events: worker cleans partials and R on failure, transfers view.nc ownership only after successful viewer creation, final consumer close performs one idempotent cleanup after handles close, shared cache C is never request cleanup, and export E is never automatically deleted. Explicitly refuse deletion when ownership is unknown and do not claim cleanup ran.' Pass only with exit code 0, exactly one completed target Skill event, no other tool event, and all six result categories. Report the commit, absolute path, and event. Uninstall only exact $target, $source, and the verified consumer, then require the old path to disappear from codearts debug skill. Do not delete the user root, root configuration, credentials, or other Skills.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models` first. This example uses project scope; replace only `$root` for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # User scope: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-weather-data-lifecycle-management'
$target=Join-Path $root 'skills\weather-data-lifecycle-management'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout --branch v18.4.0 --single-branch https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set skills/weather-data-lifecycle-management
git -C $source checkout v18.4.0
if((git -C $source rev-parse HEAD).Trim() -ne '7b534bc15d833baf3bc98b3ca4fb23eda48342bb'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\weather-data-lifecycle-management') -Destination $target -Recurse
```

## CodeArts configuration

Use `codearts models` to choose a real available `provider/model`. This content-only Skill has no runtime dependency and does not require changes to `package.json`, `codearts_cli.json`, or permission files.

## Verify

Use `codearts debug skill` to confirm the sole absolute path, then run the full `codearts run` command from the prompts above. Success requires exactly one completed Skill event from the target directory, no other tool event, and a complete result covering ownership, transfer, failure, close, cache/export boundaries, and refusal for unknown ownership.

## Use

```text
Use weather-data-lifecycle-management. Assign an owner and deletion event to every temporary file, cache, and export in this weather workflow. Cover success, failure, cancellation, and final-consumer close, and refuse to delete paths with unknown ownership. Provide a plan only; do not delete files.
```

## Update

Before changing the pinned commit, review the Skill and license again, then repeat the two fresh-project tests, clean user consumer test, representative invocation, and rollback.

## Uninstall

Delete only `$root/skills/weather-data-lifecycle-management` and `$root/vendor/agentic-awesome-weather-data-lifecycle-management`, then use `codearts debug skill` to confirm the old absolute path is gone.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | Agentic Awesome `v18.4.0`; commit `7b534bc15d833baf3bc98b3ca4fb23eda48342bb` |
| Skill / SHA-256 | `weather-data-lifecycle-management` / `32FC91E27B27ED7F9AD188885BF82D26034E2754B7CE52630755FC9BFB0354F8` |
| License | Original non-code content: CC BY 4.0; repository code and tooling: MIT; Skill frontmatter says `source: self` |
| Environment | CodeArts CLI 26.8.1; Windows 11 25H2 build 26200; `mimo/mimo-v2.5` |
| Scope | Two fresh projects plus a user consumer with no project override; 2026-09-25 |

## Known limitations

The test covered read-only lifecycle design, not deletion execution. The Skill does not replace application locks, leases, cross-process coordination, or cache eviction. Before real cleanup, resolve and verify absolute paths, close handles, and test success, failure, cancellation, and races in the actual project.

## Security

The pinned directory contains only one Markdown `SKILL.md`: no manifest, lockfile, script, binary, downloader, runtime network access, credential handling, or telemetry. All six final main/publication invocations emitted only the target Skill event. User `package.json`, `codearts_cli.json`, and persistent permission hashes were unchanged after rollback.

## Evidence and sources

- [中文研究记录](../../research/2026-09-25.md) · [English research](../../research/2026-09-25.en.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/7b534bc15d833baf3bc98b3ca4fb23eda48342bb/skills/weather-data-lifecycle-management)
- [Agentic Awesome v18.4.0](https://github.com/sickn33/agentic-awesome-skills/releases/tag/v18.4.0)
- [AAS content license](https://github.com/sickn33/agentic-awesome-skills/blob/7b534bc15d833baf3bc98b3ca4fb23eda48342bb/LICENSE-CONTENT)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
