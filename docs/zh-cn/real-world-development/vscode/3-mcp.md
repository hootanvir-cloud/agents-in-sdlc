---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

您刚刚使用智能体模式构建了筛选功能。在创建拉取请求之前，应先确认功能在浏览器中正常运行。您无需亲自逐步操作应用，而是连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

在本练习中，您将：

- 了解 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 请 Copilot 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式能够访问编辑器以外的资源时，功能会强大得多。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务交互。它是一种让智能体与外部工具通信的标准。

![智能体模式内部工作原理示意图，展示其如何与上下文、LLM 和工具交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问外部工具和服务，从而通过资源获取最新信息，并使用工具代表您执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理二者之间的通信，例如连接现有 API 或 NPM 包等本地工具。每个 MCP 服务器都代表 AI 智能体可以访问的一组工具和资源。

以下是一些常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以用它创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。AI 智能体可以用它打开网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可以访问不同工具和资源。GitHub 还托管了 [MCP 注册表](https://github.com/mcp)，以便更好地发现生态系统中的项目并参与贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样审慎对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区中可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 应该会看到已配置的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

此 `github` 条目随项目模板提供，因此 Copilot 在上一练习中可以读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容——VS Code 通过 HTTP 连接服务器，并在 Copilot 首次使用其工具时让您通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试新功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会通过运行 `npx @playwright/mcp@latest` 启动它。`--headless` 参数会让 Playwright 在没有可见窗口的情况下运行浏览器。这是 Codespace 环境的必要条件，因为其中没有可以显示窗口的桌面。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 运行端到端测试，因此 Playwright 所需的浏览器通常已经安装。如果 Copilot 稍后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器——Copilot 首次需要服务器的工具时，VS Code 会启动服务器，并询问您是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问您是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动服务器是可选的。如果跳过，VS Code 会在下一步中 Copilot 首次需要其工具时启动服务器，并提示您信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。智能体可以打开页面、导航、应用筛选并读取结果，然后向您总结观察到的情况，因此您无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着智能体会像辅助技术一样，基于结构化且带标签的元素（按钮、链接、列表项）进行推理。因此，快速的功能检查也可以顺便检查基本的无障碍情况。

连接服务器并运行应用后，请 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步执行操作并报告结果。请根据议题中的验收标准检查报告。如果发现问题，可以继续提问，或让 Copilot 在创建拉取请求前修复代码。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 运行。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果它报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 打开 Codespace 中的 **Source Control** 面板，并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示所做更改的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，例如使用 github.com 或 GitHub 命令行界面 (CLI)。不过，既然您已经在使用 GitHub Copilot，就让它代为创建 PR 吧！它可以找到相关议题并创建与议题关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在智能体下拉菜单中选择了 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会提示您通过 GitHub 身份验证；请按照提示授权。
5. 注意 Copilot 如何搜索议题、找到正确的议题并创建 PR。
6. 选择 Copilot 提供的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！在本练习中，您：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到了工作区；
- 使用它在发布功能前通过浏览器手动测试筛选功能；
- 发布了分支，并为筛选工作创建了拉取请求。

您使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来创建一个[自定义智能体][next-lesson]，简化无障碍审查等聚焦任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
