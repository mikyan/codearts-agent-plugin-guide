# Use read-all-adrs with CodeArts CLI

[简体中文](README.md)

Install `read-all-adrs` to read every architecture decision record before design or implementation work begins.

## Choose a scope

Use `<project>/.codeartsdoer` to pin the Skill with a repository or `~/.codeartsdoer` for user-wide reuse. Pick one and inspect `codearts debug skill`; project scope wins on a same-name collision. Stop if any target or vendor path exists—do not overwrite it.

## Copy-ready Agent prompts

### Project scope

```text
Install and verify project-scoped read-all-adrs in the current project. Source https://github.com/sickn33/agentic-awesome-skills.git, pinned commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3, source directory skills/read-all-adrs.
1. Create only <project>/.codeartsdoer/vendor/agentic-awesome-read-all-adrs and <project>/.codeartsdoer/skills/read-all-adrs. Do not modify ~/.codeartsdoer, package.json, codearts_cli.json, ADRs, credentials, or another Skill.
2. Run codearts --version, git --version, and codearts models, then let me choose an available <model>. Stop without credentials; never read or print secrets.
3. Check both project paths and ~/.codeartsdoer/skills/read-all-adrs. Stop on collision. Confirm docs/adr contains at least one Markdown file; otherwise stop and do not claim validation.
4. From the project root, run “Manual Windows installation” with $root set to the project .codeartsdoer. Copy exactly vendor/agentic-awesome-read-all-adrs/skills/read-all-adrs to .codeartsdoer/skills/read-all-adrs.
5. Run codearts debug skill; the sole active read-all-adrs location must be the target SKILL.md.
6. Replace <model> and run the “Verification” codearts run command verbatim.
7. Pass only on exit 0, one completed read-all-adrs Skill event, a completed read for every docs/adr/*.md, no write/shell/network event, and a faithful final answer listing every file, decision, consequence, and open question.
8. Report commit, paths, read events, and uninstall list. Remove only the exact target and source; never delete all of .codeartsdoer, docs/adr, root configuration, credentials, or another Skill. Stop truthfully on failure.
```

### User scope

```text
Install and verify read-all-adrs for the current Windows user from https://github.com/sickn33/agentic-awesome-skills.git at commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3, source directory skills/read-all-adrs.
1. Create only ~/.codeartsdoer/vendor/agentic-awesome-read-all-adrs and ~/.codeartsdoer/skills/read-all-adrs. Do not modify user package.json, codearts_cli.json, credentials, plugins, project configuration, or ADRs.
2. Run codearts --version, git --version, and codearts models and let me choose <model>. Check both user paths and choose a consumer with Markdown ADRs under docs/adr but no project-scoped read-all-adrs. Stop on collision or no ADR.
3. Run “Manual Windows installation” with $root exactly Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', then enter that consumer.
4. The sole codearts debug skill location must be the user target. Replace <model> and run “Verification” verbatim; use the same success conditions as project scope.
5. Uninstall may remove only the exact target and source; never delete the consumer, ADRs, user root, configuration, credentials, or another Skill.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models` first. This defaults to project scope; replace only `$root` for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user scope: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-read-all-adrs'; $target=Join-Path $root 'skills\read-all-adrs'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/read-all-adrs/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
if((git -C $source rev-parse HEAD).Trim() -ne 'bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\read-all-adrs') -Destination $target -Recurse
```

## Configuration and verification

Choose a working model using `codearts models`; keep credentials only in documented user configuration, never in the repository, prompt, or logs. Confirm `docs/adr/*.md` is nonempty and inspect the sole Skill source with `codearts debug skill`. Replace `<model>` and run verbatim:

```powershell
codearts run --format json --model "<model>" 'Call the skill tool exactly once with name read-all-adrs. Then use only read tools to read every Markdown file under docs/adr completely. Do not access the network or write files. Return sections ADRS READ, CURRENT DECISIONS, CONSEQUENCES, OPEN QUESTIONS; list every filename.'
```

Success requires exit 0, the target `read-all-adrs` event completed, completed Reads for every ADR, no Write/Edit/Bash/Web event, and a grounded final answer listing all files. An empty ADR directory is unverified, not passed.

## Use

```text
Use read-all-adrs. Before proposing a cache design, read every ADR under docs/adr completely and list current constraints, conflicts, and unresolved decisions.
```

## Update and uninstall

Check the pin with `git -C $source rev-parse HEAD`. Before upgrading, re-audit the license and all `skills/read-all-adrs` content, then repeat two-project, user-scope, full-read, and rollback tests.

Uninstall only `$root/skills/read-all-adrs` and `$root/vendor/agentic-awesome-read-all-adrs` after resolving their absolute paths. Do not remove ADRs or the consumer. Confirm the old path disappears from `codearts debug skill`.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | release `v17.0.0`; commit `bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3` |
| Skill / SHA-256 | `read-all-adrs` / `8413F2FA1D9504FBBD179E356ED1FA2210CF29F98962BC5BA4CEEE4C3CA0AABB` |
| License | MIT |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scopes | two clean projects plus user scope; 2026-09-11 |

## Limitations and security

The test used two synthetic ADRs and preserved SQLite, optional sync, ciphertext-only server storage, and unresolved key recovery. CodeArts semantics for the frontmatter field `disable-model-invocation: true` were not separately proven; only explicit invocation was tested. The Skill cannot determine whether an ADR is current or correct.

The directory is one 1,331-byte `SKILL.md` with no dependency, script, binary, downloader, or telemetry. Validation authorized reads only under `docs/adr`; the user run repeated reads but emitted no write, shell, or network event. Use a model compatible with the project's data boundary because ADRs may be sensitive.

## Evidence and sources

- [English research](../../research/2026-09-11.en.md) · [中文](../../research/2026-09-11.md)
- [Pinned Skill directory](https://github.com/sickn33/agentic-awesome-skills/tree/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/skills/read-all-adrs)
- [MIT License](https://github.com/sickn33/agentic-awesome-skills/blob/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/LICENSE)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
