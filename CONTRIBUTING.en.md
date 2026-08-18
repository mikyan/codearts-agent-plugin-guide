# Contributing

[简体中文](CONTRIBUTING.md)

Thank you for helping improve CodeArts Agent compatibility knowledge.

## Propose a candidate

Open an issue with the upstream repository URL, the capability you want to use, and any CodeArts environment details you already know. Popularity alone is not enough: active maintenance, a clear license, installation risk, and practical value all matter.

## Submit or update a guide

Every guide must include matching English and Chinese documents and cover:

- `README.md` is the default Chinese document and `README.en.md` is the secondary English document; research logs use `YYYY-MM-DD.md` for Chinese and `YYYY-MM-DD.en.md` for English;

- upstream purpose, repository, version, and license;
- CodeArts compatibility status and supporting evidence;
- tested and untested environments;
- project and user scope explained before installation, including paths, priority, and impact;
- copy-ready agent prompts that name every target file, full content or exact merge field, install command, copy source and destination, conflict behavior, real-call acceptance criteria, and removal list; vague instructions such as “add the dependency” are not sufficient;
- installation, verification, usage, troubleshooting, and removal;
- permissions, credentials, install scripts, binaries, and other security considerations;
- known limitations, source links, and the last verification date.

Use only these status values: `Native`, `Works`, `Partial`, `Adapter Required`, `Not Working`, and `Untested`.

Do not claim a successful test unless you performed and recorded it. A user-scope claim must be verified from a directory without a same-named project installation and must include exact rollback. Do not include secrets, personal paths, or instructions that silently alter existing user configuration.

## Pull request checklist

- [ ] English and Chinese content describe the same result.
- [ ] Chinese is the default entrypoint and English files use the `.en.md` suffix.
- [ ] Versions, dates, links, and compatibility status are consistent.
- [ ] Claims are linked to primary or upstream sources where possible.
- [ ] Security and rollback implications are documented.
- [ ] Project and user procedures are distinct; agent prompts include files, contents, commands, verification, and rollback.
- [ ] Root guide indexes and compatibility tables are updated.
