---
title: "第 4 课 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，你已让 Copilot cloud agent 处理三个任务，目标是改善用户体验并添加功能。Cloud agent 虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心可以查看所有已创建开放拉取请求（PR）的智能体任务，查看它们执行的操作，还可以引导正在进行的会话。

本课将学习如何：

- 使用智能体页面监控 cloud agent 任务。
- 在会话进行中引导智能体添加功能。

## 场景

将创建高对比度模式的任务分配给智能体后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，加入这一功能也很合理。现在引导智能体，要求它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看分配给 Copilot cloud agent 的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 留意主面板和左侧面板中的任务列表。应能看到你分配给 Copilot 的任务，包括：
   - 更新代码库文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 的工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 当前的工作状态和执行方式而异。

4. 还要留意右侧显示的 PR 面板。你可以在此查看 PR 和已更改文件，进一步监控任务。

## 引导 cloud agent

你已查看正在进行的任务。现在要求 Copilot 在实现高对比度模式时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题因 Copilot 使用的名称和当前工作状态而异。

   ![控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的 cloud agent 任务，其中突出显示了“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会开始处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求加入工作流程。

这同样需要几分钟，现在可以暂停一下，回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心控制台。你可以用它跟踪所有进行中和已完成的任务、审查已执行的工作，还可以重定向正在进行的任务来调整范围或补充指导。

本课介绍了以下内容：

- 使用控制中心和智能体页面监控 cloud agent 任务。
- 重定向正在进行的会话，要求添加功能。

Copilot 正在完成无障碍访问功能。接下来，开始[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
