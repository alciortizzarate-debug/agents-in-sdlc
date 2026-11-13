---
title: "课程 2 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查结果，并将其作为你的第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始对项目进行第一次修改了。我们先从一个小改动开始：游戏数据中已经包含星级评分，但主页上的游戏卡片还没有显示它。你将让代理显示评分、检查修改，并将其合并为你的第一个拉取请求。

本课将介绍如何：

- 在功能分支上启动专注于单项任务的 Copilot 对话。
- 让代理对项目进行小幅修改。
- 使用 `/diff` 检查差异。
- 在转发的浏览器中运行应用并确认修改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已经显示评分。不过，主页上的游戏卡片目前只显示标题、类别、发行商和描述。作为热身，你将让代理在每张卡片上显示现有评分——这是一个范围明确的小改动，很适合第一次会话。

## 对话的组成

**对话**是你在 Copilot CLI 中处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可以让你稍后回到同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三项内容：你的提示和代理的响应、代理探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并提出修改请求

新建一个对话，开始实现这项功能。

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保已启动新会话。
5. 使用以下提示请求修改：

   ```plaintext
   在列表页的游戏卡片中显示每款游戏的 starRating（满分为 5 分）。如果评分为 null，则显示“No rating yet”。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目、找到用于显示游戏详情的文件，并创建所需代码。你已通过 Copilot CLI 添加了一项新功能！

## 检查差异

合并前必须检查所有 AI 生成的更改，即使改动很小也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个更改过的文件。
2. 确认游戏卡片在有评分时显示数值评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 检查 Copilot 运行的检查结果；如有失败，请让 Copilot 修复。
5. 检查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此生成的具体代码可能有所不同。请检查功能行为，而不是要求实现与某个示例完全一致。

## 检查更改

当然，不能只阅读代码就假定它能正常工作。让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中检查更新后的界面。

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中检查星级评分修改。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示满分为 5 分的评分。
4. 返回 Copilot，并让它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

功能已完成！现在创建一个拉取请求（PR），将新代码合并到项目中。

1. 让默认代理提交更改：

   ```plaintext
   使用合适的提交消息提交已审查的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 检查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot CLI 发布了第一个更改！具体来说，你：

- 在功能分支上启动了专注于单项任务的 Copilot 对话。
- 指示代理对游戏卡片进行小幅修改。
- 使用 `/diff` 检查了更改。
- 在转发的浏览器中运行应用并确认了星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选议题开始，使用 Plan 和 Autopilot 模式][next-lesson]来构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
