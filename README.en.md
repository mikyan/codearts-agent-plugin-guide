# CodeArts Agent Plugin Guide

> How to use it on CodeArts?

[简体中文](README.md)

Community-maintained compatibility research and practical guides for running popular agent plugins, skills, hooks, commands, and MCP integrations on Huawei Cloud CodeArts Agent.

## What this repository does

This project turns upstream installation instructions into CodeArts-specific, reproducible guidance. Each guide aims to answer four questions:

1. What does the upstream project provide?
2. Can it run on CodeArts Agent?
3. How can users install, verify, troubleshoot, and remove it safely?
4. Which upstream and CodeArts versions were actually checked?

Compatibility claims are evidence-based. A guide must distinguish static analysis from hands-on verification and must never present an assumption as a successful test.

## Compatibility status

| Status | Meaning |
| --- | --- |
| Native | The upstream project explicitly supports CodeArts Agent. |
| Works | The documented upstream installation works without an adapter. |
| Partial | Core capabilities work, with documented limitations. |
| Adapter Required | A maintained compatibility layer is required. |
| Not Working | A known incompatibility currently prevents useful operation. |
| Untested | Research exists, but safe hands-on verification is still pending. |

## Guides

| Project | Upstream version | CodeArts CLI | Status | Guide |
| --- | --- | --- | --- | --- |
| Ponytail | 4.9.0 | 26.8.1 | Adapter Required | [中文](guides/ponytail/README.md) · [English](guides/ponytail/README.en.md) |
| Superpowers | 6.3.0 | 26.8.1 | Adapter Required | [中文](guides/superpowers/README.md) · [English](guides/superpowers/README.en.md) |

Both entries were installed, invoked, and rolled back in real CodeArts CLI sessions at project and user scope. Project results were reproduced in a second isolated project; user results were verified from an independent directory without project configuration. See the [2026-08-18 project log](research/2026-08-18.en.md) and [2026-08-19 user log](research/2026-08-19.en.md).

## Repository layout

```text
guides/<project>/
  README.md          # Default Chinese guide
  README.en.md       # English guide
research/
  YYYY-MM-DD.md      # Default Chinese research and evidence log
  YYYY-MM-DD.en.md   # English log
adapters/
  <project>/         # Compatibility code only when genuinely required
```

## Research principles

- Prefer active, clearly licensed upstream projects with verifiable community signals.
- Record source URLs, upstream versions, CodeArts versions, operating systems, and verification dates.
- Inspect permissions, install scripts, downloaded binaries, external services, and credential requirements.
- Verify in isolated projects by default. User-scope tests must avoid existing user configuration, use independently removable dependency directories, and be repeated from a directory with no project override.
- Keep the English and Chinese versions aligned on status, versions, limitations, and links.

## Contributing

Candidate suggestions, verification reports, corrections, and new guides are welcome. Read [CONTRIBUTING.en.md](CONTRIBUTING.en.md) before opening a contribution.

## License

A project license will be selected before accepting third-party contributions. Upstream projects retain their own licenses; every guide must identify the relevant upstream license.
