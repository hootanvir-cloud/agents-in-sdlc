---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云智能体练习前，需要先做好准备。您将创建 Tailspin Toys 存储库的个人副本，并启动一个 [Codespace][codespaces]，用于编辑说明文件和审查云智能体生成的工作。

## 设置实验存储库

要为练习创建代码副本，请从[模板存储库][template-repository]创建一个实例。新实例会包含实验所需的所有文件，您将在练习过程中使用它。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上选择 **Use this template** 按钮，然后选择 **Create a new repository**，创建自己的存储库副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果您参加的是由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可以在您有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**组织或用户名/存储库名称**），稍后将在实验中使用。

## 创建 Codespace

接下来，您将使用 Codespace 完成本实验中的练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让您直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

    ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

Codespace 的创建需要几分钟，不过仍比手动安装所有服务快得多。您可以利用这段时间探索 GitHub Copilot 的其他功能，接下来我们会介绍这些功能。

> [!CAUTION]
> 后续练习中还会返回 Codespace。现在请先让它保持打开状态，并留在浏览器标签页中。

> [!NOTE]
> 本研讨会可在 Codespace 或本地 [开发容器][dev-containers]中运行。两者都会确保环境已安装所有必要的先决条件。如果希望在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜，您已经创建了实验存储库的副本！您还启动了 Codespace 的创建流程，后续将使用它与 Copilot 云智能体协作。

## 下一步

接下来添加云智能体将遵循的自定义说明。继续学习[练习 1 - 自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [根据模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
