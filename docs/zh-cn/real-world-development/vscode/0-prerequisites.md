---
title: "第 0 课：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 课程前，需要先做好准备。你将创建 Tailspin Toys 存储库的副本，启动一个用于学习的 [Codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已正常运行。

## 设置实验存储库

要创建用于编写代码的存储库副本，请从[模板][template-repository]创建一个实例。新实例会包含实验所需的全部文件，你将在整个练习过程中使用它。

1. 在新浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面中选择 **Use this template**，然后选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

   ![填写存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下你创建的存储库路径（**organization-or-user-name/repository-name**），稍后会在实验中用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建存储库时，系统会自动为你创建 GitHub 问题待办事项。整个工作坊都会使用这些问题，无需自行创建。

## 创建 Codespace

接下来，你将使用 Codespace 完成实验。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它是一个功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

   ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，不过这比手动安装所有服务快得多！等待期间，可以探索 GitHub Copilot 的其他功能，接下来就会介绍这些内容。

> [!CAUTION]
> 后续练习还会使用此 Codespace。现在先不要关闭它，请在浏览器选项卡中保持打开状态。

> [!NOTE]
> 本工作坊可在 Codespace 或本地 [dev container][dev-containers] 中运行。两者都会安装顺利完成练习所需的先决条件。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和智能体模式

要在 IDE 中使用 GitHub Copilot Chat 智能体模式，需要安装 GitHub Copilot Chat 扩展。如果你使用 GitHub Codespace，该扩展通常已安装。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开“扩展”视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，可能还需要使用 GitHub 账户进行身份验证才能启用它。

1. 返回 Codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息，然后按 Enter。这会激活 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 账户。按照说明完成身份验证。

   ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，Copilot Chat 窗口应会显示出来。

## 总结

恭喜！你已创建实验存储库的副本，也开始创建 Codespace。接下来开始编写代码时会用到它。

## 下一步

现在让 Copilot 开始工作。继续学习[第 1 课 - 自定义说明][next-lesson]，向 Copilot 介绍项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
