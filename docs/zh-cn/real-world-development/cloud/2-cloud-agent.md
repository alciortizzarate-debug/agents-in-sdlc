---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都需要处理技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。任务完成后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给代理，加快开发流程，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要解决的技术债务。最初负责创建网站的承包商没有留下合适的文档——事实上，代码完全缺少文档。首先，团队希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队正在改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，也就是相同类别的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要一个可用于 UX 验收测试的版本。虽然该任务当前受阻，但还有优先级更高的议题。

这类任务很容易被搁置，因此很适合分配给 Copilot 云智能体。它可以异步处理任务，开发者则继续处理其他工作，之后再回来审查 Copilot 的成果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以在后台执行任务，方式类似于人类开发者。和人类开发者协作一样，有多种方式可以分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中通知你！

## 明确界定任务范围的重要性

GitHub Copilot 并非魔法——你仍然需要思考。即使看似简单的操作，深入分析后也会发现其复杂性，因此请[谨慎确定 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令以及它们如何引导 Copilot。如果已经完成自定义指令的练习，你就见过 `.github/copilot-instructions.md` 的效果；如果还没看过，现在正是快速浏览的好时机。分配任务前，先只读查看存储库中已有的指令文件，稍后便能观察它们的效果。

在 GitHub 网站界面中打开以下文件；如果已启动 codespace，也可以在那里查看：

- `.github/copilot-instructions.md` - 查看 **Code standards** 章节，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md` - 留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

在下一节中将 `Code lacks documentation` 议题分配给云智能体后，观察生成的 PR 是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述指令文件。后续练习审查 PR 时还会再次讨论。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保一切就绪。将任务分配给以类似软件工程师方式工作的 Copilot 时也是如此。

云智能体使用 [GitHub Actions][github-actions] 执行工作。你可以通过创建专用设置工作流来自定义环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 中，并会在智能体开始工作前运行。这样可以安装所需开发工具和依赖项。实验已预先配置该工作流，确保 Copilot 可以访问 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库（供单个 Astro 应用使用）：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤配置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果不在设置步骤中克隆，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流相同，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。该作业会在 Copilot 开始处理拉取请求前于 GitHub Actions 中运行。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，这样可以验证工作流是否成功，而不必等待 Copilot 启动。

## 添加文档

大家都知道文档很重要，但多数项目的文档要么过时，要么完全缺失。这类技术债务经常得不到处理，降低效率、增加代码维护难度，也让新开发者更难加入项目。Copilot 很擅长生成文档，因此这是分配给 Copilot 云智能体的理想议题。它会在后台生成所需文档，后续练习中再审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 创建的待办事项列表可能需要几分钟才会显示。Copilot 正在创建工作环境（运行前面介绍的工作流）、分析项目并确定处理任务的最佳方案。

13. 查看待办事项列表及其计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、理解当前状态、暂停思考并决定合适方案，以及生成代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步处理任务，让开发者可以专注于其他工作。现在就利用这一点：再分配一个任务给 Copilot 云智能体，然后开始编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏部分

GitHub Copilot 云智能体的一个优势是可以拆分工作：你专注于一组任务，代理则处理另一组。为设计团队添加相关游戏部分可能不会花很长时间，但也可以将这段时间用于其他工作。现在将任务分配给云智能体！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
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

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论旁会出现一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！云智能体的工作方式类似软件工程师，因此你无需持续监控，完成后再审查即可。接下来看看如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以开始创建和使用自定义代理。[Copilot 云智能体也支持 MCP 服务器][cloud-agent-mcp]，还可以使用自定义指令，前面的课程中已介绍过这些内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
