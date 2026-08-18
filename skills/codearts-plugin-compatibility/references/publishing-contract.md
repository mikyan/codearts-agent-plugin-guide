# Compatibility publishing contract

Read this only after testing produces a result worth recording.

## Evidence record

Create a Chinese-primary `research/YYYY-MM-DD.md` and matching English `research/YYYY-MM-DD.en.md`. Include:

- candidate source and selection date;
- stars or other popularity snapshot when relevant;
- pinned tag, release, commit, and license;
- CodeArts, OS, shell, Node/npm/Git, and model identifiers without credentials;
- security review and exact installation scope;
- installation, discovery, loading, invocation, reproduction, and rollback outcomes;
- exact completed tool or hook event criteria;
- facts that failed, limitations, and untested environments.

Do not turn a failed experiment into a formal installation guide. A research record may document `Not Working` or `Untested` without adding a guide or root index entry.

## Guide order

For a passing candidate, create Chinese `guides/<slug>/README.md` and English `guides/<slug>/README.en.md` in this order:

1. One-sentence purpose.
2. Project versus user installation choice, including paths, priority, and impact.
3. Copy-ready project and user Agent prompts for every verified scope.
4. Equivalent manual installation.
5. CodeArts model or environment configuration.
6. Discovery and real invocation verification.
7. Usage, update, and exact removal.
8. Verified versions, result, limitations, security, evidence, and primary sources.

## Agent prompt completeness

Each prompt must stand alone. State:

- exact target files;
- complete new-file contents or exact fields to merge while preserving unrelated configuration;
- pinned sources and install commands, including working directory and lifecycle-script policy;
- exact copy source, destination, expected directory names, and collision behavior;
- how to select the configured `provider/model` ID;
- a directly executable `codearts run` smoke test;
- the required completed tool or Hook event, resolved source path, and final observable result;
- exact removal targets and files that must not be touched.

Instructions such as “add the dependency,” “install the plugin,” or “copy the Skills” are incomplete unless the same prompt supplies those details.

## Language and consistency

Chinese is the default repository language. English is secondary. The two versions must independently express the same versions, paths, commands, compatibility status, limitations, date, and source links. Root indexes link Chinese before English.
