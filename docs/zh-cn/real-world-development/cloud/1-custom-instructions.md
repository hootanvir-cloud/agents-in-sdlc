---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解某些背景信息，就要确保它能够获取这些上下文。[说明文件][instruction-files]可以提供这类指导，让 Copilot 不仅理解您想让它做什么，也理解您希望它如何完成。

在本练习中，您将：

- 了解如何通过存储库自定义说明和路径范围说明文件，将项目特定上下文、编码指南和文档标准传递给 Copilot；
- 向 `.github/copilot-instructions.md` 添加一项新的全存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 实践路径不同，本练习不会运行“前后对比”提示。Copilot 云智能体会异步处理 GitHub 议题，因此很难实时并排演示其影响。稍后的课程中，您审查云智能体生成的拉取请求时，会看到说明文件如何影响结果。

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

完整讨论如何创建说明文件超出了本研讨会的范围。不过，示例项目展示了一种典型做法。概括来说：

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

## 添加新的存储库标准

接下来是本课唯一需要编辑的部分：添加一项项目级规则，要求在代码中使用 TSDoc 文档注释，并在文件顶部添加注释头。之后的练习中，云智能体处理您分配的议题时会读取这项规则。

编辑前先创建一个分支（云智能体会读取议题目标分支上的说明，但单独提交更改仍然是良好做法）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库编码标准补充规则，以提高代码建议的质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该在第 27 行附近。注意其中记录了项目编码标准，但尚未规定代码内文档要求，这就是之前生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件既可以在存储库级别创建（`.github/copilot-instructions.md`），用于全局指导；也可以创建为 `*.instructions.md` 文件，用于特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一练习中，您会将议题分配给 Copilot，而 Copilot 会从 `main` 创建分支。因此，说明更改必须先合并到 `main`，云智能体才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样，云智能体在下一练习中处理议题时就会读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过创建分支，直接提交到 `main`。创建分支是为了让课程流程贴近实际项目中的工作方式。

## 总结和后续步骤

您了解了 Copilot 如何从项目中的说明文件获取上下文，并向 `.github/copilot-instructions.md` 添加了一项新的全存储库标准。在本实践路径的后续课程中，您会看到该标准如何影响云智能体创建的拉取请求。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
