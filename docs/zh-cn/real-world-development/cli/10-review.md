---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到了预置的筛选问题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选问题开始，在 Plan 模式下确定方案，在 Autopilot 模式下构建，并在 Interactive 模式下检查结果。
- 使用自定义说明引导智能体，随后自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择了质量保证（QA）自定义智能体，评估需求、覆盖率、技能运行结果和浏览器证据。
- 检查了完整的筛选功能更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于管理上下文、模型、共享以及可选 cloud agent 委托的斜杠命令。

## 你交付的内容

本工作坊包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选功能、更新并应用说明、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为了简化工作坊流程，我们将这些更改合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按你新设定的格式报告结果。QA 将这些结果与需求和测试覆盖率审查结合起来，供 PR 审查使用。

添加的测试应解决实际缺口；QA 运行后不需要新增测试也可能是正确的。授权合并前，请检查代码和证据；代码发生更改后，应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明定义标准，技能描述可重复执行的任务，自定义智能体定义专家角色，MCP 服务器则连接外部工具。检查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前确定方案，使用 **Interactive** 协作完成范围明确的更改，并在任务范围清晰时使用 **Autopilot**。日常修改可选择速度较快的模型；复杂工作则可选择能力更强的模型。

上下文仍与基础设施同样重要。清晰说明要构建**什么**、**为什么**以及**如何构建**，会显著影响输出。

## 更多探索

你已完成核心工作流。还可以了解一些 CLI 功能：

- `/review`：请求代码审查智能体分析更改。
- `/rubber-duck`：通过讨论问题获得另一种思路。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

要熟练使用工具，最好的办法就是持续使用它！可以用它编写生产代码、兴趣项目，或多年来一直想做却未动手的小应用。与团队分享经验，也向团队成员学习。最后，别忘了继续探索文档。

如果想从终端继续扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
