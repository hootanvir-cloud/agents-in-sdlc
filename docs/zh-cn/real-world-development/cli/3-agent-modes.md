---
title: "练习 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 商定方案、使用 Autopilot 根据议题构建筛选功能，并使用 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能。但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作方式，确保以正确的方式构建正确的功能。本课将开启一系列课程，带您体验典型的智能体驱动开发流程：从议题出发生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能开发过程中会继续使用同一个对话和分支。通常，不同文件类型的工作会放在不同分支或 PR 中，但为帮助您聚焦核心概念，本研讨会会采用简化流程。

本课将：

- 根据 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式中明确需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续开发此功能时，您还会更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断增长，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了功能，但组合类别筛选等细节需要在编写代码前商定。您将使用 Plan 模式确定这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体，并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发人员遵循类似的流程：

1. 从描述待办事项的已创建议题开始。
2. 制定实现方案。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准方法，可以确保 AI 生成的代码满足既定需求，并接受与手写代码相同的审查流程。

## 对话模式

**对话模式**决定智能体可以自主执行多少操作。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式间切换：

- **Interactive**：您与智能体协作。智能体提出更改建议，并等待您的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试并不断迭代，直到完成任务。

先使用 Plan 模式，审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能前，请返回 Codespace，确保存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。使用 `#` 可以将 GitHub 上的议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划可以让您在交给 Copilot 前确定实现功能或执行任务的方式。对于复杂任务，花些时间规划总是有益的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划！它会先探索项目，然后确定最佳方案。

3. Copilot 可能会询问筛选功能应如何工作。根据您的偏好回答即可，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划的任何部分与您的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

计划完成并经过审查后，就可以开始实现了！使用 Autopilot 让 Copilot 执行工作。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或您所用版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为完成。

## 审查并验证实现

生成代码后，应像其他代码一样，在合并前进行审查。接下来检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并检查筛选实现和测试。
3. 将结果与议题及规划时的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决失败项。

## 探索新功能

代码看起来不错，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及按计划中商定的方式组合筛选条件。
4. 确认重置和无结果时的行为符合议题和您的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

您使用不同的对话模式构建并审查了功能。在本课中，您：

- 根据 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式中明确需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

接下来通过[使用自定义说明][next-lesson]深入了解代码生成，并确保代码遵循已记录的实践规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
