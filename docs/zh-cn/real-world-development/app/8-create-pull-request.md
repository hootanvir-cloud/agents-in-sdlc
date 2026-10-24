---
title: "练习 8 - 创建并合并功能 PR"
description: "一并审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、说明更新、技能更新、质量保证 (QA) 配置和测试都保存在同一个分支上。现在一起审查这些内容并创建拉取请求。星级评分的拉取请求 (PR) 是您手动合并的；这次将使用 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分成几个独立 PR。为简化研讨会流程，筛选和质量工作流一直在同一个会话和分支中进行，所有更改都会纳入此 PR。

在本课中，您将：

- 了解 Agent Merge 是什么，以及它如何自动化合并生命周期；
- 检查完整的功能 PR 和验证证据；
- 审查后再授权 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能的开发过程中，您使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动处理剩余的 PR 工作，同时让开发人员掌握合并授权权。

## 介绍 Agent Merge

**Agent Merge** 会自动完成通过 GitHub Copilot app 合并拉取请求所需的剩余工作。启用后，应用会话会读取拉取请求并处理阻碍项——修复失败的持续集成 (CI) 检查、回应审查意见，并在需要时执行变基——之后在 GitHub 允许时合并。该功能会在后台运行，应用重启后仍会继续，并在拉取请求合并后自动关闭。

到目前为止，您都亲自选择 **Merge pull request**。现在可以将这项工作交给 Agent Merge，但它编辑代码和合并的能力仍需要您明确授权。授予合并权限前，请审查它可执行的操作和已有工作。

## 使用 Agent Merge 管理 PR

代码全部完成并审查后，让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会更改为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮启动流程。仅从下拉菜单选择该选项并不会启动流程。

Agent Merge 流程随即启动。它会：

- 创建包含标题和说明的拉取请求；
- 如果会话从议题启动，则在说明正文中引用相关议题；
- 对目标分支执行变基或处理可能的合并冲突；
- 监控 CI 流程，确保所有检查通过；
- 监控 PR 中其他开发人员或 Copilot 代码审查提供的反馈，并更新代码以解决意见；
- 可选：在所有检查通过后自动合并 PR。

现在让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 跟踪处理进度，并解决报告的阻碍项。继续之前，请确认 PR 状态为 **Merged**；已启用合并权限或智能体正在运行，都不能证明合并已完成。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或缺失的权限。解决这些阻碍后再继续。

## 总结和后续步骤

您已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。在本课中，您：

- 了解了 Agent Merge 及其如何自动化合并生命周期；
- 检查了完整的功能 PR 和验证证据；
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来[使用现有画布并创建分类画布][next-lesson]，探索与智能体检查、规划和可视化工作的更多方式。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
