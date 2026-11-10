---
title: "第 4 课 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]让你可以创建专门处理开发工作流中特定任务或领域的 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，你可以为 Copilot 提供有针对性的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如确保符合 [WCAG][wcag] 的无障碍访问智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段，后面是定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是执行工作的角色，**技能**是工具。

自定义智能体拥有自己的上下文窗口，并且可以在工作中协调技能（甚至其他智能体）。在本实验中，无障碍访问自定义智能体会根据无障碍指南审查并更新网站；它可以在工作中调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的“正确”方式。与 AI 的其他用法一样，需要不断测试和迭代，找到适合自己环境和场景的做法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前暗色主题的文本与背景颜色对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍访问至关重要，因此希望尽快完成实现。你将使用自定义智能体生成此功能。

本课将学习如何：

- 查看现有的无障碍访问自定义智能体。
- 在 Copilot Chat 中使用无障碍访问智能体实现高对比度模式。

## 查看无障碍访问自定义智能体

已经为你创建了无障碍访问自定义智能体。先查看它的内容，了解它将如何引导 Copilot。

返回 Codespace，在终端中从 `main` 创建新分支，用于无障碍访问工作（第 3 课的筛选功能 PR 会保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 接着浏览后续部分，查看其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍访问最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，聊天会话就会遵循该智能体的说明和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个全新的对话。模式和模型选择因练习而异；每一步会在适用时说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选择 **Agent**，请选择它。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（当前智能体会显示在此处，默认为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍访问智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议并编辑项目文件。每项编辑都会以内联方式显示在聊天中，并提供文件路径和差异供你审查。
6. 随着编辑逐步完成，**Files changed** 指示器会更新，让你查看智能体修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一课将介绍如何审查并引导这项进行中的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域设计的专用 AI 助手。借助自定义智能体，你可以将团队的专业知识和标准整理成可复用的智能体，从而更有效地引导 Copilot 完成特定类型的工作。

本课介绍了以下内容：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，你将[监控并引导智能体的工作][next-lesson]，在更改生成过程中进行审查，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
