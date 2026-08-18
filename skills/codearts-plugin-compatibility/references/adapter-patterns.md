# CodeArts adapter patterns

Use these as a decision ladder, not as proof that every OpenCode extension is compatible. Each fallback still requires a real CodeArts invocation.

## 1. Prefer CodeArts-native placement

Test project scope first:

- Skills: `<project>/.codeartsdoer/skills/<skill>/SKILL.md`
- Plugins: `<project>/.codeartsdoer/plugin` or `<project>/.codeartsdoer/plugins`
- Plugin dependencies: `<project>/.codeartsdoer/package.json`

For current-user scope, CodeArts uses the matching paths under `~/.codeartsdoer`. Same-named project Skills take priority over user Skills, so user verification must run from a directory without a project override.

For a standalone Skill repository, copy only the pinned Skill directory and its required local resources into the native Skills directory. Do not install a plugin wrapper unless the package also needs Hooks, Commands, or initialization behavior.

## 2. Adapt an OpenCode plugin minimally

For a project-scoped npm or Git dependency:

1. Put the exact pinned dependency in `<project>/.codeartsdoer/package.json`, preserving unrelated fields.
2. Install with lifecycle scripts disabled unless those scripts were audited and are required.
3. If CodeArts does not discover an upstream `.mjs` entrypoint, create a local `.js` or `.ts` wrapper in `.codeartsdoer/plugins` that imports or re-exports the pinned upstream plugin.
4. Verify the wrapper is loaded before changing upstream code.

Do not assume a successful module import makes its Skills callable.

## 3. Bridge dynamic Skill paths to native Skills

On CodeArts CLI 26.8.1, two tested OpenCode plugins added directories through `config.skills.paths`. Those entries appeared in `codearts debug skill`, but the runtime `skill` tool could not call them. Copying the pinned upstream Skill directories into `.codeartsdoer/skills` made them callable.

When the same symptom appears:

1. Keep the minimal wrapper if it provides Hooks, Commands, or other plugin behavior.
2. Copy only the expected pinned Skill directories into CodeArts' native Skills directory.
3. Refuse same-name collisions rather than overwriting another source.
4. Require a completed runtime `skill` event whose base directory resolves to the intended native copy.

Record this result as `Adapter Required`, not `Works`.

Choose runtime assertions from the `SKILL.md` body. CodeArts CLI 26.8.1 uses frontmatter for discovery but omits it from the content returned by the `skill` tool, so a model cannot reliably answer a smoke-test question about frontmatter metadata after that tool call.

## 4. Isolate user dependencies

Do not casually merge third-party dependencies into an existing user-root `~/.codeartsdoer/package.json`. A tested CodeArts 26.8.1 user manifest contained an internal `@opencode-ai/plugin` version that public npm could not resolve; both npm and automatic reification failed after the manifest was changed.

When user scope is appropriate, prefer an independently removable layout:

```text
~/.codeartsdoer/
  plugins/<slug>.ts
  vendor/<slug>/
    package.json
    package-lock.json
    node_modules/
  skills/<skill>/
```

The user plugin wrapper should import the pinned entrypoint through a path relative to `~/.codeartsdoer/plugins`. Install dependencies only inside `vendor/<slug>`, copy required Skills into the native user Skills directory, and leave CodeArts' root manifest unchanged.

Before testing, record hashes only for non-secret files that may need restoration. After rollback, confirm the wrapper, vendor directory, and copied Skills are absent and those hashes match.

## 5. Diagnose by capability layer

Use the smallest relevant check:

| Layer | Evidence | Common next check |
| --- | --- | --- |
| Package | Exact version and files exist | Resolve the real entrypoint |
| Plugin | CodeArts loads the wrapper without error | Inspect registered Hooks or Commands |
| Skill discovery | Diagnostic lists name and location | Inspect runtime `skill` inventory |
| Skill invocation | Completed `skill` event from intended base directory | Exercise one representative instruction |
| Hook | Observable before/after transformation or event | Confirm it came from the pinned plugin |
| MCP | Server starts and exposes expected tools | Invoke one read-only tool |

If upstream instructions mention host-specific tools such as OpenCode `task` or `todowrite`, distinguish content portability from host-tool portability. A Skill may load successfully while some workflows remain `Partial`.

## 6. Provenance of these patterns

The `.mjs` wrapper, dynamic Skill-path, native-copy, isolated-user-vendor, clean-consumer, and rollback patterns were observed with:

- Ponytail 4.9.0 on CodeArts CLI 26.8.1
- Superpowers 6.3.0 on CodeArts CLI 26.8.1

They are preferred hypotheses for later candidates, not guarantees across CodeArts versions or extension types.
