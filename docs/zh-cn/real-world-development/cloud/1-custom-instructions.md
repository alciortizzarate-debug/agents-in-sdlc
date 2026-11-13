---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅知道你希望它做什么，也了解应如何完成。

本练习将介绍如何：

- 通过存储库自定义指令和按路径指定的指令文件，让 Copilot 获取项目上下文、编码指南和文档规范。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会使用“修改前/修改后”提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的效果。后续在本系列练习中审查云智能体创建的拉取请求时，你会看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 和优秀的开发团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，可以确保 Copilot 按照上述实践处理任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以通过自定义指令引导 Copilot 提供更相关的建议和代码片段，例如指定编码约定、库以及注释风格。你可以为整个存储库创建指令，也可以为特定任务或文件类型创建指令。

指令文件有两类：

- `.github/copilot-instructions.md`：单个指令文件，会随此存储库的**每个**请求发送给 Copilot。此文件应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都适用的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的指令文件，例如 TypeScript 文件，或创建 UI 组件、单元测试等任务。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本练习的范围。不过，示例项目展示了常见做法。总体而言：

- 将 `copilot-instructions.md` 中的指令限定在项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
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

花些时间阅读此存储库附带的指令文件，其中包含一个核心 `copilot-instructions.md` 文件和一系列适用于不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网站界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看文件，留意项目简要说明以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。**Code standards** 中还包含 **GitHub Actions Workflows** 子章节。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob 模式，用于确定适用的文件。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看该文件中针对单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并引用项目中现有的示例。（这些路径是相对于指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码标准，但尚未要求在代码中添加文档注释。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要做的唯一编辑是：添加一项项目级规则，要求代码文档使用 TSDoc 文档注释和文件级注释头。稍后分配给云智能体的议题会使用这项规则。

编辑前先创建一个分支（云智能体会读取议题目标分支中的指令，但你仍应规范地提交自己的修改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并指令更新

云智能体会从议题所指向的分支读取指令。下一练习中，你将把议题分配给 Copilot 云智能体，它会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。下一练习中分配议题后，云智能体就会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 分支上完成本研讨会，可以跳过创建分支的步骤，并直接提交到 `main`。此处创建分支是为了让练习更接近真实项目的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何获取本项目的指令文件上下文，并在 `.github/copilot-instructions.md` 中添加了一项存储库级标准。接下来的课程中，你会看到云智能体在创建的拉取请求中如何应用该标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot - 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
