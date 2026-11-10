---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，你需要让 Copilot 能够获取相关指导。[说明文件][instruction-files]不仅描述你希望编写什么代码，还描述代码应如何组织。现在你已经实现了筛选功能，接下来将探索 Copilot 使用的说明、添加文档标准，并将其应用于代码。

本课将学习如何：

- 探索存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

与其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值（包括存在时可注入的 `db` 参数）。
- 可复用的 Astro 组件应说明其 `Props` 接口约定；相关代码发生更改时，注释也应保持更新。
- 应保留现有格式设置和 lint 指南。

通过使用说明文件，可以确保 Copilot 掌握正确的信息，按照这些实践执行任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。它可以让你引导 Copilot 生成更符合需求的建议和代码片段。你可以为整个存储库指定编码约定、库以及注释风格，也可以为特定文件类型的任务创建说明文件。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库发出请求时，都会发送给 Copilot 的单个说明文件。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。依赖某种格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 探索项目中的自定义说明文件

起始项目中已包含一组说明文件。先探索现有文件，再进行修改，以了解说明文件的作用。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件，留意其中对项目的简要说明和编码指导。此文件中的说明适用于与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它通过 glob 模式指定这些说明适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分成较小、可复用的文件，并链接到可参考的示例。

## 更新说明文件以匹配团队指南

现有文件已打下良好基础，但还缺少一项要求。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 将以下内容添加为最后一条项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，逐一查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认这些注释准确说明了代码。

## 总结和后续步骤

你已探索 Copilot CLI 如何选取上下文，并应用了新的代码标准。具体来说，你：

- 探索了存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新了说明文件，确保遵循编码标准。
- 了解了说明文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持情况][custom-instructions-support]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
