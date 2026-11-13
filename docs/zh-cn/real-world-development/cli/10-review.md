---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流程中使用 GitHub Copilot CLI。本系列课程中，你：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到已创建的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并亲自合并第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式下确定方案、在 Autopilot 模式下构建，并在 Interactive 模式下审查。
- 使用自定义指令引导代理，并自定义现有的 `quality-checks` 技能，用于运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中试用筛选功能。
- 创建并选择质量保证 (QA) 自定义代理，检查需求、测试覆盖率、技能结果和浏览器证据。
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、共享和可选云端委托的斜杠命令。

## 完成的工作

本研讨会包含两个 PR 里程碑，各自基于更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流程**：实现筛选功能、更新指令并将其应用到功能中、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一个对话和分支。为了简化研讨会流程，这些工作合并为一个 PR。

## 不同类型的验证

你使用了多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 操作浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 代理将这些结果与需求和测试覆盖率审查结合起来，为 PR 提供参考。

新增测试应解决实际覆盖缺口；如果 QA 检查后认为无需添加测试，也可能是合理结果。授权合并前，请检查代码和证据；更改之后，应重新检查受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令定义标准，技能描述可重复执行的任务，自定义代理定义专用角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只是代理的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑实现方式，使用 **Interactive** 跟进聚焦的更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可选择更快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建什么、为什么构建以及如何实现，会显著影响输出。

## 探索更多功能

核心工作流程已经完成。还可以探索以下 Copilot CLI 功能：

- `/review`：让代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调彼此独立的子任务。
- `/worktree`：隔离其他任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

持续使用工具是提升熟练度的最佳方式！可以用它处理生产代码、兴趣项目，或者多年想做却一直没做的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想继续在终端中扩展 Tailspin Toys，可以学习可选的 [Foundry Backer Concierge 系列][foundry]。要比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
