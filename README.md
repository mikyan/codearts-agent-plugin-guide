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

两个项目都已通过自定义 MiMo 模型在真实 CodeArts CLI 会话中完成项目级和个人级安装、调用与回滚。项目级结果在第二个隔离项目中复现，个人级结果从没有项目配置的独立目录验证。证据见 [2026-08-18 项目级记录](research/2026-08-18.md)和 [2026-08-19 个人级记录](research/2026-08-19.md)。

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
