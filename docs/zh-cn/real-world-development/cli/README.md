---
slug: zh-cn/real-world-development/cli
title: "GitHub Copilot CLI"
description: "使用 Copilot CLI 模式、自定义项、MCP 工具和 PR 自动化，为 Tailspin Toys 实现、验证并交付两项更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** 将 GitHub Copilot 作为代理式编码助手带入终端。它可以探索代码库、生成代码、运行命令并连接外部工具，全部通过命令行完成。你无需切换到图形化编辑器，也能保持工作流畅。

本研讨会使用一个连续的 Tailspin Toys 工作流程：

1. 在 GitHub Codespaces 中准备项目、安装 Copilot CLI 并熟悉项目。
2. 修改星级评分，在浏览器中检查结果，并亲自合并第一个拉取请求 (PR)。
3. 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下构建，然后在 Interactive 模式下审查。
4. 更新存储库指令，并将其应用到筛选工作中。
5. 自定义现有的 `quality-checks` 技能并运行项目检查。
6. 添加 Playwright Model Context Protocol (MCP) 服务器，并在浏览器中试用筛选功能。
7. 创建质量保证 (QA) 自定义代理，检查需求、覆盖率和验证证据。
8. 审查完整的筛选功能更改，并对筛选 PR 使用 Agent Merge。
9. 探索用于管理上下文、模型、共享和可选云端委托的实用斜杠命令。

为使研讨会内容更聚焦，你将创建两个 PR：一个用于星级评分，另一个用于筛选功能及其指令更新、技能更新、QA 配置文件和测试。筛选和质量工作流程共用一个对话和分支，因此你可以在学习不同工具时继续扩展同一项工作。

## 课程

| 课程 | 主题 | 说明 |
| ------ | ----- | ----------- |
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 安装 Copilot CLI][ex1] | 安装 | 安装并验证 Copilot CLI，然后熟悉项目 |
| [2. 添加星级评分：快速改进][ex2] | 首次更改 | 显示现有评分和未评分时的备用文本，然后合并第一个 PR |
| [3. 代理模式：Plan 和 Autopilot][ex3] | 代理模式 | 根据议题规划功能，使用 Autopilot 构建，再用 Interactive 模式审查 |
| [4. 使用自定义指令引导 Copilot][ex4] | 上下文 | 查看并更新指令，然后将其应用到筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 查看现有技能、更改其报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器检查 | 在 CLI 中配置 MCP 并检查筛选行为 |
| [7. 创建并使用 QA 代理][ex7] | 需求和覆盖率 | 创建并选择专用配置文件，然后收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查和合并 | 审查完整更改、创建 PR 并使用 Agent Merge |
| [9. GitHub Copilot CLI 斜杠命令][ex9] | CLI 功能 | 探索上下文、模型、共享和可选的云端委托 |
| [10. 总结和后续步骤][ex10] | 总结 | 回顾工作流程、可复用自定义项和更多资源 |
| [可选：集成 Foundry][foundry] | 托管代理 | 准备模型、部署基于目录的代理，并将其连接到网站 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有 GitHub 帐户，并订阅有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 有权创建存储库和 Codespace
- [ ] 熟悉基本的终端或命令行操作

> [!TIP]
> 没有付费计划？通过 [GitHub Education][student-plan] 验证身份的学生可免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会所用的代理、MCP、代码审查和 Copilot CLI 功能。

> [!NOTE]
> 如果使用 Copilot Business 或 Copilot Enterprise，请确认管理员已启用 Copilot CLI。

## 开始

**[从先决条件开始 →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
