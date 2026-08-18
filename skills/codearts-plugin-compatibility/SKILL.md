---
name: codearts-plugin-compatibility
description: Research, safely install, adapt, and verify third-party agent plugins and SKILL.md packages on CodeArts CLI before publishing compatibility guidance. Use for GitHub agent-extension compatibility work, not ordinary CodeArts usage or unverified catalog summaries.
---

# CodeArts Plugin Compatibility

Produce an evidence-backed CodeArts compatibility result and, only after a real runtime pass, an installation-first guide.

## Required evidence boundary

Treat these as different claims:

1. **Installed**: files and dependencies exist.
2. **Discovered**: CodeArts diagnostic output lists the extension.
3. **Loaded**: the real CodeArts session registers the expected plugin, Skill, Hook, Command, or MCP capability.
4. **Invoked**: a representative capability produces an observable result in `codearts run`.
5. **Reproduced and removed**: the final process passes in a clean environment and rollback restores the baseline.

Do not publish a passing compatibility status unless all claims required for that extension type are demonstrated. A plausible model answer without the expected completed tool or hook event is not invocation evidence.

## Workflow

1. Confirm the repository is clean enough to isolate new work. Record pre-existing changes and do not include them in the result.
2. Read current CodeArts first-party documentation for the relevant extension type. Record the CodeArts CLI, operating system, runtime, upstream tag or commit, license, and verification date.
3. Inspect the pinned upstream source before installation: manifests, lockfiles, lifecycle scripts, binaries, downloads, network calls, credentials, telemetry, and files that will be executed.
4. Classify the candidate as a native CodeArts extension, standalone Skill package, OpenCode-compatible plugin, MCP integration, Hook, Command, or mixed package.
5. Read [references/adapter-patterns.md](references/adapter-patterns.md), choose the least invasive matching path, and test project scope first in an isolated project.
6. Run discovery diagnostics, then invoke one capability with a prompt whose result can be attributed to the installed extension. Preserve the completed tool or hook event and the resolved source path.
7. Repeat the final process from zero in a second isolated project. For a useful cross-project tool, separately test user scope from a directory with no same-named project installation.
8. Remove only the installed targets. Confirm discovery and runtime behavior return to baseline and any pre-existing user manifest has the same hash as before.
9. Assign one result: `Native`, `Works`, `Partial`, `Adapter Required`, `Not Working`, or `Untested`. Separate observed facts from inferences and list untested clients, platforms, and workflows.
10. When publishing, read [references/publishing-contract.md](references/publishing-contract.md) and create matching Chinese-primary and English-secondary evidence.

## Stop conditions

Stop before execution when the pinned source is unavailable, its license is unsuitable for the intended reuse, required scripts or binaries cannot be audited, credentials would need to be exposed, or the operation would overwrite an existing plugin or same-named Skill.

Stop a failing branch after its evidence distinguishes installation, discovery, loading, and invocation. Try the relevant minimal adapter from the reference; do not make unrelated upstream rewrites merely to obtain a passing result.

Keep credentials in the inherited CodeArts environment. Never print, copy, hash, or commit their values. Do not use `--auto` for compatibility tests unless the tested capability genuinely requires writes and that extra authorization is explicitly in scope.
