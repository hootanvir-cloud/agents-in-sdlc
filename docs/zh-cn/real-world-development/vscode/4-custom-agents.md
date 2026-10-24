---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]可以创建专门针对开发工作流中某些任务或领域的 AI 助手。您可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义智能体，并为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，从而更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如确保符合 [WCAG][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少有 `name` 和 `description`，后面是定义智能体行为、专业知识和说明的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体与[智能体技能][agent-skills-concept]在概念上有一些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行任务的工作者，技能是它使用的工具。**

自定义智能体有自己的上下文窗口，并且可以协调技能（甚至其他智能体）完成工作。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与其他 AI 应用一样，请通过测试和迭代找出适合您的环境和场景的做法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文字和背景颜色对比度不足，有些用户难以阅读当前深色主题。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍至关重要，因此希望尽快实现该功能。您将使用自定义智能体生成所需功能。

在本练习中，您将：

- 审查现有的无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

项目已经为无障碍工作准备了一个自定义智能体。先审查其内容，了解它将如何指导 Copilot。

返回 Codespace，然后打开终端，从 `main` 创建一个用于无障碍工作的新分支（将练习 3 的筛选 PR 保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义智能体的必需项。

3. 接下来浏览并审查以下部分：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。您可以选择某个自定义智能体，让聊天会话遵循该智能体的说明和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，先返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。每个练习的模式和模型选择可能不同，需要时会在步骤中说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（其中会显示当前智能体，默认值为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会开始工作：探索代码库、提出更改方案并编辑项目文件。每项编辑都会在聊天中以内联方式显示文件路径和差异，供您审查。
6. 智能体修改文件时，**Files changed** 指示器会更新，显示已修改的文件。

> [!NOTE]
> 这可能需要几分钟。Copilot 会修改存储库中的真实文件，例如 Astro 组件、CSS 和相关测试。

下一课会审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。通过自定义智能体，您可以将团队的专业知识和标准整理成可复用的智能体，更有效地指导 Copilot 完成工作。

您了解了：

- 自定义智能体的定义方式；
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来[监控并引导智能体的工作][next-lesson]，审查更改并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
