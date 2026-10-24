---
title: "练习 4 - 使用自定义说明指导 Copilot"
description: "探索存储库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相应指导。[说明文件][instruction-files]不仅描述希望生成什么代码，还描述代码应如何组织。完成筛选功能后，您将查看 Copilot 使用的说明、添加文档标准并将其应用于代码。

在本课中，您将：

- 了解存储库说明和路径范围说明文件如何传递给智能体；
- 更新说明文件，确保遵循编码标准；
- 观察说明文件对代码的影响。

## 场景

和其他优秀的开发团队一样，Tailspin Toys 也制定了开发实践指南和要求，包括：

- 注释应说明意图和不明显的决策，而不是重复代码内容。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果函数包含可注入的 `db` 参数，也要说明。
- 可复用的 Astro 组件应记录其 `Props` 接口约定，相关代码更改时也应同步更新注释。
- 保留现有格式和 lint 指导。

使用说明文件可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它更好地理解您的编码风格和需求。这项功能可以引导 Copilot 提供更相关的建议和代码片段。您可以指定偏好的编码约定、库，甚至希望包含的注释类型。既可以为整个存储库创建说明，也可以针对特定文件类型或任务创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：这是一个针对存储库中**每个**请求都会发送给 Copilot 的说明文件。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他说明格式的支持有所不同。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 查看本项目中的自定义说明文件

示例项目中已经包含一组说明文件。现在先了解现有内容，再进行更改并观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器中（不是终端中）打开 `.github/copilot-instructions.md`。
3. 浏览该文件，留意项目简介和编码指导。这些说明适用于您在此存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览。留意其中针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它通过 glob 指定说明适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目现有文件的链接。这样可以将较大的说明拆分为更小、可复用的文件，并为 Copilot 提供可参考的示例。

## 更新说明文件以符合团队指导

现有文件是一个不错的起点，但仍有缺漏。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，应该在第 35 行附近。
2. 在该部分最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，请退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 将其关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

您了解了 Copilot CLI 如何从说明文件获取上下文，并将新标准应用到了功能中。具体来说，您：

- 了解存储库说明和路径范围说明文件如何传递给智能体；
- 更新说明文件，确保遵循编码标准；
- 观察了说明文件对代码的影响。

接下来，您将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试以一致的方式运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
