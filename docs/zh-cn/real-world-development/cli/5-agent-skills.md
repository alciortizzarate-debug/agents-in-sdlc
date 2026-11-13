---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能、自定义结果报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已手动验证代码，也使用指令文件确保代码符合标准。但测试、lint 以及持续集成 (CI) 中的其他步骤又该如何处理？

对于这类任务，**智能体技能**非常适用！技能能帮助 Copilot 正确执行这类操作。

本课将介绍如何：

- 查看现有的 `quality-checks` 技能。
- 自定义技能结果的报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更容易阅读。

## 指令、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和辅助资源打包在一起，由智能体按需加载。技能的核心是一个文件夹，其中包含 Markdown 文件 `SKILL.md`。该文件的 frontmatter 包含 `name` 和 `description`，后面是技能用途概述以及说明何时应调用技能的指导。文件夹还可以包含供技能使用的脚本和其他资源子目录。

> [!NOTE]
> 技能不一定需要额外的文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户的 `~/.copilot/skills` 文件夹中。

## 查看技能

现在看看 Tailspin Toys 团队创建的 `quality-checks` 技能。它用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。`description` 能帮助 Copilot 判断何时调用技能。
3. 阅读指令，了解其如何引导 Copilot 执行测试和 lint 流程。
4. 注意该技能目前没有 **Results output formatting** 章节。

## 修改前先运行技能

技能可以直接通过 Copilot CLI 调用，也可以使用自然语言调用。现在让 Copilot 运行该技能中的三项检查。

1. 返回筛选对话，并切换到 Interactive 模式。
2. 使用以下提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告。

## 自定义报告

我们希望报告能说明运行了什么、是否成功，以及工具实际报告的结果。现在更新技能，让它生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下章节：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

现在看看更新后的效果！Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告，并与第一次的报告比较。

## 总结和后续步骤

你自定义并使用了现有智能体技能。本课中，你：

- 查看了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的报告格式。
- 重新加载并运行了技能。

此更改将与筛选功能一同包含在 PR 中。接下来，通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接操作网站。

## 更多技能示例

以下社区示例仅供参考，不是额外的练习任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流程：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
