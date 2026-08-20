# 在 CodeArts CLI 中使用 guidelines-advisor

[English](README.en.md)

安装 Trail of Bits `guidelines-advisor`，对智能合约仓库做有文件定位的优先级安全审查。

## 选择安装范围

项目级 `<项目>/.codeartsdoer` 适合团队固定审查规则；个人级 `~/.codeartsdoer` 适合当前用户复用。只选一种；任一范围同名冲突时停止。

## 让 Agent 帮你安装

### 项目级提示词

```text
安装并验证项目级 trailofbits/skills guidelines-advisor，固定 Commit 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e。
1. 只创建 .codeartsdoer/vendor/trailofbits-guidelines-advisor 与 .codeartsdoer/skills/guidelines-advisor；不改用户目录、package.json、codearts_cli.json、凭据或其他 Skill。运行 codearts --version、git --version、codearts models 并让我选模型；缺凭据或项目/个人同名冲突就停止。
2. 在项目根 PowerShell 执行：
   $s=Join-Path (Get-Location) '.codeartsdoer\vendor\trailofbits-guidelines-advisor'; $t=Join-Path (Get-Location) '.codeartsdoer\skills\guidelines-advisor'
   New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/trailofbits/skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/building-secure-contracts/skills/guidelines-advisor/' '/LICENSE'
   git -C $s checkout --detach 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
   if((git -C $s rev-parse HEAD).Trim() -ne '9b2813356e9b9ef670dc1f4493a69e82c8e7f27e'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'plugins\building-secure-contracts\skills\guidelines-advisor') -Destination $t -Recurse
3. 创建 smoke fixture，不得加入秘密或真实资金代码。README.md 完整内容为 `# Tiny Vault`、空行、`A deliberately small Solidity vault used only for an isolated CodeArts smoke test.`；test/Vault.t.sol 完整内容为 `// No tests implemented yet. This file intentionally exposes the testing gap.`；contracts/Vault.sol 完整内容必须逐行是：
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
4. debug skill 的唯一来源必须是 $t/SKILL.md。把 <模型> 换成已选 ID，原样运行：codearts run --format json --sandbox --model "<模型>" "Verification contract: call the skill tool exactly once with name guidelines-advisor. Then inspect only README.md, contracts/Vault.sol, and test/Vault.t.sol in this project using read-only file tools. Do not run commands, access the network, or modify files. Return a compact prioritized security review with file references. It must identify the withdraw ordering as a reentrancy risk, recommend checks-effects-interactions, and identify the missing test coverage."
5. 通过要求退出 0、一个 completed skill 事件、只读文件事件均 completed、无写入/命令事件，且结果含 reentrancy、checks-effects-interactions、缺失测试与文件定位。报告证据；卸载只删 $t、$s 和明确创建的三个 fixture，禁止删整个根或用户文件。
```

### 个人级提示词

```text
为当前 Windows 用户安装并验证个人级 guidelines-advisor，固定 trailofbits/skills Commit 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e。
1. 只创建 ~/.codeartsdoer/vendor/trailofbits-guidelines-advisor 与 ~/.codeartsdoer/skills/guidelines-advisor；不改根 package.json、codearts_cli.json、凭据、插件或项目。运行版本/models 检查；同名冲突或缺凭据时停止。
2. PowerShell 执行：
   $u=Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'; $s=Join-Path $u 'vendor\trailofbits-guidelines-advisor'; $t=Join-Path $u 'skills\guidelines-advisor'; $c=Join-Path ([IO.Path]::GetTempPath()) 'codearts-guidelines-9b28133-smoke'
   if((Test-Path $s)-or(Test-Path $t)-or(Test-Path $c)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent),$c -Force|Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/trailofbits/skills.git $s
   git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
   git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/building-secure-contracts/skills/guidelines-advisor/' '/LICENSE'
   git -C $s checkout --detach 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
   if((git -C $s rev-parse HEAD).Trim() -ne '9b2813356e9b9ef670dc1f4493a69e82c8e7f27e'){throw 'Commit mismatch'}
   Copy-Item -LiteralPath (Join-Path $s 'plugins\building-secure-contracts\skills\guidelines-advisor') -Destination $t -Recurse; Set-Location $c
3. 在 $c 创建三个 fixture：README.md 完整内容为 `# Tiny Vault`、空行、`A deliberately small Solidity vault used only for an isolated CodeArts smoke test.`；test/Vault.t.sol 完整内容为 `// No tests implemented yet. This file intentionally exposes the testing gap.`；contracts/Vault.sol 完整内容逐行为 `// SPDX-License-Identifier: MIT`、`pragma solidity ^0.8.24;`、`contract Vault {`、`mapping(address => uint256) public balances;`、`function deposit() external payable { balances[msg.sender] += msg.value; }`、`function withdraw(uint256 amount) external {`、`require(balances[msg.sender] >= amount, "insufficient");`、`(bool ok,) = msg.sender.call{value: amount}("");`、`require(ok, "transfer failed");`、`balances[msg.sender] -= amount;`、`}`、`}`。不得创建项目级同名 Skill。debug skill 必须解析到 $t/SKILL.md；运行：codearts run --format json --sandbox --model "<模型>" "Verification contract: call the skill tool exactly once with name guidelines-advisor. Then inspect only README.md, contracts/Vault.sol, and test/Vault.t.sol in this project using read-only file tools. Do not run commands, access the network, or modify files. Return a compact prioritized security review with file references. It must identify the withdraw ordering as a reentrancy risk, recommend checks-effects-interactions, and identify the missing test coverage."；通过要求一个 completed Skill、只读事件 completed、无写入/命令，并含重入、CEI、缺失测试和文件定位。
4. 卸载仅删 $t、$s 和核对后的整个专用 $c；禁止删用户根、根配置、凭据、真实合约或其他 Skill。
```

## Windows 手动安装

运行版本/models 检查，选择一个范围并执行相应提示词中的固定稀疏检出和整个 Skill 目录复制。

```powershell
$root=Join-Path (Get-Location) '.codeartsdoer' # 个人级改为：Join-Path ([Environment]::GetFolderPath('UserProfile')) '.codeartsdoer'
$s=Join-Path $root 'vendor\trailofbits-guidelines-advisor'; $t=Join-Path $root 'skills\guidelines-advisor'
if((Test-Path $s)-or(Test-Path $t)){throw 'Collision'}; New-Item -ItemType Directory -Path (Split-Path $s -Parent),(Split-Path $t -Parent) -Force|Out-Null
git clone --filter=blob:none --no-checkout https://github.com/trailofbits/skills.git $s
git -C $s config core.longpaths true; git -C $s fetch --depth 1 origin 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
git -C $s sparse-checkout init --no-cone; git -C $s sparse-checkout set --no-cone '/plugins/building-secure-contracts/skills/guidelines-advisor/' '/LICENSE'
git -C $s checkout --detach 9b2813356e9b9ef670dc1f4493a69e82c8e7f27e
Copy-Item -LiteralPath (Join-Path $s 'plugins\building-secure-contracts\skills\guidelines-advisor') -Destination $t -Recurse
```

## CodeArts 配置

模型凭据只按官方方式配置，不要写入仓库或输出。

## 验证

用提示词列出的专用合成 fixture 验证，先核对来源，再运行只读 smoke test；文件内容和成功断言必须相同。不要在未经授权的真实仓库上自动修改代码。

## 使用

```text
Use guidelines-advisor. 只读审查 contracts/，按严重度列出证据、影响、修复建议和缺失测试；不要修改文件。
```

## 更新

`git -C $s rev-parse HEAD` 检查版本；升级需重审许可证、资源和指令，并重做三环境验证。

## 卸载

只删所选范围的 `skills/guidelines-advisor` 与 `vendor/trailofbits-guidelines-advisor`，再确认不被发现。

## 已验证版本与结论

| 项目 | 值 |
| --- | --- |
| 结论 | **Works** |
| Commit | `9b2813356e9b9ef670dc1f4493a69e82c8e7f27e` |
| Skill / SHA-256 | `guidelines-advisor` / `354AA4B2CB2F311A2659BA281C9CC2C33D3107A346062EE245F7F36E5554274B` |
| 许可证 | CC-BY-SA-4.0 |
| 环境与范围 | CodeArts 26.8.1；Windows 11 Build 26200；MiMo；两个项目 + 个人级；2026-08-20 |

## 已知限制

只验证一个合成 Solidity 重入用例，不等于完整审计，也未执行测试、编译器或链上工具。

## 安全

固定目录 6 个文本/SVG 文件、31,468 字节；不运行 npm、脚本、网络、凭据或遥测。输出需人工复核；引用或改编 Skill 内容须遵守 CC-BY-SA-4.0。

## 证据与来源

- [中文研究](../../research/2026-08-20.md) · [English](../../research/2026-08-20.en.md)
- [固定 Commit](https://github.com/trailofbits/skills/tree/9b2813356e9b9ef670dc1f4493a69e82c8e7f27e/plugins/building-secure-contracts/skills/guidelines-advisor)
- [CC-BY-SA-4.0 License](https://github.com/trailofbits/skills/blob/9b2813356e9b9ef670dc1f4493a69e82c8e7f27e/LICENSE)
- [CodeArts Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
