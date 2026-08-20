# Use api-design-principles with CodeArts CLI

[简体中文](README.md)

Install `api-design-principles` to design REST APIs with consistent resources, pagination, and errors.

## Choose a scope

Choose one: `<project>/.codeartsdoer` for a repository pin or `~/.codeartsdoer` for reuse by the current user. Stop if either scope contains the same skill and trust `codearts debug skill` for the resolved source.

## Ask Agent to install it

### Project prompt

```text
Install and verify project-scoped api-design-principles from wshobson/agents at commit 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35.
1. Create only .codeartsdoer/vendor/wshobson-api-design-principles and .codeartsdoer/skills/api-design-principles. Do not modify user files, package.json, codearts_cli.json, credentials, or other skills.
2. Run codearts --version, git --version, and codearts models and ask me to select an available model. Stop on missing credentials. Check both project targets and ~/.codeartsdoer/skills/api-design-principles; stop on collision.
3. From the project root run in PowerShell:
   $s=Join-Path (Get-Location) '.codeartsdoer\vendor\wshobson-api-design-principles'; $t=Join-Path (Get-Location) '.codeartsdoer\skills\api-design-principles'
   New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/wshobson/agents.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/backend-development/skills/api-design-principles/' '/LICENSE'
   git -C $s checkout --detach 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
   if((git -C $s rev-parse HEAD).Trim() -ne '367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'plugins\backend-development\skills\api-design-principles') -Destination $t -Recurse
4. The sole matching location from codearts debug skill must be $t/SKILL.md. Replace <model> and run verbatim: codearts run --format json --sandbox --model "<model>" "Verification contract: call the skill tool exactly once with name api-design-principles. No other tool is allowed. Using only the loaded skill and this prompt, write a concise REST contract for listing a user's orders. Include GET /api/users/{id}/orders, cursor pagination, one uniform error object, 404 when the user is absent, 422 for malformed query parameters, and one sentence on HTTP method semantics."
5. Pass only on exit 0, one completed skill event, no other tools, and endpoint/cursor/404/422 in the result. Report commit, source, event, result, and removal list. Remove only $t and $s; never delete the whole root or unrelated files.
```

### User prompt

```text
Install and verify user-scoped api-design-principles from wshobson/agents commit 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35.
1. Create only ~/.codeartsdoer/vendor/wshobson-api-design-principles and ~/.codeartsdoer/skills/api-design-principles; do not alter root package.json, codearts_cli.json, credentials, plugins, or projects.
2. Run version/models checks and ask me for a model. Stop on a collision or missing credentials.
3. Run in PowerShell:
   $u=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'; $s=Join-Path $u 'vendor\wshobson-api-design-principles'; $t=Join-Path $u 'skills\api-design-principles'; $c=Join-Path ([IO.Path]::GetTempPath()) 'codearts-api-design-367cb6a-smoke'
   if((Test-Path $s)-or(Test-Path $t)-or(Test-Path $c)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent),$c -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/wshobson/agents.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/backend-development/skills/api-design-principles/' '/LICENSE'
   git -C $s checkout --detach 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
   if((git -C $s rev-parse HEAD).Trim() -ne '367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'plugins\backend-development\skills\api-design-principles') -Destination $t -Recurse; Set-Location $c
4. Confirm the user target with debug skill. Replace <model> and run: codearts run --format json --sandbox --model "<model>" "Verification contract: call the skill tool exactly once with name api-design-principles. No other tool is allowed. Using only the loaded skill and this prompt, write a concise REST contract for listing a user's orders. Include GET /api/users/{id}/orders, cursor pagination, one uniform error object, 404 when the user is absent, 422 for malformed query parameters, and one sentence on HTTP method semantics." Pass only on exit 0, one completed skill event, no other tools, and endpoint/cursor/404/422 in the result. Remove only $t, $s, and verified $c; never delete the user root or unrelated configuration.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models`; choose project or user scope, then execute the matching prompt's exact pin, sparse checkout, and full-directory copy.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # User: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$s=Join-Path $root 'vendor\wshobson-api-design-principles'; $t=Join-Path $root 'skills\api-design-principles'
if((Test-Path $s)-or(Test-Path $t)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
git clone --filter=blob:none --no-checkout https://github.com/wshobson/agents.git $s
git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/backend-development/skills/api-design-principles/' '/LICENSE'
git -C $s checkout --detach 367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35
Copy-Item -LiteralPath (Join-Path $s 'plugins\backend-development\skills\api-design-principles') -Destination $t -Recurse
```

## CodeArts configuration

Configure model credentials only through official mechanisms.

## Verification

Run `codearts debug skill` and the real verification command; all tool and REST assertions are mandatory.

## Use

```text
Use api-design-principles. Review this order API for resource naming, pagination, idempotency, and error structure: <contract>
```

## Update

Check with `git -C $s rev-parse HEAD`; re-audit and revalidate new commits.

## Remove

Remove only `skills/api-design-principles` and `vendor/wshobson-api-design-principles` from the selected scope, then verify absence. Never delete the whole root.

## Verified compatibility

| Item | Value |
| --- | --- |
| Result | **Works** |
| Commit | `367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35` |
| Skill / SHA-256 | `api-design-principles` / `7758937E5BD1F8F5AC7D0C89BE2C776D509855D9CF47DFAE75A5CB45E964AB75` |
| License | MIT |
| Environment/scopes | CodeArts CLI 26.8.1; Windows 11 Build 26200; MiMo; two projects + user; 2026-08-20 |

## Known limitations

Only the REST-contract call was tested, not GraphQL, execution of the template script, or other plugins.

## Security

The pinned directory has six files totaling 41,045 bytes. Its Python file is a template and was not executed. No npm, lifecycle, runtime network, credential, or telemetry step ran.

## Evidence and sources

- [中文研究](../../research/2026-08-20.md) · [English](../../research/2026-08-20.en.md)
- [Pinned commit](https://github.com/wshobson/agents/tree/367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35/plugins/backend-development/skills/api-design-principles)
- [MIT License](https://github.com/wshobson/agents/blob/367cb6a4a182cf7e9b0a17c9429f7411ddd9cf35/LICENSE)
- [Official CodeArts Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
