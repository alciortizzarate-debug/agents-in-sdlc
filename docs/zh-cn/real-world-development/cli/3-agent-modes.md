---
title: "课程 3 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方案，使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。较大的更改则需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流程，帮助我们以正确的方式构建正确的功能。接下来的几课将介绍典型的代理驱动开发流程：从议题着手实现新功能，确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能流程会继续使用同一对话和分支。通常，不同文件类型的工作会放在不同分支或 PR 中，但为了让我们专注于核心概念，本练习会采用简化方式。

本课将介绍如何：

- 根据 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式决定这些事项，然后批准范围明确的 Autopilot 实现。

## 背景

将 AI 编码代理引入开发流程不会改变软件开发的基本原则，甚至会让这些原则变得更重要！大多数开发者遵循的流程大致如下：

1. 从描述待完成工作的议题开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准做法，可以确保 AI 生成的代码满足需求，并像手写代码一样经过审核。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在各模式间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作——编写代码、运行测试并不断迭代，直至任务完成。

先在 Plan 模式下启动，检查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能之前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划可以先确定功能实现方式，再交由 Copilot 执行。对于复杂任务，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan** 模式指示器。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   为实现此功能制定计划。
   ```

Copilot 会开始制定计划！它会先探索项目，再确定合适的实现方式。

3. 在此过程中，Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确的答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动检查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已编写并审查，现在可以开始实现！使用 Autopilot 让 Copilot 执行工作。

Autopilot 会让 Copilot 不断迭代问题，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中类似的选项。
2. 确认提示下方显示 **Autopilot** 模式指示器。
3. 观察 Copilot 如何按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，不要将未完成的检查视为已完成。

## 检查并验证实现

生成的代码与其他代码一样，需要在合并前审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并检查筛选功能的实现和测试。
3. 对照议题和规划时确定的事项检查结果。
4. 检查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目运行的检查结果；如有失败，请让 Copilot 修复。

## 试用新功能

代码看起来不错——但它真的能运行吗？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中试用筛选功能。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 试用类别筛选、发行商筛选以及计划中商定的组合方式。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并检查了一项功能。本课中，你：

- 根据 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式下确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 检查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，了解如何通过[自定义指令][next-lesson]进一步规范代码生成。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
