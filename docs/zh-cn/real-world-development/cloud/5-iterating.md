---
title: "第 5 课：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你使用 GitHub Copilot 完成了多个改善用户体验和添加功能的任务。你要求 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换开关的无障碍功能。现在来检查代码更改；如有需要，还可以向 Copilot 提供反馈，改进它的工作。

### 场景

正如前面多次提到的，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改，再向团队开放审查。

## 安全性和 GitHub Copilot cloud agent

由于 Copilot cloud agent 会异步且无人监管地执行任务，因此它受到一些安全限制，以确保操作安全。这些限制包括：

- Copilot 对存储库只有读取权限，对它用于编写代码的分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并创建独立的临时环境来工作。
- 任何 GitHub Actions 工作流在运行前都需要获得人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求（PR）：为代码添加文档。此处会使用 GitHub.com 标准的 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括调用外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或更具体标题的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，你可以稍作休息或回顾迄今为止学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并检查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新生成的 TSDoc 文档注释及其他文档。具体更改会因情况而异。

   检查更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些都来自你在第 2 课开始时查看的自定义说明文件。

6. 检查完更改并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 的检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像在团队中提及其他成员一样，在 PR 评论或代码内联评论中提及 Copilot。Copilot 会读取这些评论并启动新会话来处理请求。由于结果具有非确定性，我们无法提供固定的请求文本。可以要求 Copilot 更新以下内容：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 在评论中请求更改生成的文档，并像提及其他用户一样提及 **@copilot**。可以采用上面的建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，查看 Copilot 如何执行工作。留意 Copilot 如何启动新会话来进行更新。
3. 选择 **Back to pull request** 返回拉取请求。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。确认无误后，可以将 PR 从草稿状态改为可审查状态，再合并到 `main` 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为“在游戏详情页显示相关游戏”问题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或更具体标题的 PR。
4. 选择 **Files changed** 选项卡，检查生成的代码。
5. 检查完更改并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入 PR 的检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能！与之前一样，你可以继续迭代，要求 Copilot 更新结果。例如，可以要求它调整显示的相关游戏数量，或补充文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你**尚未**更新自定义说明）。同样，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍访问自定义智能体实现的功能。这个 PR 应包含你在第 3 课中分配的高对比度模式，以及你在第 4 课的控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或更具体标题的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，检查生成的代码。
5. 检查实现，特别留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式对应的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 检查完更改并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 的检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能正常，并且设置会在页面重新加载后保留。

无障碍访问自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委托任务

Cloud agent 最适合处理真实待办事项，让你能腾出时间做其他工作。可以在自己的存储库中创建更多问题并分配给 Copilot，养成这种协作方式。可以尝试：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 工作坊。在这些课程中，你：

- **检查了本存储库自带的自定义说明文件**，以便之后了解它们如何影响 cloud agent 的输出。
- **将问题分配给 Copilot cloud agent**，并观察它如何设置环境、制定计划并异步执行。
- **创建并使用无障碍访问自定义智能体**，添加高对比度和浅色模式切换开关。
- **使用 Copilot 智能体页面作为控制中心**，监控并引导无障碍访问会话。
- **审查并迭代改进 cloud agent 创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 工作坊。如果想继续探索，其他工作坊可以补充本次练习的内容：

- 🖥️ **[VS Code 工作坊](../../vscode/)** — 直接在 IDE 中体验 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作坊](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

在自己的存储库中，还可以尝试以下后续练习：

- 将重构或测试覆盖率问题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
