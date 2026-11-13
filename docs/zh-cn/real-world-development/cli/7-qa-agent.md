---
title: "课程 7 - 创建并使用 QA 代理"
description: "创建一个 QA 自定义代理，结合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中检查筛选功能。现在将这些能力整合到一个具有明确 QA 流程的自定义代理中。

本课将介绍如何：

- 了解自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理，并根据筛选议题审查其结果。

## 场景

在创建拉取请求（PR）之前，Tailspin Toys 希望统一检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程，并提供可复用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用角色。该配置文件描述代理的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用过的几种自定义能力各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力检查需求并报告发现。它不会取代这些能力，也不需要启动另一个对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建一个可复用的 QA 配置文件。它将定义 QA 要执行的检查以及必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 让默认代理创建新的自定义代理：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义代理。它应根据议题和已商定的需求检查功能、遵循存储库指令、运行 quality-checks 技能、使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   它应提供报告，将每项需求标记为通过、失败或受阻，并附上支持证据。修改实现代码前必须先询问；不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新代理之前，先检查配置文件，确认 Copilot 捕获了预期的 QA 流程和边界。这样可避免代理在你只想验证功能时进行不完整或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 为必需字段，`name` 是可选字段，但添加后可显示清晰名称。
3. 阅读代理指令，确认 QA 会从需求开始，遵循存储库指令，运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据，修改实现代码前会先询问，也不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少上述责任或边界，请让默认代理先修改再继续。

## 根据议题运行 QA

创建配置文件后，Copilot CLI 会在对话启动时加载项目代理。恢复同一个筛选对话并选择 QA，即可使用已有的议题和规划决定。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 因为刚创建代理，它可能尚未出现在可用代理列表中。使用上面的命令仍可启用它。

2. 使用以下提示，让 QA 代理审查功能：

   ```plaintext
   根据议题和计划中确定的事项审查筛选功能。它是否可以提交 PR？
   ```

3. QA 代理会开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你添加了一个可复用的专用代理角色，并审查了它的工作。本课中，你：

- 了解了自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 代理，并根据筛选议题审查其结果。

现在，筛选功能实现、技能更新、QA 配置文件、测试和验证报告都已准备好接受审查。接下来，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
