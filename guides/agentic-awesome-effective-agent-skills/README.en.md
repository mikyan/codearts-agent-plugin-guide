# Use effective-agent-skills with CodeArts CLI

[简体中文](README.md)

Install `effective-agent-skills` to review Agent Skill routing, structure, determinism, validation, and safety.

## Choose a scope

Use `<project>/.codeartsdoer` for a repository-wide review standard or `~/.codeartsdoer` for cross-project reuse. Pick one and run `codearts debug skill`; a project Skill wins on a same-name collision. Stop if any target or vendor path exists and never overwrite it.

## Copy-ready Agent prompts

### Project scope

```text
Install and verify project-scoped effective-agent-skills in the current project. Source https://github.com/sickn33/agentic-awesome-skills.git, pinned commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3, source directory skills/effective-agent-skills.
1. Create only <project>/.codeartsdoer/vendor/agentic-awesome-effective-agent-skills and <project>/.codeartsdoer/skills/effective-agent-skills. Do not modify ~/.codeartsdoer, package.json, codearts_cli.json, credentials, or another Skill.
2. Run codearts --version, git --version, and codearts models, then let me select an available <model>. Stop if credentials are missing; never read or print secrets.
3. Check both project paths and ~/.codeartsdoer/skills/effective-agent-skills. Stop on any collision; do not overwrite.
4. From the project root run “Manual Windows installation” with $root set to the project's .codeartsdoer. Copy exactly vendor/agentic-awesome-effective-agent-skills/skills/effective-agent-skills to .codeartsdoer/skills/effective-agent-skills.
5. Run codearts debug skill; the sole active effective-agent-skills location must be the project target SKILL.md.
6. Replace <model> and run the “Verification” codearts run command verbatim.
7. Pass only on exit 0, exactly one name=effective-agent-skills status=completed Skill event, no other tool event, and six requested sections identifying name mismatch, missing what/when, and absent workflow/determinism/validation/safety, plus a lowercase-hyphen name and complete description.
8. Report commit, paths, events, and uninstall list. Remove only the exact target and source. Never delete all of .codeartsdoer, package.json, configuration, credentials, or another Skill. Stop truthfully on failure.
```

### User scope

```text
Install and verify effective-agent-skills for the current Windows user from https://github.com/sickn33/agentic-awesome-skills.git at commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3, source directory skills/effective-agent-skills.
1. Create only ~/.codeartsdoer/vendor/agentic-awesome-effective-agent-skills and ~/.codeartsdoer/skills/effective-agent-skills. Do not modify user package.json, codearts_cli.json, credentials, plugins, or project configuration.
2. Run codearts --version, git --version, and codearts models, then let me choose <model>. Confirm both user paths and .codeartsdoer/skills/effective-agent-skills in a fresh temporary consumer are absent. Stop on collision.
3. Run “Manual Windows installation” with $root exactly Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', then enter the consumer with no same-named project Skill.
4. The sole codearts debug skill location must be the user target. Replace <model> and run “Verification” verbatim with the same success assertions.
5. Uninstall only the exact target, source, and verified temporary consumer; never delete the user root, root configuration, credentials, or another Skill.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models` first. This defaults to project scope; replace only `$root` for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user scope: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-effective-agent-skills'; $target=Join-Path $root 'skills\effective-agent-skills'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/effective-agent-skills/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
if((git -C $source rev-parse HEAD).Trim() -ne 'bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\effective-agent-skills') -Destination $target -Recurse
```

## Configuration and verification

Select a working model using `codearts models`; keep credentials only in documented user configuration, never in the repository, prompt, or logs. Confirm the sole source using `codearts debug skill`, replace `<model>`, and run verbatim:

```powershell
codearts run --format json --model "<model>" 'Call the skill tool exactly once with name effective-agent-skills. Use no other tool, do not access the network, and do not read or write files. Review this inline draft skill: folder name report-helper; frontmatter name ReportHelper; description "Helps with reports"; body says "Do the workflow"; no trigger, steps, output format, validation loop, failure handling, or safety notes. Return exact sections ROUTING, STRUCTURE, DETERMINISM, VALIDATION, SECURITY, and VERDICT. Identify the invalid name mismatch and vague description, then propose a lowercase-hyphen name and a description containing what and when. Do not write a replacement file.'
```

Success means exit 0, exactly one completed `effective-agent-skills` event from the target absolute path, no other tool event, and a final answer with all six sections, every defect, the requested corrections, and no replacement-file write.

## Use

```text
Use effective-agent-skills. Read-only review this Skill's routing description, workflow, output contract, validation loop, failure handling, and safety boundary; cite evidence before minimal changes.
```

## Update and uninstall

Check the pin with `git -C $source rev-parse HEAD`. Before updating, re-audit the license and every file and dependency under `skills/effective-agent-skills`, then repeat project A, clean project B, user-scope, and rollback tests.

After resolving absolute paths, uninstall only `$root/skills/effective-agent-skills` and `$root/vendor/agentic-awesome-effective-agent-skills`; confirm the old path disappears from `codearts debug skill`. Never delete all of `.codeartsdoer`, root configuration, or another Skill.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | release `v17.0.0`; commit `bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3` |
| Skill / SHA-256 | `effective-agent-skills` / `801183F8629EDFCEA977F3C0C8D4ECAEEF2B59626DB6527E785DC84938EA8C73` |
| License | MIT |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scopes | two clean projects plus user scope; 2026-09-11 |

## Limitations and security

This test covers read-only review of one inline draft, not automatic repair or any other collection Skill. The 16 KB rubric may produce long answers, and a real review must still account for the target agent's frontmatter rules, tool interface, and repository conventions.

The pinned directory is one 16,172-byte `SKILL.md` with no dependency, lifecycle script, binary, downloader, or telemetry. All three runs emitted only the target Skill event and made no file, shell, or network access. Prefer read-only review and human approval before edits.

## Evidence and sources

- [English research](../../research/2026-09-11.en.md) · [中文](../../research/2026-09-11.md)
- [Pinned Skill directory](https://github.com/sickn33/agentic-awesome-skills/tree/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/skills/effective-agent-skills)
- [MIT License](https://github.com/sickn33/agentic-awesome-skills/blob/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/LICENSE)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
