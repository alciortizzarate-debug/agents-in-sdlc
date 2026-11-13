---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 一起完成了多项改善用户体验和添加功能的任务。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式开关的无障碍功能。现在查看代码更改，并在需要时向 Copilot 提供反馈，进一步完善工作。

### 场景

正如前面多次提到的，生成式 AI 不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码并遵循正常的 DevOps 流程。现在先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能创建的建议，然后再邀请团队其他成员参与审查。

## 安全和 GitHub Copilot 云智能体

Copilot 云智能体会异步且无需监督地执行任务，因此需要采取一些安全限制：

- Copilot 只能读取存储库，并且**只能**写入用于保存代码的分支。
- 云智能体在 GitHub Actions 中运行，并会创建一个单独的临时工作环境。
- 所有 GitHub Actions 工作流在运行前都需要人工批准。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含为代码添加的文档。你将通过 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。这可能需要几分钟，可以趁此休息一下或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新代码中的 TSDoc 文档注释和其他文档。具体更改会有所不同。

   检查代码时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在拉取请求的检查列表中并开始排队。顺利的话，单个 Astro 应用的项目检查将全部通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并不是单向的。你可以像对待团队成员一样，在拉取请求评论或代码内联评论中提及 Copilot。Copilot 会读取评论，并启动新会话处理修改。由于结果具有非确定性，我们无法提供固定的请求文本。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明其用途的注释头。
- 在 TypeScript 和 Astro 文件中添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上面的任一示例，也可以提出其他文档改进建议。
2. 选择 **View Session**，观察 Copilot 如何开始新会话并执行修改。
3. 选择 **Back to pull request** 返回拉取请求。

   ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿转换为准备审查状态，然后合并到主分支。

![将 PR 转换为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来审查 Copilot 为解决游戏详情页显示相关游戏这一议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到一些工作流正在等待批准。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会出现在拉取请求的检查列表中并开始排队。顺利的话，单个 Astro 应用的项目检查将全部通过。此过程可能需要几分钟。
9. **可选：**也可以在 Codespace 中检出该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能！与之前一样，你可以继续迭代并要求云智能体进行修改。例如，可以调整显示的相关游戏数量，或要求添加文件注释头和 TSDoc 文档注释（请记住，分配此议题时，你还没有更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中发表评论，Copilot 会收到评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义代理实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心要求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，尤其留意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储持久保存用户偏好的方式。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在拉取请求的检查列表中并开始排队。顺利的话，单个 Astro 应用的项目检查将全部通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中检出该分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式开关，确认功能正常并能在页面重新加载后保留设置。

无障碍自定义代理帮助 Copilot 根据无障碍最佳实践实现这些功能。如果发现无障碍问题或改进空间，可以像处理前面 PR 一样，在评论中提及 **@copilot** 并请求修改。

## 可选练习 - 继续委托任务

云智能体特别适合处理待办事项，让你可以把任务交给它并继续其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，逐步培养这种习惯。以下是一些建议：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。本系列课程中，你：

- **检查了存储库附带的自定义指令文件**，以便观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义代理**来处理无障碍问题，添加高对比度和浅色模式开关。
- **使用 Copilot 代理页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代云智能体的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

云智能体学习路径已完成。如果想继续探索，其他学习路径会补充不同的练习内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 从终端交付两个经过审查的更改，学习 Plan 和 Autopilot 模式、智能体技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下内容：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义代理。
- 针对不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
