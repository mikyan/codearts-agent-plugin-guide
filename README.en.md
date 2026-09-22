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
| Anthropic `frontend-design` | `0a64e39` | 26.8.1 | Works | [中文](guides/anthropic-skills/README.md) · [English](guides/anthropic-skills/README.en.md) |
| Addy Osmani `code-simplification` | `df1edb2` | 26.8.1 | Works | [中文](guides/addyosmani-agent-skills/README.md) · [English](guides/addyosmani-agent-skills/README.en.md) |
| Obsidian `obsidian-markdown` | `a1dc48e` | 26.8.1 | Works | [中文](guides/obsidian-skills/README.md) · [English](guides/obsidian-skills/README.en.md) |
| GitHub `commit-message-storyteller` | `318066d` | 26.8.1 | Works | [中文](guides/github-awesome-copilot/README.md) · [English](guides/github-awesome-copilot/README.en.md) |
| Humanizer | `ebf637b` | 26.8.1 | Adapter Required | [中文](guides/humanizer/README.md) · [English](guides/humanizer/README.en.md) |
| Scientific `experimental-design` | `9e8b0cb` | 26.8.1 | Partial | [中文](guides/scientific-agent-skills/README.md) · [English](guides/scientific-agent-skills/README.en.md) |
| Vercel `vercel-react-best-practices` | `b8caa26` | 26.8.1 | Works | [中文](guides/vercel-agent-skills/README.md) · [English](guides/vercel-agent-skills/README.en.md) |
| PM `prioritization-frameworks` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-skills/README.md) · [English](guides/pm-skills/README.en.md) |
| PM `prioritize-assumptions` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-prioritize-assumptions/README.md) · [English](guides/pm-prioritize-assumptions/README.en.md) |
| PM `opportunity-solution-tree` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-opportunity-solution-tree/README.md) · [English](guides/pm-opportunity-solution-tree/README.en.md) |
| PM `product-vision` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-product-vision/README.md) · [English](guides/pm-product-vision/README.en.md) |
| PM `value-proposition` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-value-proposition/README.md) · [English](guides/pm-value-proposition/README.en.md) |
| PM `lean-canvas` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-lean-canvas/README.md) · [English](guides/pm-lean-canvas/README.en.md) |
| PM `swot-analysis` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-swot-analysis/README.md) · [English](guides/pm-swot-analysis/README.en.md) |
| PM `porters-five-forces` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-porters-five-forces/README.md) · [English](guides/pm-porters-five-forces/README.en.md) |
| PM `stakeholder-map` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-stakeholder-map/README.md) · [English](guides/pm-stakeholder-map/README.en.md) |
| PM `user-stories` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-user-stories/README.md) · [English](guides/pm-user-stories/README.en.md) |
| PM `market-sizing` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-market-sizing/README.md) · [English](guides/pm-market-sizing/README.en.md) |
| PM `sql-queries` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-sql-queries/README.md) · [English](guides/pm-sql-queries/README.en.md) |
| PM `customer-journey-map` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-customer-journey-map/README.md) · [English](guides/pm-customer-journey-map/README.en.md) |
| PM `user-personas` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-user-personas/README.md) · [English](guides/pm-user-personas/README.en.md) |
| PM `north-star-metric` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-north-star-metric/README.md) · [English](guides/pm-north-star-metric/README.en.md) |
| PM `pricing-strategy` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-pricing-strategy/README.md) · [English](guides/pm-pricing-strategy/README.en.md) |
| PM `product-strategy` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-product-strategy/README.md) · [English](guides/pm-product-strategy/README.en.md) |
| OpenAI `security-best-practices` | `49f948f` | 26.8.1 | Works | [中文](guides/openai-skills/README.md) · [English](guides/openai-skills/README.en.md) |
| Google `gke-manifest-generation` | `65ef106` | 26.8.1 | Partial | [中文](guides/google-skills/README.md) · [English](guides/google-skills/README.en.md) |
| i-have-adhd | `e7555fc` | 26.8.1 | Works | [中文](guides/i-have-adhd/README.md) · [English](guides/i-have-adhd/README.en.md) |
| Agentic Awesome `ab-testing` | `e2b6ad1` | 26.8.1 | Works | [中文](guides/agentic-awesome-ab-testing/README.md) · [English](guides/agentic-awesome-ab-testing/README.en.md) |
| Agentic Awesome `ui-lint` | `bdfbf79` | 26.8.1 | Works | [中文](guides/agentic-awesome-ui-lint/README.md) · [English](guides/agentic-awesome-ui-lint/README.en.md) |
| Agentic Awesome `read-all-adrs` | `bdfbf79` | 26.8.1 | Works | [中文](guides/agentic-awesome-read-all-adrs/README.md) · [English](guides/agentic-awesome-read-all-adrs/README.en.md) |
| Agentic Awesome `effective-agent-skills` | `bdfbf79` | 26.8.1 | Works | [中文](guides/agentic-awesome-effective-agent-skills/README.md) · [English](guides/agentic-awesome-effective-agent-skills/README.en.md) |
| Agentic Awesome `documentation` | `2fce708` | 26.8.1 | Works | [中文](guides/agentic-awesome-documentation/README.md) · [English](guides/agentic-awesome-documentation/README.en.md) |
| Agentic Awesome `code-review-excellence` | `2fce708` | 26.8.1 | Works | [中文](guides/agentic-awesome-code-review-excellence/README.md) · [English](guides/agentic-awesome-code-review-excellence/README.en.md) |
| Agentic Awesome `debugging-strategies` | `2fce708` | 26.8.1 | Works | [中文](guides/agentic-awesome-debugging-strategies/README.md) · [English](guides/agentic-awesome-debugging-strategies/README.en.md) |
| Agentic Awesome `testing-patterns` | `2fce708` | 26.8.1 | Works | [中文](guides/agentic-awesome-testing-patterns/README.md) · [English](guides/agentic-awesome-testing-patterns/README.en.md) |
| Agentic Awesome `api-analyzer` | `69906dd` | 26.8.1 | Works | [中文](guides/agentic-awesome-api-analyzer/README.md) · [English](guides/agentic-awesome-api-analyzer/README.en.md) |
| Agentic Awesome `cross-platform-contract-propagation-audit` | `69906dd` | 26.8.1 | Works | [中文](guides/agentic-awesome-contract-propagation-audit/README.md) · [English](guides/agentic-awesome-contract-propagation-audit/README.en.md) |
| Agentic Awesome `decision-navigator` | `69906dd` | 26.8.1 | Works | [中文](guides/agentic-awesome-decision-navigator/README.md) · [English](guides/agentic-awesome-decision-navigator/README.en.md) |
| Agentic Awesome `anti-deception` | `69906dd` | 26.8.1 | Works | [中文](guides/agentic-awesome-anti-deception/README.md) · [English](guides/agentic-awesome-anti-deception/README.en.md) |
| Agentic Awesome `data-storytelling` | `69906dd` | 26.8.1 | Works | [中文](guides/agentic-awesome-data-storytelling/README.md) · [English](guides/agentic-awesome-data-storytelling/README.en.md) |
| Agentic Awesome `database-optimizer` | `69906dd` | 26.8.1 | Works | [中文](guides/agentic-awesome-database-optimizer/README.md) · [English](guides/agentic-awesome-database-optimizer/README.en.md) |
| Agentic Awesome `error-detective` | `69906dd` | 26.8.1 | Works | [中文](guides/agentic-awesome-error-detective/README.md) · [English](guides/agentic-awesome-error-detective/README.en.md) |
| Wshobson `api-design-principles` | `367cb6a` | 26.8.1 | Works | [中文](guides/wshobson-api-design-principles/README.md) · [English](guides/wshobson-api-design-principles/README.en.md) |
| Claude Skills `team-communications` | `aa8d778` | 26.8.1 | Works | [中文](guides/claude-skills-team-communications/README.md) · [English](guides/claude-skills-team-communications/README.en.md) |
| Khazix Writer | `7a5c493` | 26.8.1 | Works | [中文](guides/khazix-writer/README.md) · [English](guides/khazix-writer/README.en.md) |
| Trail of Bits `guidelines-advisor` | `9b28133` | 26.8.1 | Works | [中文](guides/trailofbits-guidelines-advisor/README.md) · [English](guides/trailofbits-guidelines-advisor/README.en.md) |
| Matt Pocock `grilling` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-grilling/README.md) · [English](guides/mattpocock-grilling/README.en.md) |
| Matt Pocock `grill-me` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-grill-me/README.md) · [English](guides/mattpocock-grill-me/README.en.md) |
| Matt Pocock `wait-what` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-wait-what/README.md) · [English](guides/mattpocock-wait-what/README.en.md) |
| Matt Pocock `handoff` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-handoff/README.md) · [English](guides/mattpocock-handoff/README.en.md) |
| Matt Pocock `codebase-design` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-codebase-design/README.md) · [English](guides/mattpocock-codebase-design/README.en.md) |
| Matt Pocock `tdd` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-tdd/README.md) · [English](guides/mattpocock-tdd/README.en.md) |
| Matt Pocock `diagnosing-bugs` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-diagnosing-bugs/README.md) · [English](guides/mattpocock-diagnosing-bugs/README.en.md) |
| Matt Pocock `ask-matt` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-ask-matt/README.md) · [English](guides/mattpocock-ask-matt/README.en.md) |
| Matt Pocock `code-review` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-code-review/README.md) · [English](guides/mattpocock-code-review/README.en.md) |
| Matt Pocock `resolving-merge-conflicts` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-resolving-merge-conflicts/README.md) · [English](guides/mattpocock-resolving-merge-conflicts/README.en.md) |
| Matt Pocock `to-spec` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-to-spec/README.md) · [English](guides/mattpocock-to-spec/README.en.md) |
| Matt Pocock `to-tickets` | `6654f6b` | 26.8.1 | Works | [中文](guides/mattpocock-to-tickets/README.md) · [English](guides/mattpocock-to-tickets/README.en.md) |
| PM `product-name` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-product-name/README.md) · [English](guides/pm-product-name/README.en.md) |
| PM `dummy-dataset` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-dummy-dataset/README.md) · [English](guides/pm-dummy-dataset/README.en.md) |
| PM `summarize-interview` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-summarize-interview/README.md) · [English](guides/pm-summarize-interview/README.en.md) |
| PM `intended-vs-implemented` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-intended-vs-implemented/README.md) · [English](guides/pm-intended-vs-implemented/README.en.md) |
| PM `summarize-meeting` | `18468a9` | 26.8.1 | Works | [中文](guides/pm-summarize-meeting/README.md) · [English](guides/pm-summarize-meeting/README.en.md) |

All sixty-five formal guide entries were installed, invoked, and rolled back in real CodeArts CLI sessions. Project results were reproduced in a second isolated project; user results were verified from an independent directory without a same-named project skill. Candidates below the guide threshold remain documented in research. The latest evidence is in the [2026-09-22 Agentic Awesome v18.1.0 ten-candidate verification](research/2026-09-22.en.md); earlier records are under [research](research/).

## Reusable verification skill

The repository includes a [CodeArts plugin compatibility skill](skills/codearts-plugin-compatibility/SKILL.md). It captures the verified native-skill, minimal-wrapper, dynamic-path bridge, isolated user dependency, two-project reproduction, and exact rollback patterns. Future candidates should test these paths first and deviate only when evidence requires another adapter.

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
skills/
  <skill>/           # Reusable repository testing and publishing workflows
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
