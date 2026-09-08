# PM Product Naming on CodeArts CLI

[简体中文](README.md)

Install the verified `product-name` skill from PM Skills to generate five candidate names and human-validation guidance from brand values, audience, tone, and naming constraints.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Repository-pinned versions, team sharing, or one project. Recommended by default. |
| User | `~/.codeartsdoer` | Repeated use across projects for the current user. |

Official CodeArts documentation gives a same-named project skill priority. Do not install in both scopes. Stop on any same-name collision; never overwrite it.

## Install with an agent

### Project installation prompt

```text
Install and verify the product-name skill from phuryn/pm-skills for CodeArts CLI in the current project, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow exactly:
1. Modify only this project's .codeartsdoer. Do not modify ~/.codeartsdoer, credentials, global software, or other project files.
2. From the project root run codearts --version, git --version, and codearts models. If several models are available, ask me to select a provider/model ID.
3. Check .codeartsdoer/vendor/pm-skills-product-name and .codeartsdoer/skills/product-name. Stop and report if either exists; never overwrite.
4. From the project root in PowerShell, run exactly:
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-product-name"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\product-name"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-marketing-growth/skills/product-name"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-marketing-growth\skills\product-name") -Destination $target -Recurse
5. Run codearts debug skill and confirm product-name resolves under this project's .codeartsdoer/skills/product-name/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly:
   codearts run --format json --model "<selected model>" "Call the skill tool exactly once with name product-name and use no other tool. Do not access the network or write files. Name a privacy-first offline study planner for Hong Kong commuter undergraduates. Brand values: calm, trustworthy, practical, non-surveillant. Tone: concise English names that Cantonese speakers can pronounce. Avoid the words AI, cloud, smart, study, task, and plan. Generate exactly 5 distinct names. For each give rationale, brand fit, memorability, pronunciation note, one risk, and domain/trademark status explicitly marked UNCHECKED because no search is allowed. Rank all five and recommend one for human validation, without claiming availability. Use exact markers CONTEXT, NAME-1, NAME-2, NAME-3, NAME-4, NAME-5, RATIONALE, BRAND-FIT, MEMORABILITY, PRONUNCIATION, RISK, UNCHECKED, RANKING, and RECOMMENDATION."
7. Passing criteria: one completed `product-name` skill event resolving to the selected native directory; exactly five names containing none of the blocked words; rationale, brand fit, memorability, pronunciation, risk, and `UNCHECKED` for every name; ranking and a human-validation recommendation. Never claim domain or trademark availability.
8. Report the commit, source, target, completed event, resolved path, and result. Removal may delete only .codeartsdoer/skills/product-name and .codeartsdoer/vendor/pm-skills-product-name. Never delete .codeartsdoer itself, package.json, codearts_cli.json, or another skill. Stop after any failure and do not claim success.
```

### User installation prompt

```text
Install and verify the user-scoped product-name skill from phuryn/pm-skills for the current Windows user, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow exactly:
1. In PowerShell set $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Modify only $userRoot/vendor/pm-skills-product-name and $userRoot/skills/product-name. Do not modify $userRoot/package.json, codearts_cli.json, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. If several models are available, ask me to select a provider/model ID.
3. Check the two exact targets above. Stop and report if either exists; never overwrite.
4. In the same PowerShell session run exactly:
   $source = Join-Path $userRoot "vendor\pm-skills-product-name"
   $target = Join-Path $userRoot "skills\product-name"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-marketing-growth/skills/product-name"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-marketing-growth\skills\product-name") -Destination $target -Recurse
5. Change to a clean directory with no same-named project skill. Run codearts debug skill and confirm the location is ~/.codeartsdoer/skills/product-name/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly from that clean directory:
   codearts run --format json --model "<selected model>" "Call the skill tool exactly once with name product-name and use no other tool. Do not access the network or write files. Name a privacy-first offline study planner for Hong Kong commuter undergraduates. Brand values: calm, trustworthy, practical, non-surveillant. Tone: concise English names that Cantonese speakers can pronounce. Avoid the words AI, cloud, smart, study, task, and plan. Generate exactly 5 distinct names. For each give rationale, brand fit, memorability, pronunciation note, one risk, and domain/trademark status explicitly marked UNCHECKED because no search is allowed. Rank all five and recommend one for human validation, without claiming availability. Use exact markers CONTEXT, NAME-1, NAME-2, NAME-3, NAME-4, NAME-5, RATIONALE, BRAND-FIT, MEMORABILITY, PRONUNCIATION, RISK, UNCHECKED, RANKING, and RECOMMENDATION."
7. Apply the same passing criteria: one completed event, the exact user path, five constrained names, `UNCHECKED` per name, and human-validation guidance; do not claim domain or trademark availability.
8. Report the commit, event, path, and result. Removal may delete only $userRoot/skills/product-name and $userRoot/vendor/pm-skills-product-name. Never delete the user-root package.json, codearts_cli.json, credentials, or another skill. Stop after any failure and do not claim success.
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
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-product-name"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\product-name"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-marketing-growth/skills/product-name"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-marketing-growth\skills\product-name") -Destination $target -Recurse
```

For user scope, run:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-product-name"
$target = Join-Path $userRoot "skills\product-name"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-marketing-growth/skills/product-name"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-marketing-growth\skills\product-name") -Destination $target -Recurse
```

Both flows clone the pinned commit, sparse-check out only `pm-marketing-growth/skills/product-name`, and run no upstream Python validator, installer, or dependency.

## CodeArts configuration

This skill requires no `codearts_cli.json` change. Install CodeArts CLI with the [official guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html), then choose a real `provider/model` ID with `codearts models`. For custom models, follow the [official configuration example](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html) and keep keys in local configuration or environment variables.

In this custom-provider test, CodeArts CLI 26.8.1 required `CODEARTS_CLI_AK` and `CODEARTS_CLI_SK` to exist in the process. Non-secret, non-persisted placeholders passed that preflight while the Provider's own key authenticated the model. This is observed behavior, not a compatibility guarantee. Use valid credentials from the [official AK/SK guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html) for Huawei Cloud-hosted models.

## Verify

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills | Where-Object { $_.name -eq "product-name" } | Select-Object name, location
```

Replace the model ID and run the complete `codearts run` command in the installation prompt. Require one completed `product-name` skill event resolving to the chosen native directory, exactly five constrained names, six fields per name, ranking, and human-validation guidance. Final prose without the event—or an unverified availability claim—is not a pass.

## Use

```text
Call the skill tool with name product-name. Generate five names from these brand, audience, pronunciation, and blocked-word constraints; mark domain and trademark status unchecked unless actually researched.
```

## Update

Review the new commit and `pm-marketing-growth/skills/product-name` first. Remove only the old targets in the selected scope, reinstall a pinned new commit, and repeat full verification. Do not treat floating `main` as verified.

## Remove

Project targets are `.codeartsdoer/skills/product-name` and `.codeartsdoer/vendor/pm-skills-product-name`. User targets are `~/.codeartsdoer/skills/product-name` and `~/.codeartsdoer/vendor/pm-skills-product-name`. Do not delete the `.codeartsdoer` root, user-root `package.json`, `codearts_cli.json`, credentials, or unrelated skills.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Works** |
| Upstream | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| Upstream version | v2.1.0 |
| Pinned source | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| Verified skill | `product-name` |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model | `mimo/mimo-v2.5` |
| Verified scopes | Project A, fresh project B, and user |
| Last verified | 2026-09-08 |

All three scopes passed installation, exact discovery, target-path loading, representative behavior, rollback, and post-removal diagnostics. The user-root `package.json` and `codearts_cli.json` SHA-256 hashes were unchanged.

## Known limitations

No live domain, trademark, language-ambiguity, Cantonese-user pronunciation, or market-acceptance research was performed. Generated names are ideas for human screening, not legal or brand availability conclusions.

Only Windows 11, CodeArts CLI 26.8.1, `mimo/mimo-v2.5`, the pinned commit, and the offline flow above were tested. Linux, macOS, other models, live naming research, Claude Commands, and full plugin orchestration were not tested.

## Security

- Pin the commit and copy only `pm-marketing-growth/skills/product-name`; do not run upstream `validate_plugins.py`, tests, or other plugin content.
- The test prompt supplies local facts and forbids network and file writes; `--auto` is unnecessary.
- Skill instructions influence agent behavior; review `SKILL.md` before use in a sensitive project.
- Installation and removal do not modify credentials, the user-root manifest, or CodeArts model configuration.

## Evidence and sources

- [2026-09-08 verification](../../research/2026-09-08.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-marketing-growth/skills/product-name/SKILL.md)
- [Upstream repository](https://github.com/phuryn/pm-skills)
