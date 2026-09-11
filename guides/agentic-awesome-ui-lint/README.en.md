# Use ui-lint with CodeArts CLI

[简体中文](README.md)

Install the `ui-lint` Skill to review React/JSX for design-token, logical-property, sizing, and component-contract violations.

## Choose a scope

Use `<project>/.codeartsdoer` for a team-pinned project install or `~/.codeartsdoer` for reuse by the current Windows user. Pick one. Run `codearts debug skill` first: a same-named project Skill takes precedence over user scope, and any existing target or vendor path is a stop condition—never overwrite it.

## Copy-ready Agent prompts

### Project scope

```text
Install and verify the project-scoped ui-lint Skill in the current project. Source: https://github.com/sickn33/agentic-awesome-skills.git at commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3, source directory skills/ui-lint.
1. Create only <project>/.codeartsdoer/vendor/agentic-awesome-ui-lint and <project>/.codeartsdoer/skills/ui-lint. Do not modify ~/.codeartsdoer, package.json, codearts_cli.json, credentials, or another Skill.
2. Run codearts --version, git --version, and codearts models, then let me choose an available <model>. Stop if credentials are unavailable; never read or print secrets.
3. Check both project paths and ~/.codeartsdoer/skills/ui-lint. Stop on any collision; do not overwrite.
4. Run the commands under “Manual Windows installation” from the project root with $root set to the project's .codeartsdoer. Copy exactly vendor/agentic-awesome-ui-lint/skills/ui-lint to .codeartsdoer/skills/ui-lint.
5. Run codearts debug skill. The sole active ui-lint location must be the project target's SKILL.md.
6. Substitute the chosen ID for <model> and run the codearts run command under “Verification” verbatim.
7. Pass only on exit 0, exactly one name=ui-lint status=completed Skill event, no other tool events, and a final answer containing all six issues, InlineCard.tsx:1, and a fix for each.
8. Report the commit, copy paths, events, and uninstall list. Uninstall may remove only the exact target and source above. Never delete all of .codeartsdoer, package.json, configuration, credentials, or another Skill. Stop truthfully on failure.
```

### User scope

```text
Install and verify ui-lint for the current Windows user. Source: https://github.com/sickn33/agentic-awesome-skills.git at commit bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3, source directory skills/ui-lint.
1. Create only ~/.codeartsdoer/vendor/agentic-awesome-ui-lint and ~/.codeartsdoer/skills/ui-lint. Do not modify the user package.json, codearts_cli.json, credentials, plugins, or project configuration.
2. Run codearts --version, git --version, and codearts models, then let me select an available <model>. Confirm that both user paths are absent and that a fresh temporary consumer has no project .codeartsdoer/skills/ui-lint. Stop on any collision.
3. Run “Manual Windows installation” with $root exactly Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', then enter the fresh consumer with no same-named project Skill.
4. Run codearts debug skill; the sole active ui-lint location must be the user target's SKILL.md. Substitute <model> and run the “Verification” command verbatim.
5. Pass only on exit 0, exactly one completed ui-lint Skill event, no other tool events, and all content assertions. Uninstall may remove only the exact target, source, and verified temporary consumer; never delete the user root, root configuration, credentials, or another Skill.
```

## Manual Windows installation

First run `codearts --version`, `git --version`, and `codearts models`. The example defaults to project scope; replace only the `$root` line for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # user scope: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-ui-lint'; $target=Join-Path $root 'skills\ui-lint'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source config core.longpaths true
git -C $source fetch --depth 1 origin bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
git -C $source sparse-checkout init --no-cone
git -C $source sparse-checkout set --no-cone '/skills/ui-lint/' '/LICENSE' '/LICENSE-CONTENT'
git -C $source checkout --detach bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3
if((git -C $source rev-parse HEAD).Trim() -ne 'bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\ui-lint') -Destination $target -Recurse
```

## CodeArts configuration

Choose a working model with `codearts models` and manage credentials through the documented user configuration. Never place credentials in the repository, prompt, or logs. This guide requires no `package.json` or `codearts_cli.json` change.

## Verification

Confirm the sole source with `codearts debug skill`, replace `<model>`, and run verbatim:

```powershell
codearts run --format json --model "<model>" 'Call the skill tool exactly once with name ui-lint. Use no other tool, do not execute shell commands, do not access the network, and do not read or write files. Lint only this inline JSX: function Card(){return <button className={`ml-2 p-[24px] text-[#000000] w-4 h-4`}>Save</button>}. Return exact sections SCORE, ISSUES, FIXES, and TOTAL. Identify the hardcoded color, raw pixel spacing, physical margin, separate width/height, template-literal className, and missing data-slot. Use file reference InlineCard.tsx:1 and provide a concrete fix for every issue.'
```

Success means exit 0, exactly one completed `ui-lint` event from the target absolute path, no Bash/Read/Write/Edit/Web event, and a final answer with all four sections, six issue classes, the line reference, and corresponding fixes.

## Use

```text
Use ui-lint. Review this JSX, rank design-token, RTL, sizing, and data-slot issues, and give file:line plus the smallest fix.
```

## Update and uninstall

Record the pin with `git -C $source rev-parse HEAD`. Before changing commits, re-audit the license and every file, script, and dependency under `skills/ui-lint`, then repeat project A, clean project B, user-scope, and rollback tests.

For uninstall, resolve paths and remove only `$root/skills/ui-lint` and `$root/vendor/agentic-awesome-ui-lint`; then confirm the former location is absent from `codearts debug skill`. Never delete all of `.codeartsdoer`, root configuration, or another Skill.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | release `v17.0.0`; commit `bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3` |
| Skill / SHA-256 | `ui-lint` / `F08A89DB9C2925436FCB5EC155610A78F558CEA1FAD946A234DFF02B00A9A89E` |
| License | MIT |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scopes | two clean projects plus user scope; 2026-09-11 |

## Limitations and security

This result covers only the pinned `ui-lint` directory, not the collection. Rule-based review does not replace browser, visual-regression, accessibility-tree, or interaction testing. Resolve collisions with `codearts debug skill`.

The directory contains one 3,553-byte `SKILL.md` and no dependency, lifecycle script, binary, downloader, or telemetry. Although its text demonstrates `grep`, validation used complete inline JSX and denied shell, file, and network access.

## Evidence and sources

- [English research](../../research/2026-09-11.en.md) · [中文](../../research/2026-09-11.md)
- [Pinned Skill directory](https://github.com/sickn33/agentic-awesome-skills/tree/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/skills/ui-lint)
- [MIT License](https://github.com/sickn33/agentic-awesome-skills/blob/bdfbf79ccaabdc31f60ce60ef1703a9abe95f9c3/LICENSE)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
