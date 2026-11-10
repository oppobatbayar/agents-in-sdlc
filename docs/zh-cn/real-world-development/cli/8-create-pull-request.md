---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起检查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置文件和测试都已保存在同一分支上。现在可以一起检查这些更改并创建拉取请求。你已手动合并星级评分拉取请求（PR）；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为了简化工作坊流程，筛选功能和质量工作流都保留在同一对话和分支中，所有更改会一起进入这个 PR。

本课将学习如何：

- 了解什么是 Agent Merge，以及它如何自动管理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 先完成审查，再启用 Agent Merge 并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了一项功能。现在，Tailspin Toys 希望自动处理剩余的 PR 工作，同时让开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的事项，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时进行变基；然后在存储库允许的情况下启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代为处理这项工作。启用 Agent Merge 前，先检查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

所有代码都已完成，现在一起检查代码、创建 PR，并让 Agent Merge 处理其余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监视 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认看到 Copilot 发出的类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或缺失的权限。如果流程受阻，请阅读报告的原因；在存储库允许的情况下，完成审查后再手动合并。

## 总结和后续步骤

你已自动处理开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。你：

- 了解了什么是 Agent Merge，以及它如何自动管理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在完成审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，你将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享以及可选的 cloud agent 委托。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
