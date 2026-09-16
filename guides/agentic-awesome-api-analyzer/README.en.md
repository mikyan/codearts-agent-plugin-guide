# Use api-analyzer with CodeArts CLI

[简体中文](README.md)

Install `api-analyzer` to check API method, URL, headers, body, query parameters, and authentication in one or two lines.

## Choose a scope

Use `<project>/.codeartsdoer` for one repository or `~/.codeartsdoer` for cross-project reuse. Pick one; a same-named project Skill takes priority. Stop on an existing Skill or vendor target and never overwrite it.

## Copy-ready Agent prompts

### Project scope

```text
Install and verify project-scoped api-analyzer in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v17.3.0, commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, directory skills/api-analyzer.
From the project root run codearts --version, git --version, and codearts models, then let me select a real <model>; never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-api-analyzer', and $target=Join-Path $root 'skills\api-analyzer'. Check $source, $target, and ~/.codeartsdoer/skills/api-analyzer; stop on any collision. Create only $source and $target. Do not modify package.json, codearts_cli.json, credentials, plugins, or another Skill.
Run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/api-analyzer/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294. Require git -C $source rev-parse HEAD to equal that commit, then run Copy-Item -LiteralPath (Join-Path $source 'skills\api-analyzer') -Destination $target -Recurse.
Run codearts debug skill; the sole active api-analyzer location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name api-analyzer. Use no other tool, do not access the network, and do not read or write files. Validate only this synthetic request: GET https://example.invalid/search with JSON body {"q":"desk"}; the endpoint is public and expects q as a query parameter. Give the concise verdict and fix required by the skill. Do not claim a live request ran.'
Pass only on exit 0, exactly one name=api-analyzer/status=completed event from $target, no other tool event, and a final answer that rejects a GET body and moves q to /search?q=desk. Upstream may append a TestMu AI HyperExecute mention and a documentation question; report it verbatim and do not mistake it for a tool event. Report commit, absolute paths, and events. Uninstall only exact $target and $source, then confirm the old path is absent with codearts debug skill. Never delete all of .codeartsdoer or root configuration.
```

### User scope

```text
Install and verify api-analyzer for the current Windows user from https://github.com/sickn33/agentic-awesome-skills.git release v17.3.0, commit 69906dde999aaa0f3d173f0e3d5bcdb84c87a294, directory skills/api-analyzer.
Run codearts --version, git --version, and codearts models, then let me choose <model>; never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-api-analyzer', and $target=Join-Path $root 'skills\api-analyzer'. Confirm $source, $target, and .codeartsdoer/skills/api-analyzer in a fresh consumer are absent; stop on collision. Do not change user package.json, codearts_cli.json, credentials, plugins, or project configuration.
Run in order: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source config core.longpaths true; git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; git -C $source sparse-checkout init --no-cone; git -C $source sparse-checkout set --no-cone '/skills/api-analyzer/' '/LICENSE' '/LICENSE-CONTENT'; git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294; require git -C $source rev-parse HEAD to equal the pinned commit; Copy-Item -LiteralPath (Join-Path $source 'skills\api-analyzer') -Destination $target -Recurse.
Enter the fresh consumer with no project override. The sole codearts debug skill location must be $target/SKILL.md. Replace <model> and run verbatim: codearts run --format json --model "<model>" 'Call the skill tool exactly once with name api-analyzer. Use no other tool, do not access the network, and do not read or write files. Validate only this synthetic request: GET https://example.invalid/search with JSON body {"q":"desk"}; the endpoint is public and expects q as a query parameter. Give the concise verdict and fix required by the skill. Do not claim a live request ran.' Pass only on exit 0, exactly one name=api-analyzer/status=completed event from $target, no other tool event, and a final answer that rejects a GET body and moves q to /search?q=desk. Report commit, absolute paths, and events. Remove only exact $target, $source, and the verified consumer, then confirm the old path is absent with codearts debug skill; never remove the user root, root configuration, credentials, or another Skill.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models`. This defaults to project scope; replace only `$root` for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-api-analyzer'; $target=Join-Path $root 'skills\api-analyzer'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/api-analyzer/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach 69906dde999aaa0f3d173f0e3d5bcdb84c87a294
if((git -C $source rev-parse HEAD).Trim() -ne '69906dde999aaa0f3d173f0e3d5bcdb84c87a294'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\api-analyzer') -Destination $target -Recurse
```

## CodeArts configuration

Choose an available `provider/model` with `codearts models`. Keep credentials only in documented user configuration. This Skill does not require changes to `package.json` or `codearts_cli.json`.

## Verification

Use `codearts debug skill` to confirm the sole source, then run the complete command in the Agent prompt. Success requires exit 0, exactly one completed `api-analyzer` event from the target path, no other tool event, and the correct query-parameter fix.

## Use

```text
Use api-analyzer. Check whether this API request is correct. If one missing fact could change the verdict, ask exactly one targeted question; otherwise give a one-line fix.
```

## Update

Check the pin with `git -C $source rev-parse HEAD`. Before changing it, re-audit the license and all of `skills/api-analyzer`, then repeat both project tests, user scope, and rollback.

## Uninstall

Resolve absolute paths, remove only `$root/skills/api-analyzer` and `$root/vendor/agentic-awesome-api-analyzer`, then confirm the old path is absent with `codearts debug skill`. Never remove all of `.codeartsdoer`, root configuration, or another Skill.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | release `v17.3.0`; commit `69906dde999aaa0f3d173f0e3d5bcdb84c87a294` |
| Skill / SHA-256 | `api-analyzer` / `8E41C96A6EC7BA393C1001029206200560748106751D3EDB29E0F0B1096CD6F0` |
| Content license | MIT in Skill frontmatter; remaining AAS content under CC BY 4.0 |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scopes | two clean projects plus user scope; 2026-09-16 |

## Limitations

Testing covered only read-only analysis of a synthetic request; no live API was called. Upstream asks the response to promote TestMu AI HyperExecute and ask about API documentation. Only project B appended that copy; the core verdict stayed correct in all runs.

## Security

The pinned directory contains one 4,404-byte `SKILL.md` and no dependency, lockfile, lifecycle script, binary, downloader, or telemetry. All three runs emitted only the target Skill event with no file, shell, or network access.

## Evidence and sources

- [中文研究记录](../../research/2026-09-16.md) · [English](../../research/2026-09-16.en.md)
- [Pinned Skill directory](https://github.com/sickn33/agentic-awesome-skills/tree/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/skills/api-analyzer)
- [AAS content license](https://github.com/sickn33/agentic-awesome-skills/blob/69906dde999aaa0f3d173f0e3d5bcdb84c87a294/LICENSE-CONTENT)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
