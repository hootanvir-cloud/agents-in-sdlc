---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因一直没有时间实现而长期搁置的功能请求。GitHub Copilot 云智能体可以自主完成更新代码和添加功能等任务。工作完成后，它会创建草稿 PR，供开发人员审查。这样可以委派繁琐任务、加快开发流程，让开发人员专注于更重要的工作。

本课将探索 Copilot 云智能体如何：

- 自定义代码生成环境；
- 确保操作安全；
- 处理范围明确的议题；
- 接收分配的议题。

## 场景

Tailspin Toys 有一些希望处理的技术债务。最初负责创建网站的承包商没有妥善编写文档——事实上，您会发现代码完全缺少文档。团队希望先为应用程序中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望游戏详情页显示相关游戏，也就是同一类别中的其他游戏，方便访客继续浏览。目前不需要精致的实现，只要可以用于用户体验验收测试即可。该工作目前是阻碍项，但还有其他优先级更高的议题。

这些都是容易被降低优先级的任务，非常适合分配给 Copilot 云智能体。它可以在后台异步处理，开发人员则可以先做其他工作，之后再回来审查 Copilot 的结果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发人员一样在后台执行任务。与人类协作一样，您可以通过多种方式分配工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求跟踪进度、设置环境并开始处理任务。您可以在会话进行期间或完成后查看 Copilot 的工作。解决方案准备好审查后，Copilot 会在拉取请求中通知您。

## 清晰界定任务范围的重要性

GitHub Copilot 并没有什么魔法——您仍然需要思考。即使看似简单的操作，深入分析后也可能很复杂，因此请注意[为 Copilot 云智能体明确任务范围][cloud-agent-best-practices]。把它视为 AI 配对程序员：分阶段开展工作、学习、尝试并不断调整。引入生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果您已经完成自定义说明实践，就已经看过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。将工作分配给 Copilot 云智能体前，请先只读查看存储库中的说明文件，稍后便能辨认其影响。

在 GitHub Web UI 中打开存储库中的以下文件；如果已有 Codespace 在运行，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将说明范围限定为 `**/*.test.ts` 文件。

下一部分会将 `Code lacks documentation` 议题分配给云智能体。审查结果 PR 时，留意这些说明文件带来的 TSDoc 文档注释、TypeScript 约定和文件注释头。稍后的课程还会再次指出这些内容。

## 为 Copilot 云智能体设置开发环境

编写代码通常需要特定环境并运行一些设置脚本，确保项目状态正常。将任务分配给 Copilot 时也是如此，它会像软件工程师一样完成工作。

云智能体使用 [GitHub Actions][github-actions] 提供工作环境。您可以创建一个[专用设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中配置，让 Copilot 开始工作前先执行，以便访问所需的开发工具和依赖项。为帮助实验流程并提供学习机会，项目已预先配置此工作流。它会为当前单一 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许您在存储库的 Actions 选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅授予这些步骤所需的最低权限。Copilot 会获得用于自身操作的独立令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），需要 contents: read 权限。如果不克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端 / 应用设置 - Node.js（整个应用现在由 Astro + Drizzle/libSQL 构成）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，确保构建和测试有数据可用。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许您在存储库的 **Actions** 选项卡中手动运行工作流。这样可以在等待 Copilot 运行之前测试工作流是否成功。

## 添加文档

大家都知道文档的重要性，但多数项目的信息已经过时或完全缺失。这类技术债务往往长期得不到处理，会降低效率，增加维护代码库和帮助新开发人员入门的难度。Copilot 擅长生成文档，因此这是非常适合分配给 Copilot 云智能体的议题。它会在后台生成所需文档，后续练习中您可以回来审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚生成的拉取请求 (PR)，其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等一两分钟，然后刷新浏览器。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 生成待办事项列表可能需要几分钟。Copilot 正在创建环境（运行前面提到的工作流）、分析项目并确定最佳处理方案。

13. 查看待办事项及计划完成的任务。
14. 向下滚动拉取请求时间线，应该会看到 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码和了解当前状态、暂停思考并确定合适方案，以及生成代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们可以专注于其他工作。现在利用这一点：再给 Copilot 云智能体分配一个任务，同时开始为应用编写代码、添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：您专注于一组任务，它则处理另一组任务。为设计团队添加相关游戏部分也许不需要太久，但这段时间仍可用于其他工作。现在将任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，您会在议题的第一条评论中看到一组 👀，表示 Copilot 正在处理任务！

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二项请求！Copilot 云智能体的工作方式类似软件工程师，因此您无需主动监控；任务完成后再审查即可。现在开始创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。您可以将议题分配给云智能体，让它异步处理技术债务、创建新功能，或帮助将代码从一个框架迁移到另一个框架。

您探索了以下概念：

- 自定义代码生成环境；
- 确保操作安全；
- 清晰界定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台工作，接下来可以开始创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以访问自定义说明，您已在前面的模块中了解过。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
