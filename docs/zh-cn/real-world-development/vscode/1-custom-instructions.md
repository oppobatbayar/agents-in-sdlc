---
title: "第 1 课 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。你可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]来指导代码生成。

本课将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和筛选功能切片提交到 `main`，以便 Copilot 在下一课中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设定的部分标准。Copilot 的输出具有不确定性。本练习的重点是观察更新说明后行为的**整体变化趋势**，而不是要求输出逐字相同。

## 说明文件

### 场景

Tailspin Toys 与优秀的开发团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应有一段注释，说明文件的用途。

使用说明文件可以确保 Copilot 掌握正确的信息，按照这些实践执行任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。它可以让你引导 Copilot 生成更符合需求的建议和代码片段。你可以为整个存储库指定编码约定、库以及注释风格，也可以为特定文件类型的任务创建说明文件。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库发出请求时，都会发送给 Copilot 的单个说明文件。它应包含与大多数聊天或 CLI 请求相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型创建的文件，可针对 TypeScript 或 Astro 等语言，或创建 UI 组件、单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级说明文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊的范围。不过，示例项目中的文件展示了一种有代表性的做法。总体而言：

- 让 `copilot-instructions.md` 聚焦于项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码应当和不应当如何编写的示例。

说明文件没有唯一的编写方式，正如使用 AI 也没有唯一方法。通过尝试，你会找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的说明文件集合。查看本项目中的文件时，你可能会发现有些文件针对多种任务提供指导，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作方式不同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；相关课程会在适用时介绍具体操作。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

花些时间阅读存储库自带的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它通过相对于存储库根目录的 glob 模式指定适用文件。例如，任何符合 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意它对其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成较小、可复用的文件，并为 Copilot 提供示例。（这些路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求为代码添加说明。接下来你会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建一个分支。

1. 从上一课返回 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个全新的对话。模式和模型选择因练习而异；每一步会在适用时说明。

先在当前说明文件下发送提示，观察自定义说明的影响。稍后会更新说明文件，再发送相同提示进行比较。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取无关上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 能够修改文件。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意生成的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有不确定性，因此即使没有相关说明，它也可能添加文档注释。如果出现这种情况也没关系，本练习的重点仍然是观察更新说明后行为的一致性改进。

## 添加存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库添加编码标准，改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意该部分描述了项目的编码标准，但还没有规定代码文档要求，因此之前生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件 (`.github/copilot-instructions.md`) 中设置全局指导，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。刚刚添加的文档注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将它应用到下一课会继续使用的真实代码中。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，也需要通过源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也一并添加。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并将其关联到待办事项中的文档标准问题。也可以手动完成，但已有 GitHub 工具连接到项目，因此让 Copilot 帮忙查找问题、创建用于关闭该问题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已经在工作区中配置了 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这种连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选中 **Agent**。
3. 要求 Copilot 找到与更新说明文件相关的问题，并从当前分支创建 PR，说明其中包含说明更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找问题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，请先检查命令，再酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 shell 命令时，请先检查命令，再酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并拉取请求！

## 总结和后续步骤

你已了解 Copilot 如何获取本项目说明文件中的上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，并观察 Copilot 如何根据**现有**说明生成代码。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 再次发送相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和辅助函数基础提交到 `main`，以便下一课继续使用。

接下来，使用这些说明引导[智能体模式][next-lesson]中的 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更有效 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
