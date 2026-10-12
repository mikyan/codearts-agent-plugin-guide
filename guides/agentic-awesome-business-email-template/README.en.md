# Use business-email-template in CodeArts CLI

[中文](README.md)

Install the `business-email-template` Skill so the Agent manages reusable business email templates, triggers, personalization tokens, authentication, and send checks in an auditable field register instead of handling customer lists or sending mail.

## Choose an installation scope

Project scope under `<project>/.codeartsdoer` affects only that repository. User scope under `~/.codeartsdoer` affects every project. Choose one. CodeArts documents project Skills as taking precedence over same-named user Skills; stop instead of overwriting if the Skill, vendor, or same name in the other scope already exists.

## Ask the Agent to install it

### Project-scope prompt

```text
Install and verify the project-scoped business-email-template Skill in the current Windows project. Pin https://github.com/sickn33/agentic-awesome-skills.git release v19.4.0 and commit 9f2c979c0841ad3f24cc42e38f9f7cb377034558. Copy the complete skills/business-email-template directory, including SKILL.md and references/common-pitfalls.md, to <project>/.codeartsdoer/skills/business-email-template and keep the sparse clone at <project>/.codeartsdoer/vendor/agentic-awesome-business-email-template. From the project root, first run codearts --version, git --version, and codearts models, then ask me to choose a real available <model>. Never read or print credentials. Set $root=Join-Path (Get-Location) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-business-email-template', and $target=Join-Path $root 'skills\business-email-template'. Stop if $source, $target, or ~/.codeartsdoer/skills/business-email-template exists. Do not modify package.json, codearts_cli.json, credentials, plugins, permission files, or other Skills.
From the project root run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/business-email-template; require git -C $source rev-parse HEAD to equal 9f2c979c0841ad3f24cc42e38f9f7cb377034558 exactly; Copy-Item -LiteralPath (Join-Path $source 'skills\business-email-template') -Destination $target -Recurse; require both $target/SKILL.md and $target/references/common-pitfalls.md to exist.
Run codearts debug skill and require the sole name=business-email-template location to be $target/SKILL.md. Replace <model> with the chosen provider/model ID and run exactly: codearts run -m "<model>" --format json 'Call the skill tool exactly once with name business-email-template and use no other tool. Build a data-only empty CSV template register for transactional enquiry replies. Return exactly one CSV header line using the skill''s complete field list, followed by one short sentence that no customer data, DNS values, or email bodies were used. Do not create a file, send email, or invent a domain. End with the literal markers EMAIL_SCHEMA_22_COLUMNS and DATA_ONLY.' Pass only when the exit code is 0, exactly one name=business-email-template/status=completed event comes from $target, no other tool event occurs, and the sole CSV header has these 22 columns in order: Template ID, Template Name, Template Type, Trigger, Sender Role, Recipient Type, Subject Line Pattern, Preview Text, Body Structure, Tone, Call To Action, Required Fields, Personalisation Tokens, Signature Block, Unsubscribe Required, Spam Risk Notes, Test Status, Mobile Checked, Accessibility Checked, Version, Status, Notes. The answer must also state that no customer data, DNS values, or email bodies were used and contain both markers. Report the commit, absolute path, and event. Uninstall only exact $target and $source, then run codearts debug skill and require the old path to be absent. Never delete the whole .codeartsdoer directory, project root, user root, or root configuration.
```

### User-scope prompt

```text
Install and verify business-email-template for the current Windows user. Pin https://github.com/sickn33/agentic-awesome-skills.git release v19.4.0 and commit 9f2c979c0841ad3f24cc42e38f9f7cb377034558. Copy the complete skills/business-email-template directory to ~/.codeartsdoer/skills/business-email-template and keep the sparse clone at ~/.codeartsdoer/vendor/agentic-awesome-business-email-template. First run codearts --version, git --version, and codearts models, then ask me to choose a real available <model>. Never read or print credentials. Set $root=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer', $source=Join-Path $root 'vendor\agentic-awesome-business-email-template', and $target=Join-Path $root 'skills\business-email-template'. Stop if $source, $target, or .codeartsdoer/skills/business-email-template in the fresh verification consumer exists. Do not modify user package.json, codearts_cli.json, credentials, plugins, permission files, or project configuration.
Run: New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null; git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source; git -C $source sparse-checkout set skills/business-email-template; require HEAD to equal 9f2c979c0841ad3f24cc42e38f9f7cb377034558 exactly; Copy-Item -LiteralPath (Join-Path $source 'skills\business-email-template') -Destination $target -Recurse; require both pinned Markdown files. Enter a fresh consumer without a project Skill of the same name. Run codearts debug skill and require the sole name=business-email-template location to be $target/SKILL.md. Replace <model> and run exactly: codearts run -m "<model>" --format json 'Call the skill tool exactly once with name business-email-template and use no other tool. Build a data-only empty CSV template register for transactional enquiry replies. Return exactly one CSV header line using the skill''s complete field list, followed by one short sentence that no customer data, DNS values, or email bodies were used. Do not create a file, send email, or invent a domain. End with the literal markers EMAIL_SCHEMA_22_COLUMNS and DATA_ONLY.' Apply the same pass criteria as project scope. Report the commit, absolute path, and event. Uninstall only exact $target, $source, and the verified consumer, then require the old path to disappear from codearts debug skill. Do not delete the user root, root configuration, credentials, or other Skills.
```

## Manual Windows installation

Run `codearts --version`, `git --version`, and `codearts models` first. This example uses project scope; replace only `$root` for user scope.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # User scope: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$source=Join-Path $root 'vendor\agentic-awesome-business-email-template'
$target=Join-Path $root 'skills\business-email-template'
if((Test-Path $source)-or(Test-Path $target)){throw 'Collision'}
New-Item -ItemType Directory -Path (Split-Path $source -Parent),(Split-Path $target -Parent) -Force | Out-Null
git clone --depth 1 --filter=blob:none --sparse --branch v19.4.0 https://github.com/sickn33/agentic-awesome-skills.git $source
git -C $source sparse-checkout set skills/business-email-template
if((git -C $source rev-parse HEAD).Trim() -ne '9f2c979c0841ad3f24cc42e38f9f7cb377034558'){throw 'Commit mismatch'}
Copy-Item -LiteralPath (Join-Path $source 'skills\business-email-template') -Destination $target -Recurse
if(-not (Test-Path (Join-Path $target 'references\common-pitfalls.md'))){throw 'Incomplete copy'}
```

## CodeArts configuration

Use `codearts models` to select a real available `provider/model`. This content-only Skill has no runtime dependency and needs no change to `package.json`, `codearts_cli.json`, or permission files. It does not connect to mailboxes, Notion, DNS, or a sending provider.

## Verify

Use `codearts debug skill` to confirm the sole absolute path, then run the full `codearts run` command above. Success requires exactly one target Skill event, no other tool event, the exact 22-column header, and an explicit data-boundary statement.

## Use

```text
Use business-email-template. We need reusable enquiry reply, quote, and invoice messages. First specify the smallest template set, triggers, sender role, recipient type, subject pattern, body structure, personalization tokens, signature, unsubscribe rule, and pre-send checks. Do not ask for customer lists, real email bodies, DNS values, or credentials. Generate data templates only after I explicitly ask, and never send mail.
```

## Update

Before changing the pinned commit, review `SKILL.md`, the reference, and licenses again, then repeat two fresh projects, a clean user consumer, the 22-column output, and rollback.

## Uninstall

Delete only `$root/skills/business-email-template` and `$root/vendor/agentic-awesome-business-email-template`, then use `codearts debug skill` to confirm the old absolute path is gone.

## Verified versions and result

| Item | Value |
| --- | --- |
| Result | **Works** |
| Upstream | Agentic Awesome `v19.4.0`; commit `9f2c979c0841ad3f24cc42e38f9f7cb377034558` |
| Files / SHA-256 | `SKILL.md` / `5EE81C9BFC7FA76957274C4379B64AC0DF16D306B03B20CFE9C494C108ABBC24`; `references/common-pitfalls.md` / `F30CB313C7EEEDD2002D4E1F39EAA65E3F78CA22F3E07312442DD1BB3F8AA333` |
| License | Original AAS non-code content: CC BY 4.0; repository code and tooling: MIT; Skill frontmatter says `source: self` |
| Environment | CodeArts CLI 26.8.1; Windows 11 build 26200; `mimo/mimo-v2.5` |
| Scope | Two fresh projects plus a user consumer without a project override; three main and three publication calls; 2026-10-12 |

## Known limitations

The Skill produces a template register and recommendations. It does not configure SPF/DKIM/DMARC, measure spam scores, render mail, validate legal wording, or send messages. Marketing consent, unsubscribe, address, and regulatory language vary by jurisdiction and need business/legal review. Do not place customer lists or personal data in the template register.

## Security

The pinned directory contains only two Markdown files: no manifest, lockfile, script, binary, downloader, runtime network access, credential, or telemetry. All six final calls emitted only the target Skill event and did not access files, mailboxes, DNS, Notion, or the network. User-root configuration and permission hashes were unchanged after rollback.

## Evidence and sources

- [中文研究记录](../../research/2026-10-12.md) · [English research](../../research/2026-10-12.en.md)
- [Pinned Skill](https://github.com/sickn33/agentic-awesome-skills/tree/9f2c979c0841ad3f24cc42e38f9f7cb377034558/skills/business-email-template)
- [Agentic Awesome v19.4.0](https://github.com/sickn33/agentic-awesome-skills/releases/tag/v19.4.0)
- [AAS content license](https://github.com/sickn33/agentic-awesome-skills/blob/9f2c979c0841ad3f24cc42e38f9f7cb377034558/LICENSE-CONTENT)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
