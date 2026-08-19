# Use i-have-adhd with CodeArts CLI

[简体中文](README.md)

Install the `i-have-adhd` skill to make Agent responses action-first, stepwise, and low-distraction.

## Choose an installation scope

Choose exactly one scope first:

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project-root>/.codeartsdoer` | A repository-pinned version, team sharing, or one-project use. Recommended by default. |
| User | `~/.codeartsdoer` | Reuse across projects for the current Windows user. |

The official CodeArts CLI documentation says a project skill should override a same-named user skill. On this machine, however, CodeArts CLI 26.8.1 resolved the user path when `i-have-adhd` existed in both scopes. To avoid this version-dependent conflict, this guide requires stopping when either scope already contains the skill.

## Ask Agent to install it

### Project-scope prompt

```text
Install and verify the project-scoped ayghri/i-have-adhd skill for CodeArts CLI from the current project root. Pin it to commit e7555fcaf612dfa1739dc86610ea926a906db614 (upstream package.json version 0.2.0).

Follow these requirements exactly:
1. Install only <project-root>/.codeartsdoer/vendor/i-have-adhd (pinned source) and <project-root>/.codeartsdoer/skills/i-have-adhd/SKILL.md (native CodeArts skill). Do not change ~/.codeartsdoer, any package.json, codearts_cli.json, credentials, or global software.
2. From the project root, run codearts --version, git --version, and codearts models. Ask me to select an actually available provider/model ID. If CodeArts reports missing credentials, stop and ask me to configure them from official documentation; do not read, print, or write secrets.
3. Check the project source directory, project skill directory, and ~/.codeartsdoer/skills/i-have-adhd. If any exists, stop and report the collision; do not overwrite it.
4. State that this installation has no npm dependency, install/postinstall step, binary, or runtime network access. Run only Git clone/checkout and copy only upstream skills/i-have-adhd/SKILL.md; do not copy hooks, extensions, plugin manifests, or the agents subdirectory.
5. Run the following verbatim in PowerShell from the project root:
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\i-have-adhd"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\i-have-adhd"
   $userSkill = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer\skills\i-have-adhd"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target) -or (Test-Path -LiteralPath $userSkill)) { throw "An i-have-adhd source or Skill already exists; inspect the conflict instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/ayghri/i-have-adhd.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/i-have-adhd"
   git -C $source checkout --detach "e7555fcaf612dfa1739dc86610ea926a906db614"
   if ((git -C $source rev-parse HEAD).Trim() -ne "e7555fcaf612dfa1739dc86610ea926a906db614") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\i-have-adhd\SKILL.md") -Destination (Join-Path $target "SKILL.md")
6. Run codearts debug skill. There must be exactly one i-have-adhd entry, and its location must be the current project's .codeartsdoer/skills/i-have-adhd/SKILL.md.
7. Replace <selected-model> with the ID selected in step 2, then run verbatim:
   codearts run --format json --sandbox --model "<selected-model>" "Verification contract: call the skill tool exactly once with name i-have-adhd. The skill tool is the only allowed tool. After its completed event, immediately produce the final answer; a bash, file, time, task, or any second tool call means failure. Answer in Chinese using only the prompt and loaded skill: 给出三步阅读一段 npm 错误文本的清单。第一行是打开错误文本；正好三步编号；最后一个动作是在两分钟内圈出第一个 Error 行。不要查询时间，不要修改文件，不要执行命令。"
8. Pass criteria: exit status 0; exactly one event with tool=skill, name=i-have-adhd, and status=completed; Base directory points to the project target; no other tool event occurs; the final result has exactly three numbered actions, starting by opening the error text and ending by circling the first Error line within two minutes.
9. Report the commit, target file, resolved CodeArts source, tool event, final result, and removal list. Removal may delete only <project-root>/.codeartsdoer/skills/i-have-adhd and <project-root>/.codeartsdoer/vendor/i-have-adhd. Never delete the whole .codeartsdoer directory, package.json, ProjectSkillStatus.txt, or another skill. Stop honestly on any failure and do not claim success.
```

### User-scope prompt

```text
Install and verify the user-scoped ayghri/i-have-adhd skill for the current Windows user. Pin it to commit e7555fcaf612dfa1739dc86610ea926a906db614 (upstream package.json version 0.2.0).

Follow these requirements exactly:
1. In PowerShell, define $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Install only $userRoot/vendor/i-have-adhd and $userRoot/skills/i-have-adhd/SKILL.md. Do not change $userRoot/package.json, codearts_cli.json, credentials, existing plugins, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. Ask me to select an actually available provider/model ID. If credentials are missing, stop and ask me to configure them from official documentation; do not read, print, or write secrets.
3. Check $userRoot/vendor/i-have-adhd and $userRoot/skills/i-have-adhd. If either exists, stop and report it; do not overwrite it. Also confirm the verification directory has no .codeartsdoer/skills/i-have-adhd.
4. State that this installation has no npm dependency, install/postinstall step, binary, or runtime network access. Run only Git clone/checkout and copy only upstream skills/i-have-adhd/SKILL.md; do not copy hooks, extensions, plugin manifests, or the agents subdirectory.
5. Run the following verbatim in one PowerShell session:
   $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
   $source = Join-Path $userRoot "vendor\i-have-adhd"
   $target = Join-Path $userRoot "skills\i-have-adhd"
   $consumer = Join-Path ([System.IO.Path]::GetTempPath()) "codearts-i-have-adhd-e7555fc-smoke"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target) -or (Test-Path -LiteralPath $consumer)) { throw "Source, target, or clean smoke directory already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path $target -Force | Out-Null
   New-Item -ItemType Directory -Path $consumer | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/ayghri/i-have-adhd.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/i-have-adhd"
   git -C $source checkout --detach "e7555fcaf612dfa1739dc86610ea926a906db614"
   if ((git -C $source rev-parse HEAD).Trim() -ne "e7555fcaf612dfa1739dc86610ea926a906db614") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\i-have-adhd\SKILL.md") -Destination (Join-Path $target "SKILL.md")
   Set-Location -LiteralPath $consumer
6. Run codearts debug skill from $consumer. There must be exactly one i-have-adhd entry, its location must be $userRoot/skills/i-have-adhd/SKILL.md, and $consumer must not contain a project-scoped copy.
7. Replace <selected-model> with the ID selected in step 2, then run verbatim:
   codearts run --format json --sandbox --model "<selected-model>" "Verification contract: call the skill tool exactly once with name i-have-adhd. The skill tool is the only allowed tool. After its completed event, immediately produce the final answer; a bash, file, time, task, or any second tool call means failure. Answer in Chinese using only the prompt and loaded skill: 给出三步阅读一段 npm 错误文本的清单。第一行是打开错误文本；正好三步编号；最后一个动作是在两分钟内圈出第一个 Error 行。不要查询时间，不要修改文件，不要执行命令。"
8. Pass criteria: exit status 0; exactly one successful i-have-adhd skill event; Base directory points to the user target; no other tool event occurs; the final result has exactly three numbered actions, starting by opening the error text and ending by circling the first Error line within two minutes.
9. Report the commit, target file, resolved CodeArts source, tool event, final result, and removal list. Removal may delete only $userRoot/skills/i-have-adhd, $userRoot/vendor/i-have-adhd, and optionally $consumer after its exact path is confirmed. Never delete all of $userRoot, $userRoot/package.json, codearts_cli.json, credentials, or another skill. Stop honestly on any failure and do not claim success.
```

## Manual Windows installation

### Prerequisites

Install CodeArts CLI and Git, then confirm an available model:

```powershell
codearts --version
git --version
codearts models
```

No npm command is required. Before installing, confirm neither project nor user scope already contains the same-named skill.

### Project scope

Run from the target project root:

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\i-have-adhd"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\i-have-adhd"
$userSkill = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer\skills\i-have-adhd"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target) -or (Test-Path -LiteralPath $userSkill)) { throw "An i-have-adhd source or Skill already exists." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path $target -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/ayghri/i-have-adhd.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/i-have-adhd"
git -C $source checkout --detach "e7555fcaf612dfa1739dc86610ea926a906db614"
if ((git -C $source rev-parse HEAD).Trim() -ne "e7555fcaf612dfa1739dc86610ea926a906db614") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\i-have-adhd\SKILL.md") -Destination (Join-Path $target "SKILL.md")
```

### User scope

Run in PowerShell:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\i-have-adhd"
$target = Join-Path $userRoot "skills\i-have-adhd"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "An i-have-adhd source or Skill already exists." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path $target -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/ayghri/i-have-adhd.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/i-have-adhd"
git -C $source checkout --detach "e7555fcaf612dfa1739dc86610ea926a906db614"
if ((git -C $source rev-parse HEAD).Trim() -ne "e7555fcaf612dfa1739dc86610ea926a906db614") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\i-have-adhd\SKILL.md") -Destination (Join-Path $target "SKILL.md")
```

## CodeArts configuration

Use `codearts models` to select an available `provider/model` ID and substitute it into the verification command. For Huawei-hosted models, follow the [official AK/SK guidance](https://support.huaweicloud.com/codeartsagent_faq/codeartsagent_faq_0054.html); for a custom provider, supply credentials through the existing local configuration. Never place an API key, AK, or SK in the repository or command output.

In this MiMo custom-provider environment, CodeArts CLI 26.8.1 still required `CODEARTS_CLI_AK` and `CODEARTS_CLI_SK` to exist during preflight. Non-secret placeholders passed only that local preflight; the configured provider still supplied the real model credential. This is an observed environment detail, not general configuration advice.

## Verify

First run `codearts debug skill` and confirm that the sole matching `location` belongs to the selected scope. User-scope verification must run from a directory without a project copy.

Then replace the model ID and run:

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Verification contract: call the skill tool exactly once with name i-have-adhd. The skill tool is the only allowed tool. After its completed event, immediately produce the final answer; a bash, file, time, task, or any second tool call means failure. Answer in Chinese using only the prompt and loaded skill: 给出三步阅读一段 npm 错误文本的清单。第一行是打开错误文本；正好三步编号；最后一个动作是在两分钟内圈出第一个 Error 行。不要查询时间，不要修改文件，不要执行命令。"
```

Success requires exactly one `skill` event with input `i-have-adhd`, status `completed`, and a Base directory under the installation target. The final text must contain exactly three actions, with no other tool event.

## Use

Explicit invocation is the reliable path:

```text
Use i-have-adhd for this task. Rewrite the following troubleshooting plan so the first step is immediately actionable, the list has at most five single-action items, and it ends with one next action: <paste plan>
```

This run verified one explicit invocation only. Do not treat upstream session persistence or the always-on hook as verified.

## Update

Check the current pin first:

```powershell
git -C <selected-scope>\.codeartsdoer\vendor\i-have-adhd rev-parse HEAD
```

Before moving to a new commit, re-review the license, `SKILL.md`, manifests, hooks, scripts, and security changes. Remove the old skill and dedicated vendor exactly, replace this guide's commit, and repeat installation, clean reproduction, and rollback. Do not carry forward the `Works` result until the new commit passes real CodeArts verification.

## Remove

For project scope, run from the project root:

```powershell
$targets = @(
  (Join-Path (Get-Location) ".codeartsdoer\skills\i-have-adhd"),
  (Join-Path (Get-Location) ".codeartsdoer\vendor\i-have-adhd")
)
foreach ($target in $targets) {
  if (Test-Path -LiteralPath $target) {
    $resolved = (Resolve-Path -LiteralPath $target).Path
    if ($resolved -ne [System.IO.Path]::GetFullPath($target)) { throw "Unexpected path: $resolved" }
    Remove-Item -LiteralPath $resolved -Recurse -Force
  }
}
```

For user scope:

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$targets = @(
  (Join-Path $userRoot "skills\i-have-adhd"),
  (Join-Path $userRoot "vendor\i-have-adhd")
)
foreach ($target in $targets) {
  if (Test-Path -LiteralPath $target) {
    $resolved = (Resolve-Path -LiteralPath $target).Path
    if ($resolved -ne [System.IO.Path]::GetFullPath($target)) { throw "Unexpected path: $resolved" }
    Remove-Item -LiteralPath $resolved -Recurse -Force
  }
}
```

Do not delete the whole `.codeartsdoer` directory, root `package.json`, `codearts_cli.json`, credentials, `ProjectSkillStatus.txt`, or another skill. After removal, `codearts debug skill` must have no `i-have-adhd` entry. The real rollback also confirmed that the user-root `package.json` SHA-256 returned to its pre-install value.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Works** |
| Upstream version | `0.2.0`; no GitHub Release |
| Pinned commit | `e7555fcaf612dfa1739dc86610ea926a906db614` |
| Verified skill | `i-have-adhd` |
| Skill SHA-256 | `13969C0AA5A69127828EF10FB4A53AA2B56D08C184B9F4354D0EA34695C358BB` |
| License | MIT |
| CodeArts | CLI 26.8.1 on Windows 11 Build 26200 |
| Model | `mimo/mimo-v2.5` |
| Verified scopes | Project, second clean project, and user |
| Last verified | 2026-08-19 |

`Works` applies only to placing the pinned content-only `SKILL.md` in CodeArts' native skills directory. No upstream plugin, hook, or extension for another client was verified.

## Known limitations

- CodeArts CLI 26.8.1 resolved a same-named user skill ahead of the project skill, contrary to the current official CLI documentation. Install one scope only and trust the diagnostic path.
- Only an explicit, single-turn `skill` call was verified. Upstream persistence, `stop adhd mode`, always-on hooks, and OpenCode/Claude/Codex plugins were not tested.
- Upstream frontmatter contains `disable-model-invocation: true`, but CodeArts' runtime `skill` tool omits frontmatter from returned content. This guide makes no automatic-invocation claim.
- The first user-scope smoke attempt violated the no-other-tool contract by reading the system time. A stricter, self-contained retry used only the skill and passed. Tool constraints remain dependent on model compliance.
- CodeArts IDE, Linux, other models, auxiliary `agents` metadata, and accessibility or medical outcomes were not tested.

## Security

The pinned commit is MIT-licensed. The installation target is one 6,953-byte `SKILL.md`; it runs no npm command, lifecycle script, hook, extension, or binary, and needs no runtime network access, credentials, or telemetry. The upstream repository does contain hooks, PowerShell/Shell/Node scripts, and TypeScript extensions for other clients. They were statically reviewed but not installed or executed. The skill shapes output and describes a persistence mode, so read its body before installation; system and user safety requirements always take priority.

## Evidence and sources

- [中文 verification record](../../research/2026-08-19.md) · [English](../../research/2026-08-19.en.md)
- [Pinned commit](https://github.com/ayghri/i-have-adhd/tree/e7555fcaf612dfa1739dc86610ea926a906db614)
- [Copied SKILL.md](https://github.com/ayghri/i-have-adhd/blob/e7555fcaf612dfa1739dc86610ea926a906db614/skills/i-have-adhd/SKILL.md)
- [MIT License](https://github.com/ayghri/i-have-adhd/blob/e7555fcaf612dfa1739dc86610ea926a906db614/LICENSE)
- [Official CodeArts CLI Skills documentation](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
- [Official CodeArts CLI commands](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0034.html)
