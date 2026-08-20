# Use khazix-writer with CodeArts CLI

[简体中文](README.md)

Install `khazix-writer` to turn source material into Chinese articles using its L1–L4 workflow.

## Choose a scope

Choose project `<project>/.codeartsdoer` for a team pin or user `~/.codeartsdoer` for reuse. Stop if either scope already contains the skill.

## Ask Agent to install it

### Project prompt

```text
Install and verify project-scoped khazix-writer from KKKKhazix/khazix-skills at commit 7a5c4934be4106ac740ffdb95280bb81b3f4b83c.
1. Create only .codeartsdoer/vendor/khazix-writer and .codeartsdoer/skills/khazix-writer. Do not change user files, package.json, codearts_cli.json, credentials, or other skills. Run codearts --version, git --version, and codearts models and ask me for an available model; stop on missing credentials or project/user collisions.
2. From the project root run in PowerShell:
   $s=Join-Path (Get-Location) '.codeartsdoer\vendor\khazix-writer'; $t=Join-Path (Get-Location) '.codeartsdoer\skills\khazix-writer'
   New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/KKKKhazix/khazix-skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/khazix-writer/' '/LICENSE'
   git -C $s checkout --detach 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
   if((git -C $s rev-parse HEAD).Trim() -ne '7a5c4934be4106ac740ffdb95280bb81b3f4b83c'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'khazix-writer') -Destination $t -Recurse
3. The sole debug-skill match must be $t/SKILL.md. Replace <model> and run verbatim: codearts run --format json --sandbox --model "<model>" "Verification contract: call the skill tool exactly once with name khazix-writer. The skill tool is the only allowed tool. Using only the prompt and loaded skill, write a short Chinese public-account excerpt about this fact: 今天我用十五分钟把一个 AI 写作 Skill 复制到 CodeArts 的原生 Skills 目录，并看到 completed skill event. Include one direct reader address using 你, one concrete scene, and a compact quality-check line explicitly naming L1, L2, L3, L4. Do not read or write files, browse, or execute commands."
4. Pass only on exit 0, one completed skill event, no other tools, and a Chinese result containing CodeArts, 你, a concrete scene, and L1–L4. Report evidence. Remove only $t and $s; never delete the whole root or unrelated files.
```

### User prompt

```text
Install and verify user-scoped khazix-writer at commit 7a5c4934be4106ac740ffdb95280bb81b3f4b83c.
1. Create only ~/.codeartsdoer/vendor/khazix-writer and ~/.codeartsdoer/skills/khazix-writer; do not alter root package.json, codearts_cli.json, credentials, plugins, or projects. Run version/model checks; stop on missing credentials or collisions.
2. In PowerShell run:
   $u=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'; $s=Join-Path $u 'vendor\khazix-writer'; $t=Join-Path $u 'skills\khazix-writer'; $c=Join-Path ([IO.Path]::GetTempPath()) 'codearts-khazix-writer-7a5c493-smoke'
   if((Test-Path $s)-or(Test-Path $t)-or(Test-Path $c)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent),$c -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/KKKKhazix/khazix-skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/khazix-writer/' '/LICENSE'
   git -C $s checkout --detach 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
   if((git -C $s rev-parse HEAD).Trim() -ne '7a5c4934be4106ac740ffdb95280bb81b3f4b83c'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'khazix-writer') -Destination $t -Recurse; Set-Location $c
3. Confirm the user source. Replace <model> and run: codearts run --format json --sandbox --model "<model>" "Verification contract: call the skill tool exactly once with name khazix-writer. The skill tool is the only allowed tool. Using only the prompt and loaded skill, write a short Chinese public-account excerpt about this fact: 今天我用十五分钟把一个 AI 写作 Skill 复制到 CodeArts 的原生 Skills 目录，并看到 completed skill event. Include one direct reader address using 你, one concrete scene, and a compact quality-check line explicitly naming L1, L2, L3, L4. Do not read or write files, browse, or execute commands." Pass only on exit 0, one completed skill, no other tools, and a Chinese result with CodeArts, 你, a concrete scene, and L1–L4. Remove only $t, $s, and verified $c. Never delete the user root or unrelated configuration.
```

## Manual Windows installation

Run the version/model checks, choose one scope, and execute the matching prompt's exact pinned checkout and full-directory copy.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # User: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$s=Join-Path $root 'vendor\khazix-writer'; $t=Join-Path $root 'skills\khazix-writer'
if((Test-Path $s)-or(Test-Path $t)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
git clone --filter=blob:none --no-checkout https://github.com/KKKKhazix/khazix-skills.git $s
git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/khazix-writer/' '/LICENSE'
git -C $s checkout --detach 7a5c4934be4106ac740ffdb95280bb81b3f4b83c
Copy-Item -LiteralPath (Join-Path $s 'khazix-writer') -Destination $t -Recurse
```

## CodeArts configuration

Configure credentials only through official mechanisms; never put them in repository files or output.

## Verification

Verify the resolved source and real completed tool event; prose alone is not evidence.

## Use

```text
Use khazix-writer. Turn this source into an 800-character Chinese article; first confirm reader and thesis, and invent no data: <source>
```

## Update

Check `git -C $s rev-parse HEAD`; re-audit and revalidate upgrades.

## Remove

Remove only `skills/khazix-writer` and `vendor/khazix-writer` in the selected scope and confirm absence.

## Verified compatibility

| Item | Value |
| --- | --- |
| Result | **Works** |
| Commit | `7a5c4934be4106ac740ffdb95280bb81b3f4b83c` |
| Skill / SHA-256 | `khazix-writer` / `8683276199A350DDB5195A9A2A00704A4EF10C0CC8DE9B9FAB43D328ADE41D7A` |
| License | MIT |
| Environment/scopes | CodeArts 26.8.1; Windows 11 Build 26200; MiMo; two projects + user; 2026-08-20 |

## Known limitations

Only a short Chinese rewrite and L1–L4 check were tested. Long-form research, publishing, and style authorization were not.

## Security

Three text files total 54,112 bytes and run no npm, script, runtime network, credential, or telemetry step. The skill contains author/sign-off copy; explicitly decide whether to retain it and review facts and copyright manually.

## Evidence and sources

- [中文研究](../../research/2026-08-20.md) · [English](../../research/2026-08-20.en.md)
- [Pinned commit](https://github.com/KKKKhazix/khazix-skills/tree/7a5c4934be4106ac740ffdb95280bb81b3f4b83c/khazix-writer)
- [MIT License](https://github.com/KKKKhazix/khazix-skills/blob/7a5c4934be4106ac740ffdb95280bb81b3f4b83c/LICENSE)
- [Official CodeArts Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
