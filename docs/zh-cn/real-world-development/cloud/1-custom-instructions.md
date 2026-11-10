---
title: "第 1 课 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。[说明文件][instruction-files]可为 Copilot 提供指导，让它不仅理解要做什么，也理解应该如何完成。

本课将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作坊不同，本课程不会通过前后对比提示来展示效果。Copilot cloud agent 会异步处理 GitHub 问题，因此难以实时并排展示说明的影响。稍后审查 cloud agent 创建的拉取请求时，你会看到说明文件发挥的作用。

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

花些时间阅读存储库自带的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它通过相对于存储库根目录的 glob 模式指定适用文件。例如，任何符合 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意它对其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成较小、可复用的文件，并为 Copilot 提供示例。（这些路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求为代码添加说明。接下来你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加存储库级标准

本课唯一需要编辑的内容是添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件注释头。稍后在本工作坊中为问题分配任务时，cloud agent 会读取这条规则。

编辑前，先创建一个分支（cloud agent 会从问题目标分支读取说明，但仍应在独立分支上提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库添加编码标准，改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意该部分描述了项目编码标准，但还没有规定代码文档要求，因此之前生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件 (`.github/copilot-instructions.md`) 中设置全局指导，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。刚刚添加的文档注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 提交、推送并合并说明更新

Cloud agent 会从问题的目标分支读取说明。下一课将问题分配给 Copilot 时，它会从 `main` 分支创建新分支，因此必须先将说明更改合并到 `main`，cloud agent 才能读取这些更改。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。之后 cloud agent 处理下一课分配的问题时就会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。这里保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何获取本项目说明文件中的上下文，并向 `.github/copilot-instructions.md` 添加了存储库级标准。在本工作坊后续课程中，你会看到 cloud agent 创建的拉取请求如何遵循这项标准。

接下来，将[第一个问题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更有效 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
