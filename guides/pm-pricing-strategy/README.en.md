# PM Pricing Strategy on CodeArts CLI

[简体中文](README.md)

Install the verified `pricing-strategy` skill from PM Skills to design a pricing model, tiers, and validation experiment from value, alternatives, cost, and willingness-to-pay evidence.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Repository-pinned versions, team sharing, or one project. Recommended by default. |
| User | `~/.codeartsdoer` | Repeated use across projects for the current user. |

Official CodeArts documentation gives a same-named project skill priority. Do not install in both scopes. Stop on any same-name collision; never overwrite it.

## Install with an agent

### Project installation prompt

```text
Install and verify the pricing-strategy skill from phuryn/pm-skills for CodeArts CLI in the current project, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow exactly:
1. Modify only this project's .codeartsdoer. Do not modify ~/.codeartsdoer, credentials, global software, or other project files.
2. Run codearts --version, git --version, and codearts models. If several models are available, ask me to select a provider/model ID.
3. Check .codeartsdoer/vendor/pm-skills-pricing-strategy and .codeartsdoer/skills/pricing-strategy. Stop and report if either exists; never overwrite.
4. From the project root in PowerShell, run exactly:
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-pricing-strategy"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\pricing-strategy"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-product-strategy/skills/pricing-strategy"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-product-strategy\skills\pricing-strategy") -Destination $target -Recurse
5. Run codearts debug skill and confirm pricing-strategy resolves under this project's .codeartsdoer/skills/pricing-strategy/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly:
   codearts run --format json --model "<selected model>" "Call the skill tool exactly once with name pricing-strategy and use no other tool. Do not access the network or write files. Design pricing for an offline-first study planner. Facts: students save about 2 hours monthly, alternatives are free calendars and a 6 USD monthly planner, local core has near-zero marginal cost, encrypted cross-device sync costs 0.50 USD per active user monthly, and survey willingness to pay centers at 4 USD. Recommend model and value metric, define Free/Pro/University tiers, state annual discount, list 3 assumptions and 3 risks, and design one ethical pricing experiment with threshold. Use exact markers MODEL, VALUE-METRIC, FREE, PRO, UNIVERSITY, ANNUAL-DISCOUNT, ASSUMPTIONS, RISKS, and EXPERIMENT."
7. Passing criteria: There must be one completed `pricing-strategy` skill event resolving to the selected native directory. The result must include a model, value metric, Free/Pro/University tiers, annual discount, three assumptions, three risks, and an ethical pricing experiment with a threshold.
8. Report the commit, source, target, completed event, resolved path, and result. Removal may delete only .codeartsdoer/skills/pricing-strategy and .codeartsdoer/vendor/pm-skills-pricing-strategy. Never delete .codeartsdoer itself, package.json, codearts_cli.json, or another skill. Stop after any failure and do not claim success.
```

### User installation prompt

```text
Install and verify the user-scoped pricing-strategy skill from phuryn/pm-skills for the current Windows user, pinned to commit 18468a95b427e70e258b51389796367c6f684e7d.

Follow exactly:
1. In PowerShell set $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Modify only $userRoot/vendor/pm-skills-pricing-strategy and $userRoot/skills/pricing-strategy. Do not modify $userRoot/package.json, codearts_cli.json, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. If several models are available, ask me to select a provider/model ID.
3. Check the two exact targets above. Stop and report if either exists; never overwrite.
4. In the same PowerShell session run exactly:
   $source = Join-Path $userRoot "vendor\pm-skills-pricing-strategy"
   $target = Join-Path $userRoot "skills\pricing-strategy"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "pm-product-strategy/skills/pricing-strategy"
   git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
   if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "pm-product-strategy\skills\pricing-strategy") -Destination $target -Recurse
5. Change to a clean directory with no same-named project skill. Run codearts debug skill and confirm the location is ~/.codeartsdoer/skills/pricing-strategy/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly from that clean directory:
   codearts run --format json --model "<selected model>" "Call the skill tool exactly once with name pricing-strategy and use no other tool. Do not access the network or write files. Design pricing for an offline-first study planner. Facts: students save about 2 hours monthly, alternatives are free calendars and a 6 USD monthly planner, local core has near-zero marginal cost, encrypted cross-device sync costs 0.50 USD per active user monthly, and survey willingness to pay centers at 4 USD. Recommend model and value metric, define Free/Pro/University tiers, state annual discount, list 3 assumptions and 3 risks, and design one ethical pricing experiment with threshold. Use exact markers MODEL, VALUE-METRIC, FREE, PRO, UNIVERSITY, ANNUAL-DISCOUNT, ASSUMPTIONS, RISKS, and EXPERIMENT."
7. Passing criteria: There must be one completed `pricing-strategy` skill event resolving to the selected native directory. The result must include a model, value metric, Free/Pro/University tiers, annual discount, three assumptions, three risks, and an ethical pricing experiment with a threshold.
8. Report the commit, completed event, resolved path, and result. Removal may delete only $userRoot/skills/pricing-strategy and $userRoot/vendor/pm-skills-pricing-strategy. Never delete the user-root package.json, codearts_cli.json, credentials, or another skill. Stop after any failure and do not claim success.
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
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\pm-skills-pricing-strategy"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\pricing-strategy"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-product-strategy/skills/pricing-strategy"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-product-strategy\skills\pricing-strategy") -Destination $target -Recurse
```

For user scope, run:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\pm-skills-pricing-strategy"
$target = Join-Path $userRoot "skills\pricing-strategy"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/phuryn/pm-skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "pm-product-strategy/skills/pricing-strategy"
git -C $source checkout --detach "18468a95b427e70e258b51389796367c6f684e7d"
if ((git -C $source rev-parse HEAD).Trim() -ne "18468a95b427e70e258b51389796367c6f684e7d") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "pm-product-strategy\skills\pricing-strategy") -Destination $target -Recurse
```

Both scopes copy only the selected skill and run no upstream Python validator, installer, or dependency.

## CodeArts configuration

This skill requires no `codearts_cli.json` change. Install CodeArts CLI with the [official guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html), then choose a real `provider/model` ID with `codearts models`. For custom models, follow the [official configuration example](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html) and keep keys in local configuration or environment variables.

In this custom-provider test, CodeArts CLI 26.8.1 required `CODEARTS_CLI_AK` and `CODEARTS_CLI_SK` to exist in the process. Non-secret, non-persisted placeholders passed that preflight while the Provider's own key authenticated the model. This is observed behavior, not a compatibility guarantee. Use valid credentials from the [official AK/SK guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html) for Huawei Cloud-hosted models.

## Verify

Check the resolved source path:

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills | Where-Object { $_.name -eq "pricing-strategy" } | Select-Object name, location
```

Then replace the model ID and perform the real call:

```powershell
codearts run --format json --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name pricing-strategy and use no other tool. Do not access the network or write files. Design pricing for an offline-first study planner. Facts: students save about 2 hours monthly, alternatives are free calendars and a 6 USD monthly planner, local core has near-zero marginal cost, encrypted cross-device sync costs 0.50 USD per active user monthly, and survey willingness to pay centers at 4 USD. Recommend model and value metric, define Free/Pro/University tiers, state annual discount, list 3 assumptions and 3 risks, and design one ethical pricing experiment with threshold. Use exact markers MODEL, VALUE-METRIC, FREE, PRO, UNIVERSITY, ANNUAL-DISCOUNT, ASSUMPTIONS, RISKS, and EXPERIMENT."
```

There must be one completed `pricing-strategy` skill event resolving to the selected native directory. The result must include a model, value metric, Free/Pro/University tiers, annual discount, three assumptions, three risks, and an ethical pricing experiment with a threshold. A plausible final answer without the completed target `skill` event is not a pass.

## Use

```text
Call the skill tool with name pricing-strategy. Design three pricing tiers from these value, cost, competitor, and willingness-to-pay facts and propose one decisive experiment.
```

## Update

Review a new commit and the selected skill directory first. Remove only the old targets in the chosen scope, reinstall the pinned new commit, and repeat full verification. Do not treat floating `main` as verified.

## Remove

Project targets only:

- `.codeartsdoer/skills/pricing-strategy`
- `.codeartsdoer/vendor/pm-skills-pricing-strategy`

User targets only:

- `~/.codeartsdoer/skills/pricing-strategy`
- `~/.codeartsdoer/vendor/pm-skills-pricing-strategy`

Do not delete the `.codeartsdoer` root, user-root `package.json`, `codearts_cli.json`, credentials, or unrelated skills.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Works** |
| Upstream | [phuryn/pm-skills](https://github.com/phuryn/pm-skills) |
| Upstream version | v2.1.0 |
| Pinned source | [18468a9](https://github.com/phuryn/pm-skills/commit/18468a95b427e70e258b51389796367c6f684e7d) |
| Verified skill | `pricing-strategy` |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model | `mimo/mimo-v2.5` |
| Verified scopes | Project A, fresh project B, and user |
| Last verified | 2026-09-03 |

All three scopes passed installation, exact discovery, target-path loading, representative behavior, rollback, and post-removal diagnostics. The user-root `package.json` and `codearts_cli.json` SHA-256 hashes were unchanged.

## Known limitations

No live competitor research, raw Van Westendorp data, taxes, or channel fees were tested; the output is a hypothesis to validate, not financial advice.

Only Windows 11, CodeArts CLI 26.8.1, `mimo/mimo-v2.5`, the pinned commit, and the representative flow above were tested. Linux, macOS, other models, Claude `/commands`, and full PM plugin orchestration were not tested.

## Security

- Pin the commit and copy only `pm-product-strategy/skills/pricing-strategy`; do not run upstream `validate_plugins.py`, tests, or other plugin content.
- The test prompt supplies complete local facts and forbids network and file writes; `--auto` is unnecessary.
- Skill instructions influence agent behavior; review `SKILL.md` before use in a sensitive project.
- Installation and removal do not modify credentials, the user-root manifest, or CodeArts model configuration.

## Evidence and sources

- [2026-09-03 verification](../../research/2026-09-03.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned skill](https://github.com/phuryn/pm-skills/blob/18468a95b427e70e258b51389796367c6f684e7d/pm-product-strategy/skills/pricing-strategy/SKILL.md)
- [Upstream repository](https://github.com/phuryn/pm-skills)
