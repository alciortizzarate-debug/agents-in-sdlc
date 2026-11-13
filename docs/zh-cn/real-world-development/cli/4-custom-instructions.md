---
title: "课程 4 - 使用自定义指令引导 Copilot"
description: "查看存储库指令、添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，就应让 Copilot 能够获取相关指导。[指令文件][instruction-files]不仅说明你希望生成什么代码，也说明代码应如何组织。现在你已经构建了筛选功能，接下来将查看 Copilot 使用的指令、添加文档标准，并将其应用到代码中。

本课将介绍如何：

- 查看存储库指令和按路径指定的指令文件如何传递给代理。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 和优秀的开发团队一样，有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也要说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时应同步更新注释。
- 保留现有格式和 lint 指南。

通过使用指令文件，可以确保 Copilot 按照上述实践处理任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。例如，你可以指定编码约定、库以及希望采用的注释风格。你可以为整个存储库创建指令，也可以为特定任务或文件类型创建指令。

指令文件有两类：

- `.github/copilot-instructions.md`：单个指令文件，会随此存储库的**每个**请求发送给 Copilot。此文件应包含大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 查看本项目的自定义指令文件

入门项目已附带一组指令文件。现在先查看现有文件，再进行修改，以了解指令的效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件，留意项目简要说明和编码指导。这些指令适用于你在此存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个 glob 模式，决定哪些文件适用。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并引用项目示例。

## 更新指令以符合团队标准

现有文件是不错的起点，但仍缺少一些要求。现在修改 `copilot-instructions.md`，确保新生成的 TypeScript 包含文档注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 章节，通常位于第 35 行附近。
2. 在该章节最后一条后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 应用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存更新后，退出并恢复筛选对话，使 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了指令和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff` 并检查更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你探索了 Copilot CLI 如何读取指令文件，并将新标准应用到筛选功能中。具体来说，你：

- 查看了存储库指令和按路径指定的指令文件如何传递给代理。
- 更新了指令文件，确保遵循编码标准。
- 了解了指令文件对代码的影响。

接下来，使用现有的 `quality-checks` 技能并对其进行自定义，以便[统一运行检查][next-lesson]。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot - 指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
