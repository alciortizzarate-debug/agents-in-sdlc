---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者往往需要先找到所有相关文件、进行修改、运行测试、调试、找出遗漏的文件并继续修改……工作内容常常会不断增加。

Copilot 代理模式正适合处理这类任务。

Copilot 代理模式可以在 IDE 中更自主地工作。它会像开发者一样先探索项目结构、执行必要的更新、运行测试等任务，并自动修复发现的问题。现在看看如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式与云智能体虽然名称相似，但适用于不同场景。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 代理模式探索项目、找到相关文件并协调多文件更改。
- 使用 GitHub Copilot 代理模式跨用户界面和数据层实现新功能。
- 在将代码合并到项目之前，审查代理模式生成的更改和测试。

## 场景

游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成剩余的数据层、界面和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先查看 Tailspin Toys 网站，了解它目前的功能。

网站是一个以开发者为主题的桌游众筹平台。用户可以浏览游戏列表并查看详情。网站是单个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库——没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中；交互功能由使用标准 DOM API 的轻量 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。请按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应已切换回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd> 打开新终端（Mac），或按 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道，网站服务器正在其中运行。

### 探索网站

网站运行后，可以查看其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

网站的初始实现已经可以使用，但我们希望添加新功能。先查看待办事项。根据模板创建项目时，系统已自动创建一组议题；现在让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理会自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中显示代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此示例使用了该模型，以便尽量保持一致。不过，LLM 具有概率性，收到的建议可能与课程示例不同。这是正常现象。

> [!NOTE]
> Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项：

   ```plaintext
   请显示我的 GitHub 存储库中的待办事项。请根据对用户最有用的程度，帮助我确定优先级。
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

请注意，Copilot 还根据它认为对用户最有用的程度对项目进行了排序。

## 查看指令文件

让代理生成代码前，先查看将用于为 Copilot 提供上下文的指令文件。我们将使用[用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含向网站添加功能的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型专用指令文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这样可以复用内容，将较大的指令拆分成更小、更易管理的部分，并在生成代码时引用示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项独立更新：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选界面。

此外，合并前还必须运行并通过测试。Copilot 代理模式可以执行这些任务！现在添加筛选功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中的代理下拉菜单仍选中 **Agent**。

   ![Chat 视图中显示代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   请根据待办事项中相关 GitHub 议题的要求，更新网站以支持按发行商和类别筛选。上一练习已添加发行商辅助函数，请保留并酌情完善该工作，同时完成剩余的数据层、界面和测试。完成前确保所有测试通过。服务器已经运行，无需重新启动。
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。它应能找到数据层辅助函数、界面和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中可能会看到代码编辑器中出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会出现 **Keep** 或 **Undo** 选项，因此不必在工作进行时立即选择。

7. 按 Copilot 提示选择 **Continue**，运行测试。

   ![Copilot Chat 面板中提示用户确认运行测试的屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 工作过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直至任务完成且未发现错误。

   ![Copilot 代理模式完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加了新功能。你了解了如何：

- 使用 GitHub Copilot 代理模式跨用户界面和数据层实现新功能。
- 使用 Copilot 代理模式探索项目、找到相关文件并协调多文件更改。
- 在将代码合并到项目之前，审查代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，然后为功能创建拉取请求。

### 附加探索练习 - 实现分页

随着游戏列表不断增加，网站需要支持分页。使用本练习中学到的技能，让 Copilot 实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用项目现有的指令文件。
- 考虑分页方式：允许用户选择每页大小，还是使用固定值。
- 为 Copilot 提供足够的指导，让它按你的期望实现功能。
- 你可能需要与 GitHub Copilot 多次迭代，提出修改请求并提供上下文。这是正常的协作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和使用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
