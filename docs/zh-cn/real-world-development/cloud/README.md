---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 让 GitHub Copilot 在云端异步工作。你可以在 GitHub 上分配任务，云智能体便会在后台接手——探索存储库、进行更改并创建拉取请求——而你可以继续处理其他工作。

本系列练习将先添加云智能体要遵循的自定义指令，然后分配 GitHub 议题，让它实现相应工作。你将审查并使用自定义代理来引导它的工作方式，在代理控制面板中监控并指导会话，最后审查它创建的拉取请求并继续迭代结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云智能体要遵循的自定义指令 |
| [2. 云智能体][ex2] | 异步代理 | 将议题分配给 Copilot 云智能体 |
| [3. 自定义代理][ex3] | 专用代理 | 审查并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有 GitHub 帐户，并订阅有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 已为你的帐户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education] 验证身份的学生可免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会所用的代理、MCP、代码审查和 Copilot CLI 功能，因此可完成所有环境中的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，Copilot Student、Pro、Pro+、Business 和 Enterprise 均可使用云智能体。Copilot Business 或 Enterprise 组织需由管理员启用云智能体。

> [!NOTE]
> 本系列练习不涉及 MCP。若想了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 环境][cli]或 [VS Code 环境][vscode]。

[cli]: ../cli/
[vscode]: ../vscode/

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
