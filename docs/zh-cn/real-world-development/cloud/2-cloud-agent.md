---
title: "第 2 课 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都需要处理技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主完成更新代码、添加功能等任务。任务完成后，它会创建草稿 PR，供开发者审查。这样可以把繁琐的工作交给智能体，加快开发流程，让开发者专注于更重要的事项。

本课将使用 Copilot cloud agent 探索以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定问题范围的重要性。
- 将问题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有妥善编写文档，事实上几乎完全没有文档。首先，团队希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现体验。他们希望在每款游戏的详情页上显示相关游戏（同一类别中的其他游戏），让用户可以继续浏览。他们暂时不需要完善的实现，只需要一个可用于用户体验验收测试的版本。目前这项工作受阻，但还有优先级更高的问题需要先处理。

这两项任务都可能很快被推迟，因此很适合交给 Copilot cloud agent。它可以异步处理任务，让开发者专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。就像与人类开发者协作一样，你可以通过多种方式分配工作，例如[将 GitHub 问题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。你可以在 Copilot 进行中的会话里查看工作，也可以在任务完成后查看。解决方案准备好接受审查时，Copilot 会在拉取请求中通知你。

## 明确界定任务范围的重要性

GitHub Copilot 没有魔法，不能替你省去思考过程。即使看似简单的操作，也可能隐藏复杂问题，因此要[谨慎界定交给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、尝试并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的课程介绍过自定义说明及其如何引导 Copilot。如果你已完成自定义说明练习，就见过 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。在将工作分配给 Copilot cloud agent 前，先只读查看存储库中已有的说明文件，之后就能留意它们产生的影响。

在 GitHub 网页界面中打开以下文件；如果已有 Codespace，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些说明限定用于 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 问题分配给 cloud agent 时，请留意生成的 PR 是否遵循这些说明中的 TSDoc 文档注释、TypeScript 约定和文件注释头。稍后的课程会再次介绍如何审查 PR。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境状态正常。将任务分配给 Copilot 时也是如此，因为它会像软件工程师一样处理任务。

Cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。你可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，让它在开始工作前运行，以便访问所需的开发工具和依赖项。我们已预先配置此工作流，以便课程顺利进行并提供学习机会。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库（供单个 Astro 应用使用）：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅授予这些步骤所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果不在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro 和 Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有数据可用。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件类似，但有几个重点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会先运行该作业。
- 留意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，以便在等待 Copilot 运行之前验证工作流是否成功。

## 添加文档

每个人都知道文档的重要性，但大多数项目的信息要么过时，要么完全缺失。这类技术债务往往无人处理，降低工作效率，也让维护代码库或引入新开发者变得更加困难。Copilot 擅长编写文档，因此这是分配给 Copilot cloud agent 的理想任务。它会在后台生成所需文档，之后的课程会带你回来审查结果。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 创建空白问题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将问题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR）。它的标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看 Copilot 准备完成的任务列表。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理问题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并选择合适的方案，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们可以专注于其他工作。接下来会利用这一点：先再分配一项任务给 Copilot cloud agent，然后转而编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务，它则处理另一组任务。即使为设计团队添加相关游戏部分并不需要太长时间，也可以把这段时间用于其他工作。现在就把这项任务分配给 Copilot cloud agent！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将问题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在问题的第一条评论中看到一组 👀，表示 Copilot 已开始处理！

![Copilot 用眼睛表情符号表示正在处理问题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！Copilot cloud agent 会像软件工程师一样工作，因此无需一直监控它；等任务完成后再审查即可。现在来创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将问题分配给 cloud agent，让它异步处理任务。Copilot 可以帮助处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本课介绍了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定问题范围的重要性。
- 将问题分配给 Copilot。

Cloud agent 在后台工作时，我们可以开始创建和使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，而且能够读取前面模块中介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
