---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。提供这些指导的主要方式之一是使用[指令文件][instruction-files]，指导 Copilot 如何生成代码。

本练习将介绍如何：

- 通过存储库自定义指令和按路径指定的指令文件，让 Copilot 获取项目上下文、编码指南和文档规范。
- 在现有指令下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准。
- 再次运行相同提示，观察生成的代码如何应用新标准。
- 将指令更新和筛选代码切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不符合你设定的某些标准，因为 Copilot 具有概率性。本练习的目标是观察更新指令前后行为的变化趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 和优秀的开发团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，可以确保 Copilot 按照上述实践处理任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。例如，你可以指定编码约定、库以及希望采用的注释风格。你可以为整个存储库创建指令，也可以为特定任务或文件类型创建指令。

指令文件有两类：

- `.github/copilot-instructions.md`：单个指令文件，会随此存储库的**每个**请求发送给 Copilot。此文件应包含大多数请求都适用的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件，例如 TypeScript 文件，或创建 UI 组件、单元测试等任务。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本练习的范围。不过，示例项目展示了常见做法。总体而言：

- 将 `copilot-instructions.md` 中的指令限定在项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或任务提供指导。
- 使用自然语言，确保指导清晰，并说明代码应当如何编写以及不应如何编写。

创建指令文件并没有唯一的方法，使用 AI 也是如此。通过试验，你会逐渐找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件集合。查看本项目的文件时，你可能会发现有些文件用于指导[用户界面更新][ui-instructions]和 [Astro][astro-instructions]等任务。
>
> Copilot 也可以帮你生成指令文件。不同产品界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前产品对应的课程会在相关处说明。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目的自定义指令文件

入门项目已附带一组指令文件，其中包含一个核心 `copilot-instructions.md` 文件和一系列适用于不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网站界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看文件，留意项目简要说明以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。**Code standards** 中还包含 **GitHub Actions Workflows** 子章节。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob 模式。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看该文件中针对单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并引用项目示例。（这些路径是相对于指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码标准，但尚未要求在代码中添加文档注释。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

先在现有指令下发送提示，观察自定义指令的效果。稍后会更新指令文件并再次发送相同提示，比较两次结果。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，告诉 Copilot 辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，让 Copilot 应用文件更改。

   ![Chat 视图中显示代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回包含所有发行商名称和 ID 的列表。应用文件更改。
   ```

5. Copilot 会探索项目并进行代码更新，通常会同时修改辅助函数文件及其测试。
6. 留意生成的辅助函数是带类型的函数，第一个参数是 `db` 客户端，返回带类型的发行商数组。这些约定来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层指令。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此它也可能在没有明确指示的情况下添加文档注释。如果遇到这种情况也没有关系；本练习仍然可以观察更新指令后的一致性变化。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项编码标准，提高代码建议的质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，通常位于第 27 行附近。注意该章节记录了项目的编码标准，但还没有关于代码文档的规则，因此生成的辅助函数不会包含文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，指令文件可以放在存储库级 `.github/copilot-instructions.md` 中，为所有请求提供指导；也可以作为 `*.instructions.md` 文件，针对特定语言、文件类型或任务提供指导。存储库级文件适合放置新添加的文档注释规则等全局标准。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回包含所有发行商名称和 ID 的列表。应用文件更改。
   ```

4. 留意建议的文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 留意建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns Promise，解析为发行商对象数组。
    */
   ```

现在你已引导 Copilot 遵循新的项目标准，并将其应用到真实代码中，后续练习会基于这些代码继续开展。

## 提交指令并推送分支

指令文件与其他项目文件一样，应通过源代码管理流程进行管理。因此，现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改已保存：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并将其关联到待办事项中的文档标准议题。Copilot 可以使用项目已连接的 GitHub 工具查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是 Copilot 首次代表你操作 GitHub。项目模板已配置 GitHub 工具，因此无需设置任何内容。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这种连接（Model Context Protocol）的工作方式。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```plaintext
   查找与更新指令文件相关的议题。从当前分支创建新 PR，说明该 PR 会关闭此议题，并说明我们也添加了发行商辅助函数基础，为接下来的筛选工作做准备。
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 当 Copilot 提示允许它代表你执行 GitHub 操作时，请先检查请求，然后选择 **Allow**。
6. 创建 PR 后，让 Copilot 合并 PR 并将分支切回 `main`：

   ```plaintext
   将 PR 合并到 main。然后在本地切回 main 并拉取最新代码，确保代码已更新。
   ```

7. 当 Copilot 提示允许它执行 GitHub 操作和 shell 命令时，请先检查请求，然后选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你探索了 Copilot 如何获取本项目的指令文件上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在现有指令下的输出。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准。
- 再次运行相同提示，观察生成的代码如何应用新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，让 Copilot 在[代理模式][next-lesson]下使用这些指令，跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot - 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
