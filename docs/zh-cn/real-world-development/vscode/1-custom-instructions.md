---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解某些背景信息，就要确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种是[说明文件][instruction-files]，可用于指导代码生成。

在本练习中，您将：

- 了解如何通过存储库自定义说明和路径范围说明文件，将项目特定上下文、编码指南和文档标准传递给 Copilot；
- 在当前说明生效的情况下，为筛选功能生成第一块数据切片（publishers helper）；
- 向 `.github/copilot-instructions.md` 添加一项新的全存储库标准；
- 再次运行相同提示，观察生成的代码如何采用新标准；
- 将说明更新和筛选功能基础提交到 `main`，供下一练习中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能与部分标准不完全一致。Copilot 具有概率性。本练习的目的是观察更新说明后行为变化的*趋势*，而不是要求输出逐字符匹配。

## 说明文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也制定了开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式并具有现代感。
- 应以 TSDoc 文档注释的形式为代码编写文档。
- 每个文件顶部都应添加一段注释，说明文件的用途。

使用说明文件可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它更好地理解您的编码风格和需求。这项功能可以引导 Copilot 提供更相关的建议和代码片段。您可以指定偏好的编码约定、库，甚至希望包含的注释类型。既可以为整个存储库创建说明，也可以针对特定文件类型提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：这是一个针对存储库中**每个**请求都会发送给 Copilot 的说明文件。它应包含项目级信息，也就是大多数 Copilot Chat 或 CLI 请求都会用到的上下文，例如技术栈、项目概览、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可以针对特定任务或文件类型创建这些文件，用于指导特定语言（如 TypeScript 或 Astro），或 UI 组件、单元测试等任务。

> [!NOTE]
> 在 IDE 中工作时，说明文件只用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他规范的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本练习的范围。不过，示例项目展示了一种典型做法。概括来说：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体指导。
- 使用自然语言，让指导清晰易懂，并提供代码应当和不应当如何编写的示例。

创建说明文件没有唯一正确的方法，使用 AI 也一样。通过不断尝试，您会找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。查看本项目中的文件时，您会发现它们涵盖多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口各不相同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`），当前界面的相关课程会在适当位置说明。
>
> 想找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中汇集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件——其中有一个核心 `copilot-instructions.md`，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览该文件，留意项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，还要留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于您与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览。您会看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，用于决定说明适用的文件。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 查看为本项目编写单元测试的专门说明。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明拆分成更小、可复用的文件，并为 Copilot 提供生成代码时可参考的示例。（这些路径相对于说明文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前还未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

遵循最佳实践，为更改创建一个分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明*之前*使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，先返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。每个练习的模式和模型选择可能不同，需要时会在步骤中说明。

要了解自定义说明的影响，先在当前说明文件生效的情况下发送提示。稍后更新说明后，再发送相同提示进行比较。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只获取您需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改 helper 文件和测试。
6. 留意建议的 helper 是一个带类型的函数，第一个参数接收 `db` 客户端，并返回带类型的 publishers 数组——这是 `.github/instructions/drizzle.instructions.md`（适用于 `src/lib/*.ts`）中的数据层约定。
7. 注意，建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，它也可能添加文档注释。即便如此也没关系；这里要观察的是更新说明后*一致性*的提升。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库编码标准补充规则，以提高代码建议的质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该在第 27 行附近。注意其中记录了项目编码标准，但尚未规定代码内文档要求，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件既可以在存储库级别创建（`.github/copilot-instructions.md`），用于全局指导；也可以创建为 `*.instructions.md` 文件，用于特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空上下文。
2. 再次选中 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议文件现在会以类似下面的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议函数现在会包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

您刚刚引导 Copilot 遵循新的项目标准，并将其应用到实际代码中；下一练习将以此为基础继续开展。

## 提交说明并推送分支

说明文件与存储库中的其他文件一样，使用相同的源代码管理流程。现在提交更改并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和 helper 更改都已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新，以及 Copilot 为 helper 基础更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，应创建拉取请求，并将其关联到待办事项中的文档标准议题。也可以手动操作，不过 Copilot 可以使用已连接到项目的 GitHub 工具代劳。让 Copilot 找到议题、创建用于关闭该议题的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表您在 GitHub 上执行操作。项目模板已在工作区中配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示您登录 GitHub。按照提示授权即可。稍后的练习会介绍这种连接方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选中了 **Agent**。
3. 请 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中既包含说明更新，也包含新的 publishers helper 基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 当系统提示您允许 Copilot 代表您执行 GitHub 操作时，请检查命令，并根据需要选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并在本地切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当系统提示您允许 Copilot 代表您执行 GitHub 操作和 shell 命令时，请检查命令，并根据需要选择 **Allow**。

现在，您已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

您了解了 Copilot 如何从项目中的说明文件获取上下文，然后在 VS Code 中使用 Copilot Chat 完成了以下事项：

- 发送代码生成提示，观察 Copilot 根据*现有*说明生成的内容；
- 向 `.github/copilot-instructions.md` 添加一项新的全存储库标准；
- 再次运行相同提示，观察建议的代码如何采用新标准；
- 将说明更新和 helper 基础提交到 `main`，供下一练习使用。

接下来，您将使用[智能体模式][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
