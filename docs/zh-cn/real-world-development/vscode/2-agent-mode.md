---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发人员，您的工作流程通常包括：找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次修改……诸如此类。

这时就可以使用 Copilot 智能体模式。

Copilot 智能体模式旨在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。接下来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 名称相似，但智能体模式和云智能体适用于两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与您协作。

在本练习中，您将了解：

- Copilot 智能体模式如何探索项目、识别相关文件并协调修改；
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能；
- 如何在将更改合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，您希望用户可以按类别和发行商筛选。上一练习中已经添加了 publishers helper，接下来使用 Copilot 智能体模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改之前，先浏览 Tailspin Toys 网站，了解当前功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台，允许用户查看游戏列表及其详细信息。这是一个 Astro 单体应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库——没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能则使用基于标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，可启动单个 Astro 开发服务器，方便您运行网站。请按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，您应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似下面的横幅和 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 的链接会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有通道；您的 Web 服务器现在通过它提供服务。

### 浏览网站

网站运行后，您可以探索其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后，会打开详情页，其中包含游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，先返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。每个练习的模式和模型选择可能不同，需要时会在步骤中说明。

网站的初始实现已经可以运行，但我们希望添加新功能。先查看待办事项。根据模板设置项目时，系统已自动创建一组议题；请让 GitHub Copilot 列出这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好任何特定模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此在说明中列出该模型，希望能让代码建议相对一致，带来良好体验。不过，大型语言模型具有概率性，您收到的建议可能与课程说明不同。这完全正常，也在预期之内。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，允许运行列出所有议题的命令。
5. 查看生成的议题列表。

您会看到 Copilot 根据它认为对用户最有用的项目，帮助您安排了优先级。

## 查看说明文件

让智能体生成代码之前，先查看将用于向 Copilot 提供上下文的说明文件。本练习将使用[用户界面 (UI) 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍方面的原则；
   - 指向各种文件类型专用说明文件的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件，也可以引用项目中的文件。路径相对于说明文件所在位置。这样可以复用内容，将复杂说明拆分为更易管理的小文件，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，应用程序至少需要进行三项独立更新：

- 在数据层中添加或完善剩余的筛选逻辑（`src/lib/`）；
- 为筛选行为添加或更新测试；
- 更新游戏列表页面，加入筛选 UI。

此外，在将更改合并到代码库之前，还必须运行并通过测试。Copilot 智能体模式可以完成这些任务！现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍然选择了 **Agent**。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍然选择了 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到与目标功能相关的文件。它应该会找到数据层 helper、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，这可能需要几分钟。过程中，代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项，因此不必在工作进行中选择。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板提示用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，甚至有测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且未检测到错误。

   ![Copilot 智能体模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 请记住，务必审查 Copilot 或任何 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。您了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能；
- Copilot 智能体模式如何探索项目、识别相关文件并协调修改；
- 如何在将更改合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 额外探索练习——实现分页

随着游戏列表不断增长，需要启用分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用已有的说明文件；
- 考虑分页的具体实现方式，例如是否允许用户选择每页条目数，还是使用固定值；
- 提示中应包含必要指导，以便 Copilot 按照您的期望实现功能；
- 可能需要与 GitHub Copilot 多轮迭代，提出更改要求并补充上下文。这是使用 Copilot 时的正常工作方式！

## 资源

- [Copilot 的 Ask、Edit 和 Agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
