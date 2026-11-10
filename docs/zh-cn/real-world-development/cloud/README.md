---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) 可在云端异步处理工作。你在 GitHub 上分配任务后，cloud agent 会在后台开始工作，探索存储库、进行更改并创建拉取请求，让你可以继续处理其他事情。

在这些课程中，你将添加 cloud agent 会遵循的自定义说明，然后分配 GitHub 问题并让它实现任务。你还将审查和使用自定义智能体来引导它的工作方式，在智能体页面监控并引导会话，最后审查它创建的拉取请求并迭代改进结果。

## 课程

| 课程 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加 cloud agent 会遵循的自定义说明 |
| [2. Cloud agent][ex2] | 异步智能体 | 将问题分配给 Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专用智能体 | 查看并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代改进][ex5] | 审查 | 审查 PR、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有 GitHub 账户，并已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 已为你的账户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？已验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊所用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以完成所有学习路径。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，所有计划（**Copilot Student、Pro、Pro+、Business 和 Enterprise**）均提供 cloud agent。在 Copilot Business 或 Enterprise 计划中，管理员必须为组织启用该功能。

> [!NOTE]
> 本工作坊不介绍 MCP。若想了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 工作坊](../cli/)或 [VS Code 工作坊](../vscode/)。

## 开始

**[从第 0 课“先决条件”开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
