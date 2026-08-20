# Use guidelines-advisor with CodeArts CLI

[简体中文](README.md)

Install Trail of Bits `guidelines-advisor` for prioritized smart-contract security reviews with file evidence.

## Choose a scope

Use `<project>/.codeartsdoer` for team-pinned review rules or `~/.codeartsdoer` for current-user reuse. Choose one and stop on a same-name collision in either scope.

## Ask Agent to install it

### Project prompt

```text
Install and verify project-scoped guidelines-advisor from trailofbits/skills at commit 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e.
1. Create only .codeartsdoer/vendor/trailofbits-guidelines-advisor and .codeartsdoer/skills/guidelines-advisor. Do not change user files, package.json, codearts_cli.json, credentials, or other skills. Run codearts --version, git --version, and codearts models and ask me for an available model; stop on missing credentials or project/user collisions.
2. From the project root run in PowerShell:
   $s=Join-Path (Get-Location) '.codeartsdoer\vendor\trailofbits-guidelines-advisor'; $t=Join-Path (Get-Location) '.codeartsdoer\skills\guidelines-advisor'
   New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/trailofbits/skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/building-secure-contracts/skills/guidelines-advisor/' '/LICENSE'
   git -C $s checkout --detach 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
   if((git -C $s rev-parse HEAD).Trim() -ne '9b2813356e9b9ef670dc1f4493a69e82c8e7f27e'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'plugins\building-secure-contracts\skills\guidelines-advisor') -Destination $t -Recurse
3. Create a smoke fixture with no secrets or real-funds code. README.md is exactly `# Tiny Vault`, a blank line, then `A deliberately small Solidity vault used only for an isolated CodeArts smoke test.` test/Vault.t.sol is exactly `// No tests implemented yet. This file intentionally exposes the testing gap.` contracts/Vault.sol must be exactly these lines:
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.24;
   contract Vault {
       mapping(address => uint256) public balances;
       function deposit() external payable { balances[msg.sender] += msg.value; }
       function withdraw(uint256 amount) external {
           require(balances[msg.sender] >= amount, "insufficient");
           (bool ok,) = msg.sender.call{value: amount}("");
           require(ok, "transfer failed");
           balances[msg.sender] -= amount;
       }
   }
4. The sole debug-skill source must be $t/SKILL.md. Replace <model> and run verbatim: codearts run --format json --sandbox --model "<model>" "Verification contract: call the skill tool exactly once with name guidelines-advisor. Then inspect only README.md, contracts/Vault.sol, and test/Vault.t.sol in this project using read-only file tools. Do not run commands, access the network, or modify files. Return a compact prioritized security review with file references. It must identify the withdraw ordering as a reentrancy risk, recommend checks-effects-interactions, and identify the missing test coverage."
5. Pass only on exit 0, one completed skill event, completed read-only file events, no write/command event, and reentrancy/CEI/missing-test/file evidence. Report evidence. Remove only $t, $s, and the three explicit fixtures; never delete the whole root or user files.
```

### User prompt

```text
Install and verify user-scoped guidelines-advisor from trailofbits/skills commit 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e.
1. Create only ~/.codeartsdoer/vendor/trailofbits-guidelines-advisor and ~/.codeartsdoer/skills/guidelines-advisor; do not alter root package.json, codearts_cli.json, credentials, plugins, or projects. Run version/models checks; stop on missing credentials or collisions.
2. In PowerShell run:
   $u=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'; $s=Join-Path $u 'vendor\trailofbits-guidelines-advisor'; $t=Join-Path $u 'skills\guidelines-advisor'; $c=Join-Path ([IO.Path]::GetTempPath()) 'codearts-guidelines-9b28133-smoke'
   if((Test-Path $s)-or(Test-Path $t)-or(Test-Path $c)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent),$c -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/trailofbits/skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/building-secure-contracts/skills/guidelines-advisor/' '/LICENSE'
   git -C $s checkout --detach 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
   if((git -C $s rev-parse HEAD).Trim() -ne '9b2813356e9b9ef670dc1f4493a69e82c8e7f27e'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'plugins\building-secure-contracts\skills\guidelines-advisor') -Destination $t -Recurse; Set-Location $c
3. Create these exact fixtures in $c: README.md is `# Tiny Vault`, a blank line, and `A deliberately small Solidity vault used only for an isolated CodeArts smoke test.` test/Vault.t.sol is exactly `// No tests implemented yet. This file intentionally exposes the testing gap.` contracts/Vault.sol lines are `// SPDX-License-Identifier: MIT`, `pragma solidity ^0.8.24;`, `contract Vault {`, `mapping(address => uint256) public balances;`, `function deposit() external payable { balances[msg.sender] += msg.value; }`, `function withdraw(uint256 amount) external {`, `require(balances[msg.sender] >= amount, "insufficient");`, `(bool ok,) = msg.sender.call{value: amount}("");`, `require(ok, "transfer failed");`, `balances[msg.sender] -= amount;`, `}`, `}`. Do not create a project-scoped same-name skill. Confirm the user source, then run: codearts run --format json --sandbox --model "<model>" "Verification contract: call the skill tool exactly once with name guidelines-advisor. Then inspect only README.md, contracts/Vault.sol, and test/Vault.t.sol in this project using read-only file tools. Do not run commands, access the network, or modify files. Return a compact prioritized security review with file references. It must identify the withdraw ordering as a reentrancy risk, recommend checks-effects-interactions, and identify the missing test coverage." Apply the same tool and content assertions.
4. Remove only $t, $s, and the verified dedicated $c. Never delete the user root, root configuration, credentials, real contracts, or another skill.
```

## Manual Windows installation

Run version/model checks, choose one scope, and execute the matching prompt's exact sparse checkout and full-directory copy.

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # User: Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$s=Join-Path $root 'vendor\trailofbits-guidelines-advisor'; $t=Join-Path $root 'skills\guidelines-advisor'
if((Test-Path $s)-or(Test-Path $t)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
git clone --filter=blob:none --no-checkout https://github.com/trailofbits/skills.git $s
git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/building-secure-contracts/skills/guidelines-advisor/' '/LICENSE'
git -C $s checkout --detach 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
Copy-Item -LiteralPath (Join-Path $s 'plugins\building-secure-contracts\skills\guidelines-advisor') -Destination $t -Recurse
```

## CodeArts configuration

Configure credentials only through official mechanisms and never write them to the repository or output.

## Verification

Use the exact synthetic fixture specified in the prompts, verify the source, and run the same read-only smoke test and assertions; do not modify an unauthorized real repository.

## Use

```text
Use guidelines-advisor. Read-only review contracts/ and list evidence, impact, remediation, and missing tests by severity; modify nothing.
```

## Update

Check `git -C $s rev-parse HEAD`; re-audit and revalidate upgrades.

## Remove

Remove only `skills/guidelines-advisor` and `vendor/trailofbits-guidelines-advisor` in the chosen scope, then confirm absence.

## Verified compatibility

| Item | Value |
| --- | --- |
| Result | **Works** |
| Commit | `9b2813356e9b9ef670dc1f4493a69e82c8e7f27e` |
| Skill / SHA-256 | `guidelines-advisor` / `354AA4B2CB2F311A2659BA281C9CC2C33D3107A346062EE245F7F36E5554274B` |
| License | CC-BY-SA-4.0 |
| Environment/scopes | CodeArts 26.8.1; Windows 11 Build 26200; MiMo; two projects + user; 2026-08-20 |

## Known limitations

Only one synthetic Solidity reentrancy case was tested; this is not a complete audit, and no tests, compiler, or chain tool ran.

## Security

Six text/SVG files total 31,468 bytes and run no npm, script, runtime network, credential, or telemetry step. Review output manually; attribution/share-alike applies to adapted skill content.

## Evidence and sources

- [中文研究](../../research/2026-08-20.md) · [English](../../research/2026-08-20.en.md)
- [Pinned commit](https://github.com/trailofbits/skills/tree/9b2813356e9b9ef670dc1f4493a69e82c8e7f27e/plugins/building-secure-contracts/skills/guidelines-advisor)
- [CC-BY-SA-4.0 License](https://github.com/trailofbits/skills/blob/9b2813356e9b9ef670dc1f4493a69e82c8e7f27e/LICENSE)
- [Official CodeArts Skills](https://support.huaweicloud.com/intl/en-us/usermanual-cli/codeartsagent_cli_0019.html)
