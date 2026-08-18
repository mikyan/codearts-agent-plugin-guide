# Google GKE Manifest Generation on CodeArts CLI

[简体中文](README.md)

Install the verified `gke-manifest-generation` skill from Google Skills to generate GKE YAML with resources, security contexts, probes, and availability controls.

## Choose an installation scope

| Scope | Location | Best for |
| --- | --- | --- |
| Project | `<project root>/.codeartsdoer` | Repository-pinned versions, team sharing, or one project. Recommended by default. |
| User | `~/.codeartsdoer` | Repeated use across projects for the current user. |

A same-named project skill takes priority over a user skill. Do not install the same version in both scopes unless the project intentionally overrides the user copy.

## Install with an agent

### Project installation prompt

```text
Install and verify the gke-manifest-generation skill from google/skills for CodeArts CLI in the current project, pinned to commit 65ef106f1e902b546d37525e8b3bfd4921db0f92.

Follow exactly:
1. Modify only this project's .codeartsdoer. Do not modify ~/.codeartsdoer, credentials, or global software.
2. Run codearts --version, git --version, and codearts models. Ask me to select a provider/model ID if ambiguous.
3. Check .codeartsdoer/vendor/google-skills and .codeartsdoer/skills/gke-manifest-generation. Stop and report if either exists; never overwrite.
4. From the project root in PowerShell run exactly:
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
5. Run codearts debug skill. Confirm gke-manifest-generation resolves under this project's .codeartsdoer/skills/gke-manifest-generation/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name gke-manifest-generation. Do not use any other tool. Generate only Kubernetes YAML for a two-replica nginx deployment and internal service in namespace tea-dev. Include every security, resource, health, Spot VM, service-account, and availability requirement that the skill mandates for this case."
7. Passing criteria: JSON must contain exactly one completed `gke-manifest-generation` skill call. Final YAML must include the `tea-dev` namespace, a dedicated ServiceAccount, resources, liveness/readiness probes, non-root and read-only-root security, Spot node selector/toleration, a PodDisruptionBudget, and a `ClusterIP` service.
8. Report the commit, changed paths, tool event, final result, and removal list. If any step fails, stop and do not claim success.
```

### User installation prompt

```text
Install and verify the user-scoped gke-manifest-generation skill from google/skills for the current Windows user, pinned to commit 65ef106f1e902b546d37525e8b3bfd4921db0f92.

Follow exactly:
1. In PowerShell run $userRoot = Join-Path ([Environment]::GetFolderPath("UserProfile")) ".codeartsdoer". Modify only vendor/google-skills and skills/cloud/gke-manifest-generation below it. Do not modify $userRoot/package.json, codearts_cli.json, credentials, project configuration, or global software.
2. Run codearts --version, git --version, and codearts models. Ask me to select a provider/model ID if ambiguous.
3. Check $userRoot/vendor/google-skills and $userRoot/skills/cloud/gke-manifest-generation. Stop and report if either exists; never overwrite.
4. In the same PowerShell session run exactly:
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
5. From a directory with no same-named project skill, run codearts debug skill. Confirm gke-manifest-generation resolves under the current user's .codeartsdoer/skills/gke-manifest-generation/SKILL.md.
6. Replace <selected model> with the model from step 2 and run exactly from the same directory:
   codearts run --format json --sandbox --model "<selected model>" "Call the skill tool exactly once with name gke-manifest-generation. Do not use any other tool. Generate only Kubernetes YAML for a two-replica nginx deployment and internal service in namespace tea-dev. Include every security, resource, health, Spot VM, service-account, and availability requirement that the skill mandates for this case."
7. Passing criteria: JSON must contain exactly one completed `gke-manifest-generation` skill call. Final YAML must include the `tea-dev` namespace, a dedicated ServiceAccount, resources, liveness/readiness probes, non-root and read-only-root security, Spot node selector/toleration, a PodDisruptionBudget, and a `ClusterIP` service.
8. Removal may include only $userRoot/skills/cloud/gke-manifest-generation and $userRoot/vendor/google-skills. Report the tool event and result; do not claim success after any failure.
```

## Install manually on Windows

### Prerequisites

Install CodeArts CLI and Git, then confirm the intended model:

```powershell
codearts --version
git --version
codearts models
```

### Project scope

Run from the target project root:

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

### User scope

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

Neither scope changes CodeArts model configuration or executes third-party install scripts.

## CodeArts configuration

This skill does not require changes to `codearts_cli.json`. If CodeArts CLI is not installed, follow the [official installation guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0005.html), then list the available models:

```powershell
codearts models
```

Replace `mimo/mimo-v2.5` in later commands with an actual `provider/model` ID from that list. For a custom model, follow the [official configuration example](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_00022.html). Keep API keys only in local configuration or environment variables, never in this project.

In this custom-provider test, CodeArts CLI 26.8.1 still required `CODEARTS_CLI_AK` and `CODEARTS_CLI_SK` to exist in the process. Non-secret placeholders passed that preflight while the Provider's own API key authenticated the model request. This is observed behavior, not a compatibility guarantee. Use valid credentials as described in the [official AK/SK guide](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0026.html) for Huawei Cloud-hosted models.

## Verify

Check the resolved source path first:

```powershell
$skills = codearts debug skill 2>$null | Out-String | ConvertFrom-Json
$skills | Where-Object { $_.name -eq "gke-manifest-generation" } | Select-Object name, location
```

Then perform the real call, replacing the model ID if needed:

```powershell
codearts run --format json --sandbox --model "mimo/mimo-v2.5" `
  "Call the skill tool exactly once with name gke-manifest-generation. Do not use any other tool. Generate only Kubernetes YAML for a two-replica nginx deployment and internal service in namespace tea-dev. Include every security, resource, health, Spot VM, service-account, and availability requirement that the skill mandates for this case."
```

JSON must contain exactly one completed `gke-manifest-generation` skill call. Final YAML must include the `tea-dev` namespace, a dedicated ServiceAccount, resources, liveness/readiness probes, non-root and read-only-root security, Spot node selector/toleration, a PodDisruptionBudget, and a `ClusterIP` service. A plausible final answer without a completed `skill` event is not a pass.

## Use

```text
Call the skill tool with name gke-manifest-generation, then generate a hardened GKE manifest for this workload without applying it to a cluster.
```

## Update and remove

Review a new commit before updating. Remove the old skill and dedicated `vendor/google-skills` in the selected scope, reinstall the new pinned commit, and repeat verification. Do not treat floating `main` as a verified version.

Project removal targets only:

- `.codeartsdoer/skills/gke-manifest-generation`
- `.codeartsdoer/vendor/google-skills`

User removal targets only:

- `~/.codeartsdoer/skills/gke-manifest-generation`
- `~/.codeartsdoer/vendor/google-skills`

Do not delete the CodeArts user-root `package.json`, `codearts_cli.json`, or unrelated skills.

## Verified compatibility

| Item | Verified value |
| --- | --- |
| Compatibility | **Partial** |
| Upstream | [google/skills](https://github.com/google/skills) |
| Pinned source | [65ef106](https://github.com/google/skills/commit/65ef106f1e902b546d37525e8b3bfd4921db0f92) |
| Verified skill | `gke-manifest-generation` |
| License | Apache-2.0 |
| CodeArts | CLI 26.8.1 on Windows 11 |
| Model | `mimo/mimo-v2.5` |
| Verified scopes | Project and user |
| Last verified | 2026-08-19 |

The installation was reproduced in two fresh projects and then verified at user scope from a directory without project configuration. Real CodeArts sessions called `gke-manifest-generation` and completed the representative task above. All three scopes were rolled back, after which the skill disappeared from discovery.

Basic GKE YAML generation was verified, but no live cluster, `gcloud`, Google Developer Knowledge API, inference workload, or complete reference workflow was tested. Upstream requires some of those tools for specific scenarios, so the result is `Partial`.

## Security notes

- Copy only the selected skill directory from the pinned commit; do not execute unrelated repository code.
- Review `SKILL.md` and resources copied with it before installation.
- Skill instructions influence agent behavior; review them before use in sensitive repositories.
- This process does not require `--auto` or read/write credentials.

## Evidence and sources

- [2026-08-19 batch verification](../../research/2026-08-19.en.md)
- [CodeArts CLI Skills](https://support.huaweicloud.com/usermanual-cli/codeartsagent_cli_0019.html)
- [Pinned skill](https://github.com/google/skills/blob/65ef106f1e902b546d37525e8b3bfd4921db0f92/skills/cloud/gke-manifest-generation/SKILL.md)
- [Upstream repository](https://github.com/google/skills)
