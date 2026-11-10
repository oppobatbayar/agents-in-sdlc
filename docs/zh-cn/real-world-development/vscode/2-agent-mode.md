---
title: "第 2 课 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的工作流程通常包括：找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次修改……如此往复。

这正是 Copilot 智能体模式的用武之地。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。接下来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但智能体模式和 cloud agent 面向两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过问题和拉取请求与你协作。

本课将学习如何：

- 使用 Copilot 智能体模式探索项目、确定相关文件并协同修改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将更改合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户可以按类别和发行商筛选。上一课已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解它目前的功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台。它允许用户浏览游戏列表并查看详情。网站是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过范围有限的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行网站：

1. 返回 Codespace。上一课结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新的终端窗口。
3. 切换到 `main` 并更新代码，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的 URL 横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否要在浏览器窗口中打开 `http://localhost:4321`，请通过选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，在终端中选择 localhost URL 会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是一个通往 Codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

网站运行后，可以探索它的功能。网站的主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个全新的对话。模式和模型选择因练习而异；每一步会在适用时说明。

网站的初始实现已可正常运行，但我们希望添加新功能来改进它。先查看待办事项。使用模板设置项目时，系统已自动创建一组问题。现在让 GitHub Copilot 显示这些问题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某一种模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此操作说明中列出了该模型。这样做是希望代码建议相对一致，以提供良好体验。不过，由于 LLM 具有概率性，你看到的建议可能与本工作坊描述不同。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问 GitHub 问题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有问题的命令。
5. 查看生成的问题列表。

Copilot 还根据对用户的帮助程度为问题排序。

## 查看说明文件

让智能体生成代码之前，先查看用于向 Copilot 提供上下文的说明文件。我们将使用[用户界面 (UI) 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各类文件专用说明的链接，包括：
     - Astro。
     - Tailwind CSS。
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这样可以复用说明，将较大的说明拆分成较小、更易于管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项更改：

- 在数据层 (`src/lib/`) 添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，在将更改合并到代码库之前，必须运行并通过测试。Copilot 智能体模式可以完成这些任务！现在开始添加此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，根据待办事项中的相关问题要求 Copilot 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。你应能看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo**，因此无需在工作进行期间选择它们。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未发现错误。

   ![Copilot 智能体模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否有潜在问题。

> [!CAUTION]
> 无论代码由 Copilot 还是其他 AI 工具生成，都必须仔细审查。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本课介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用 Copilot 智能体模式探索项目、确定相关文件并协同修改。
- 在将更改合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 – 实现分页

随着游戏列表不断增加，网站需要支持分页。使用本课学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 考虑分页方式：允许用户选择每页大小，还是使用固定大小。
- 编写提示时提供必要指导，以便 Copilot 按照你的设想实现功能。
- 你可能需要与 GitHub Copilot 多次交互，要求修改并提供上下文。这是使用 Copilot 的正常工作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
