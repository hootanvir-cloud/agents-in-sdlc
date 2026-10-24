---
title: "练习 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义配置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

您使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。您已经：

- 准备了 Codespace、安装 Copilot CLI、探索项目，并找到预置的筛选议题。
- 添加星级评分、在转发的浏览器中审查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，再在 Interactive 模式中审查。
- 使用自定义说明指导智能体，然后自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 里程碑，每个 PR 都从更新后的 `main` 分支创建独立分支：

1. **星级评分：** 在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流：** 实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，您一直使用同一个会话和分支。为了让研讨会更精简，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

您通过多种方式检查代码：运行自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 的浏览器探索功能。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 智能体在 PR 提交前结合这些结果，审查需求和测试覆盖率。

新增测试应能弥补实际缺口；QA 运行后不需要新增测试也可能是合理的。授权合并前，请审查代码和证据；更改后请刷新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作方式。在本研讨会中，您更新了说明、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在不同会话中复用这些自定义设置，并根据团队需求进行调整。说明用于设定标准，技能描述可重复执行的任务，自定义智能体定义专家角色，MCP 服务器连接外部工具。审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前考虑方案，使用 **Interactive** 参与聚焦的更改过程，对范围明确的任务使用 **Autopilot**。常规编辑可选择速度较快的模型，复杂工作则选择能力更强的模型。

上下文和基础设施同样重要。清楚描述想构建的**内容**、**原因**和**方式**，会显著影响输出。

## 深入探索

您已经完成核心工作流。还可以了解一些 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获取另一种观点。
- 使用 `/fleet` 并行协调相互独立的子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

提升任何工具使用能力的最佳方式就是持续使用它！可以在生产代码、业余项目，或那个想了很多年却一直没动手的小应用中使用它。与团队分享经验，也向团队成员学习。还可以继续探索文档。

如果想在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。若想比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
