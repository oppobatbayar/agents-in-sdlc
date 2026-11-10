---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方案，使用 Autopilot 根据问题构建筛选功能，再通过 Interactive 模式检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。较大的更改则需要更稳健的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助我们以正确的方式构建正确的功能。接下来的几课将介绍典型的智能体驱动开发流程：从问题着手创建新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 在整个功能工作流中，你会继续使用同一对话和分支。通常会针对要处理的不同文件类型使用不同分支或 PR；为了聚焦核心概念，这里会简化流程。

本课将学习如何：

- 从 GitHub 问题启动新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。待办事项中的问题描述了这项功能，但在编写代码前，需要先商定组合类别等细节。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 在限定范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则。事实上，这些原则会变得更加重要！大多数开发者的流程大致如下：

1. 从描述待办事项的问题开始。
2. 制定要构建内容的计划。
3. 构建并检查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这一标准流程，可以确保 AI 生成的代码满足需求，并像手写代码一样经过审查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并不断迭代，直至任务完成。

先使用 Plan 模式，检查计划，然后切换到 Autopilot 模式实现计划。

## 从问题开始

开始筛选功能之前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有问题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的问题。
9. 按 <kbd>C</kbd> 将问题添加到提示中，然后返回 **Session** 选项卡。

此时会看到提示以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的问题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以让你在交由 Copilot 处理之前，先确定实现功能或执行任务的方法。对于复杂任务，花些时间规划总是值得的。切换到 Plan 模式，并要求 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的问题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划！它会先探索项目，再确定合适的实现方式。

3. Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答，没有标准答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动查看计划。
6. 如果计划中的任何部分不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划已经编写并检查完毕，现在可以开始实现了！使用 Autopilot，让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已经解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中措辞相似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 查看 Copilot 如何按照已确定的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 检查并验证实现

生成的代码与其他代码一样，在合并前需要经过检查。现在一起检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与问题描述以及规划时做出的决定进行比较。
4. 检查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但能运行吗？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及规划时商定的组合方式。
4. 确认重置和无结果时的行为符合问题描述和你的决定。
5. 返回 Codespace，要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并检查功能。本课中，你：

- 从 GitHub 问题启动了新的 Copilot 对话。
- 在 Plan 模式下明确了需求。
- 使用 Autopilot 模式实现了新功能。
- 检查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，我们将[深入了解代码生成，确保生成的代码遵循文档化的实践][next-lesson]，并使用自定义说明。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
