# 在 CodeArts CLI 中使用 Google GKE Manifest Generation

[English](README.en.md)

从 Google Skills 安装经过验证的 `gke-manifest-generation` Skill，生成带资源限制、安全上下文、探针和可用性配置的 GKE YAML。

## 选择安装范围

| 范围 | 安装位置 | 适合场景 |
| --- | --- | --- |
| 项目级 | `<项目根目录>/.codeartsdoer` | 随仓库固定版本、团队共享或只在一个项目使用。默认推荐。 |
| 个人级 | `~/.codeartsdoer` | 当前用户需要在多个项目中反复使用。 |

CodeArts 同名 Skill 以项目级为优先。不要在两个范围重复安装同一版本，除非有意让项目覆盖个人配置。

## 让 Agent 帮你安装

### 项目级安装提示词

```text
请在当前项目中为 CodeArts CLI 安装并验证 google/skills 的 gke-manifest-generation Skill，源码固定到 Commit 65ef106f1e902b546d37525e8b3bfd4921db0f92。

严格执行：
1. 只修改当前项目的 .codeartsdoer；不要修改 ~/.codeartsdoer、凭据或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；模型不明确时先询问我选择 provider/model ID。
3. 检查 .codeartsdoer/vendor/google-skills 和 .codeartsdoer/skills/gke-manifest-generation。任一存在就停止并报告，不得覆盖。
4. 在项目根目录的 PowerShell 原样执行：
   $source = Join-Path (Get-Location) ".codeartsdoer\vendor\google-skills"
   $target = Join-Path (Get-Location) ".codeartsdoer\skills\gke-manifest-generation"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/google/skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/cloud/gke-manifest-generation"
   git -C $source checkout --detach "65ef106f1e902b546d37525e8b3bfd4921db0f92"
   if ((git -C $source rev-parse HEAD).Trim() -ne "65ef106f1e902b546d37525e8b3bfd4921db0f92") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\cloud\gke-manifest-generation") -Destination $target -Recurse
5. 运行 codearts debug skill，确认 gke-manifest-generation 的 location 位于当前项目 .codeartsdoer/skills/gke-manifest-generation/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步确认的模型，然后原样执行：
   codearts run --format json --sandbox --model "<选择的模型>" "Call the skill tool exactly once with name gke-manifest-generation. Do not use any other tool. Generate only Kubernetes YAML for a two-replica nginx deployment and internal service in namespace tea-dev. Include every security, resource, health, Spot VM, service-account, and availability requirement that the skill mandates for this case."
7. 通过标准：JSON 中必须只有一次成功的 `gke-manifest-generation` Skill 调用；最终 YAML 至少包含 `tea-dev` Namespace、专用 ServiceAccount、Resources、Liveness/Readiness、非 Root、安全只读根文件系统、Spot NodeSelector/Toleration、PodDisruptionBudget 和 `ClusterIP` Service。
8. 报告 Commit、修改路径、工具事件、最终结果和卸载清单。任一步失败就如实停止，不得宣称成功。
```

### 个人级安装提示词

```text
请为当前 Windows 用户安装并验证个人级 google/skills gke-manifest-generation Skill，源码固定到 Commit 65ef106f1e902b546d37525e8b3bfd4921db0f92。

严格执行：
1. 在 PowerShell 执行 $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"。只修改该目录下 vendor/google-skills 和 skills/cloud/gke-manifest-generation；不要修改 $userRoot/package.json、codearts_cli.json、凭据、项目配置或全局软件。
2. 运行 codearts --version、git --version 和 codearts models；模型不明确时先询问我选择 provider/model ID。
3. 检查 $userRoot/vendor/google-skills 和 $userRoot/skills/cloud/gke-manifest-generation。任一存在就停止并报告，不得覆盖。
4. 在同一个 PowerShell 会话原样执行：
   $source = Join-Path $userRoot "vendor\google-skills"
   $target = Join-Path $userRoot "skills\gke-manifest-generation"
   if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
   New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
   New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
   git clone --filter=blob:none --no-checkout https://github.com/google/skills.git $source
   git -C $source sparse-checkout init --cone
   git -C $source sparse-checkout set "skills/cloud/gke-manifest-generation"
   git -C $source checkout --detach "65ef106f1e902b546d37525e8b3bfd4921db0f92"
   if ((git -C $source rev-parse HEAD).Trim() -ne "65ef106f1e902b546d37525e8b3bfd4921db0f92") { throw "Unexpected commit." }
   Copy-Item -LiteralPath (Join-Path $source "skills\cloud\gke-manifest-generation") -Destination $target -Recurse
5. 在没有项目级同名 Skill 的目录运行 codearts debug skill，确认 gke-manifest-generation 的 location 位于当前用户 .codeartsdoer/skills/gke-manifest-generation/SKILL.md。
6. 把 <选择的模型> 替换为第 2 步确认的模型，在同一目录原样执行：
   codearts run --format json --sandbox --model "<选择的模型>" "Call the skill tool exactly once with name gke-manifest-generation. Do not use any other tool. Generate only Kubernetes YAML for a two-replica nginx deployment and internal service in namespace tea-dev. Include every security, resource, health, Spot VM, service-account, and availability requirement that the skill mandates for this case."
7. 通过标准：JSON 中必须只有一次成功的 `gke-manifest-generation` Skill 调用；最终 YAML 至少包含 `tea-dev` Namespace、专用 ServiceAccount、Resources、Liveness/Readiness、非 Root、安全只读根文件系统、Spot NodeSelector/Toleration、PodDisruptionBudget 和 `ClusterIP` Service。
8. 卸载只能移除 $userRoot/skills/cloud/gke-manifest-generation 和 $userRoot/vendor/google-skills。报告工具事件与结果；任一步失败不得宣称成功。
```

## Windows 手动安装

### 前置条件

安装 CodeArts CLI 和 Git，并确认模型可用：

```powershell
codearts --version
git --version
codearts models
```

### 项目级

在目标项目根目录执行：

```powershell
$source = Join-Path (Get-Location) ".codeartsdoer\vendor\google-skills"
$target = Join-Path (Get-Location) ".codeartsdoer\skills\gke-manifest-generation"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/google/skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/cloud/gke-manifest-generation"
git -C $source checkout --detach "65ef106f1e902b546d37525e8b3bfd4921db0f92"
if ((git -C $source rev-parse HEAD).Trim() -ne "65ef106f1e902b546d37525e8b3bfd4921db0f92") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\cloud\gke-manifest-generation") -Destination $target -Recurse
```

### 个人级

```powershell
$userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer"
$source = Join-Path $userRoot "vendor\google-skills"
$target = Join-Path $userRoot "skills\gke-manifest-generation"
if ((Test-Path -LiteralPath $source) -or (Test-Path -LiteralPath $target)) { throw "Source or target already exists; inspect it instead of overwriting." }
New-Item -ItemType Directory -Path (Split-Path $source -Parent) -Force | Out-Null
New-Item -ItemType Directory -Path (Split-Path $target -Parent) -Force | Out-Null
git clone --filter=blob:none --no-checkout https://github.com/google/skills.git $source
git -C $source sparse-checkout init --cone
git -C $source sparse-checkout set "skills/cloud/gke-manifest-generation"
git -C $source checkout --detach "65ef106f1e902b546d37525e8b3bfd4921db0f92"
if ((git -C $source rev-parse HEAD).Trim() -ne "65ef106f1e902b546d37525e8b3bfd4921db0f92") { throw "Unexpected commit." }
Copy-Item -LiteralPath (Join-Path $source "skills\cloud\gke-manifest-generation") -Destination $target -Recurse
```

两种范围都不修改 CodeArts 模型配置，也不执行第三方安装脚本。

## CodeArts 配置

本 Skill 不需要修改 `codearts_cli.json`。如果尚未安装 CodeArts CLI，先按[官方安装说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html)完成安装；然后列出当前可用模型：

```powershell
codearts models
```

在后续命令中把 `mimo/mimo-v2.5` 替换为列表里的实际 `provider/model` ID。需要自定义模型时，参考[官方配置示例](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html)；API Key 只保留在本地配置或环境变量中，不要写入本项目。

CodeArts CLI 26.8.1 在本次自定义 Provider 测试中仍要求当前进程存在 `CODEARTS_CLI_AK` 和 `CODEARTS_CLI_SK`。本次使用非秘密占位值即可通过前置检查，实际模型鉴权使用 Provider 自己的 API Key；这是实测现象，不是兼容保证。使用华为云托管模型时应按[官方 AK/SK 说明](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html)配置有效凭据。

## 验证

先检查来源路径：

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills | Where-Object { $_.name -eq "gke-manifest-generation" } | Select-Object name, location
```

再进行真实调用；替换模型 ID：

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name gke-manifest-generation. Do not use any other tool. Generate only Kubernetes YAML for a two-replica nginx deployment and internal service in namespace tea-dev. Include every security, resource, health, Spot VM, service-account, and availability requirement that the skill mandates for this case."
```

JSON 中必须只有一次成功的 `gke-manifest-generation` Skill 调用；最终 YAML 至少包含 `tea-dev` Namespace、专用 ServiceAccount、Resources、Liveness/Readiness、非 Root、安全只读根文件系统、Spot NodeSelector/Toleration、PodDisruptionBudget 和 `ClusterIP` Service。 只有最终文本但没有成功的 `skill` 工具事件，不能视为通过。

## 使用

```text
Call the skill tool with name gke-manifest-generation, then generate a hardened GKE manifest for this workload without applying it to a cluster.
```

## 更新与卸载

更新时先审查新 Commit，再移除所选范围内的旧 Skill 和专用 `vendor/google-skills`，按相同步骤重新安装并复测。不要复用浮动的 `main` 作为已验证版本。

项目级卸载只移除：

- `.codeartsdoer/skills/gke-manifest-generation`
- `.codeartsdoer/vendor/google-skills`

个人级卸载只移除：

- `~/.codeartsdoer/skills/gke-manifest-generation`
- `~/.codeartsdoer/vendor/google-skills`

不要删除 CodeArts 用户根 `package.json`、`codearts_cli.json` 或其他 Skills。

## 已验证版本与结论

| 项目 | 已验证值 |
| --- | --- |
| 兼容状态 | **Partial** |
| 上游项目 | [google/skills](https://github.com/google/skills) |
| 固定源码 | [65ef106](https://github.com/google/skills/commit/65ef106f1e902b546d37525e8b3bfd4921db0f92) |
| 已验证 Skill | `gke-manifest-generation` |
| 许可证 | Apache-2.0 |
| CodeArts | Windows 11 上的 CLI 26.8.1 |
| 测试模型 | `mimo/mimo-v2.5` |
| 已验证范围 | 项目级、个人级 |
| 最后验证日期 | 2026-08-19 |

该安装在两个全新项目中复现，随后又从没有项目配置的目录验证个人级加载。真实 CodeArts 会话成功调用 `gke-manifest-generation` 并完成上述代表任务；三个范围均完成回滚，回滚后 Skill 不再被发现。

基本 GKE YAML 生成已验证，但没有连接真实集群、执行 `gcloud`、查询 Google Developer Knowledge API，也未验证推理工作负载和全部 References。上游把部分场景的这些工具列为必需，因此状态为 `Partial`。

## 安全说明

- 只复制固定 Commit 下的所选 Skill 目录；不运行仓库中的其他代码。
- 安装前审阅 `SKILL.md` 及随目录复制的资源。
- Skill 指令会影响 Agent 行为；敏感仓库中使用前应先审阅。
- 本流程不需要 `--auto`，也不读取或写入凭据。

## 证据与来源

- [2026-08-19 批量实测记录](../../research/2026-08-19.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [固定版本 Skill](https://github.com/google/skills/blob/65ef106f1e902b546d37525e8b3bfd4921db0f92/skills/cloud/gke-manifest-generation/SKILL.md)
- [上游仓库](https://github.com/google/skills)
