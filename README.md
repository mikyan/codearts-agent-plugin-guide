# CodeArts Agent Plugin Guide

> How to use it on CodeArts?

[简体中文](README.zh-CN.md)

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

No compatibility guides have been published yet. The first researched candidates will appear under [`guides/`](guides/).

## Repository layout

```text
guides/<project>/
  README.md          # English guide
  README.zh-CN.md    # Chinese guide
research/
  YYYY-MM-DD.md      # Candidate research and evidence log
adapters/
  <project>/         # Compatibility code only when genuinely required
```

## Research principles

- Prefer active, clearly licensed upstream projects with verifiable community signals.
- Record source URLs, upstream versions, CodeArts versions, operating systems, and verification dates.
- Inspect permissions, install scripts, downloaded binaries, external services, and credential requirements.
- Never modify a user's global agent configuration as part of an unattended test.
- Keep the English and Chinese versions aligned on status, versions, limitations, and links.

## Contributing

Candidate suggestions, verification reports, corrections, and new guides are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a contribution.

## License

A project license will be selected before accepting third-party contributions. Upstream projects retain their own licenses; every guide must identify the relevant upstream license.
