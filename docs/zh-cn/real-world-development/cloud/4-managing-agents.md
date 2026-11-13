---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 Copilot 云智能体处理了三项不同任务，用于改进用户体验和添加功能。云智能体虽然能够异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，例如 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心中，可以查看所有包含开放拉取请求 (PR) 的代理任务、探索代理执行的操作，甚至可以引导正在进行的会话。

本课将介绍如何：

- 探索代理页面并监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给代理以创建高对比度模式后，团队意识到也可以顺便添加浅色模式。网站正在更新样式和开关功能，因此一起实现浅色模式也很合理。现在引导代理，要求它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 为游戏修改功能生成 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作。可能包括：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤因 Copilot 的工作状态和执行方式而异。

4. 留意右侧的拉取请求面板，可在其中查看 PR 和已更改文件，进一步监控任务。

## 引导云智能体

现在已经了解正在运行的任务，接下来要求 Copilot 在实现高对比度模式时添加浅色模式开关。

1. 选择与添加高对比度模式相关的会话。会话标题因 Copilot 使用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成环境设置并开始工作。此时会出现类似下文的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

   ```plaintext
   添加高对比度模式时，也添加浅色模式。用户也应能使用开关选择所需的显示模式。
   ```

   ![代理页面中的云智能体任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会开始处理更新后的任务！它会在完成收到消息时正在执行的具体步骤后，将新请求纳入工作流程。

如前所述，这需要几分钟，正好可以暂停一下，回顾至今学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在任务控制中心中，可以跟踪所有正在进行和已完成的任务、审查工作，也可以重定向正在运行的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用任务控制中心和代理页面监控云智能体任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
