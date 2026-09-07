# CodeArts Agent 社区插件兼容指南

> 如何在 CodeArts 中使用？

[English](README.en.md)

这是一个由社区维护的兼容性研究与实用指南项目，帮助用户在华为云 CodeArts Agent（码道）中使用热门的 Agent 插件、Skills、Hooks、Commands 和 MCP 集成。

## 这个仓库解决什么问题

本项目把上游项目的安装说明转化为针对 CodeArts、可复现的操作指南。每篇指南都应回答四个问题：

1. 上游项目提供什么能力？
2. 它能否在 CodeArts Agent 中运行？
3. 用户如何安全地安装、验证、排障和卸载？
4. 实际检查过哪些上游版本和 CodeArts 版本？

所有兼容性结论都必须有证据。指南必须区分静态分析和实际验证，不能把推测描述成测试成功。

## 兼容状态

| 状态 | 含义 |
| --- | --- |
| Native | 上游项目明确支持 CodeArts Agent。 |
| Works | 按文档安装即可使用，无需适配器。 |
| Partial | 核心能力可用，但存在已记录的限制。 |
| Adapter Required | 必须使用持续维护的兼容层。 |
| Not Working | 已知的不兼容问题导致当前无法有效使用。 |
| Untested | 已完成研究，但尚未进行安全的实际验证。 |

## 指南索引

| 项目 | 上游版本 | CodeArts CLI | 状态 | 指南 |
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

四十六个正式指南条目都已通过自定义 MiMo 模型在真实 CodeArts CLI 会话中完成安装、调用与回滚。项目级结果在第二个隔离项目中复现，个人级结果从没有项目级同名 Skill 的独立目录验证。未达指南门槛的候选仍记录在 research。最新证据见 [2026-09-07 PM Skills 十项验证记录](research/2026-09-07.md)，早期记录见 [research](research/)。

## 可复用验证 Skill

仓库提供 [CodeArts 插件兼容验证 Skill](skills/codearts-plugin-compatibility/SKILL.md)，把本轮确认的原生 Skills 安装、最小插件包装、动态路径转原生目录、个人级依赖隔离、双项目复现和精确回滚方法固化下来。后续候选应先按这套路径验证；只有实际证据不支持时再采用其他适配方式。

## 仓库结构

```text
guides/<project>/
  README.md          # 默认中文指南
  README.en.md       # 英文指南
research/
  YYYY-MM-DD.md      # 默认中文研究与证据记录
  YYYY-MM-DD.en.md   # 英文记录
adapters/
  <project>/         # 仅在确实需要兼容代码时添加
skills/
  <skill>/           # 仓库自身可复用的测试与发布流程
```

## 研究原则

- 优先研究仍在活跃维护、许可证清晰且社区热度可验证的上游项目。
- 记录来源链接、上游版本、CodeArts 版本、操作系统和验证日期。
- 检查权限、安装脚本、下载的二进制文件、外部服务和凭据要求。
- 默认在隔离项目中验证；需要验证个人级安装时，必须避开现有用户配置、使用可精确回滚的独立依赖目录，并在无项目覆盖的目录复测。
- 中英文版本中的状态、版本、限制和链接必须保持一致。

## 参与贡献

欢迎提交候选项目建议、验证报告、内容修正和新指南。贡献前请阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 许可证

项目将在接受第三方贡献前确定许可证。上游项目保留各自的许可证；每篇指南必须标明相关上游许可证。
