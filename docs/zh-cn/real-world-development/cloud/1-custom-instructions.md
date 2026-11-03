---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可以提供此类指导，让 Copilot 不仅了解你想让它做什么，也了解你希望它如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过仓库自定义指令及按路径限定范围的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会比较提示修改前后的效果。Copilot 云智能体会异步处理 GitHub 议题，因此很难实时并排展示影响。稍后审阅云智能体生成的拉取请求时，可以看到指令文件产生的影响。

## 指令文件

### 场景

Tailspin Toys 团队制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，可以让 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它可以引导 Copilot 提供更符合需求的建议和代码片段。你可以为整个仓库设置首选编码规范、库和注释习惯，也可以针对特定文件类型设置指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件。内容应提供项目级信息，例如技术栈、项目概述、最佳实践，以及对大多数 Copilot 聊天或 CLI 请求都适用的指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供指导的文件。你可以为 TypeScript 或 Astro 等语言，或创建 UI 组件、编写单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用仓库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件格式，包括 AGENTS.md 和 CLAUDE.md。请参阅[自定义指令支持情况][custom-instructions-support]。

### 管理指令文件的最佳实践

创建指令文件的完整过程超出本工作坊范围，但示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供指令。
- 使用自然语言，保持指导清晰，并提供代码应如何编写或不应如何编写的示例。

指令文件没有唯一的正确写法，AI 的用法也不止一种。通过实践可以找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护完善的指令文件集。浏览本项目中的文件时，你可能会发现其中有适用于多种任务的指令，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适当位置介绍对应操作。
>
> 需要模板或起点？浏览 [awesome-copilot][awesome-copilot]，其中收录了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间阅读仓库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于各种任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网站界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，了解简短的项目说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置了一个相对于仓库根目录的 glob，用于决定指令适用的文件。例如，任何符合 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看适用于本项目单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意它对其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并为 Copilot 提供参考示例。这里的路径相对于指令文件所在位置，而非仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档。下一步将添加 TSDoc 文档注释和文件头部注释的规则。

## 添加新的仓库标准

接下来需要编辑一次：添加项目级规则，要求代码内文档使用 TSDoc 文档注释，并为每个文件添加用途说明注释。之后本学习路径中分配的议题将由云智能体处理，它会遵循这项规则。

编辑前，先创建一个分支（云智能体会读取议题目标分支中的指令，但仍建议单独提交本次更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项仓库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。注意该部分记录了项目编码标准，但尚未规定代码内文档，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头部注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可以放在仓库级 `.github/copilot-instructions.md` 中，提供全局指导；也可以放在 `*.instructions.md` 文件中，针对特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，因此放在仓库级文件中最合适。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习将把议题分配给 Copilot，届时它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上从 `custom-instructions-cloud` 向 `main` 创建拉取请求并合并。下一练习中，云智能体处理议题时就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目中的处理方式。

## 总结和后续步骤

你探索了 Copilot 如何读取本项目的指令文件，并向 `.github/copilot-instructions.md` 添加了新的仓库级标准。接下来的学习路径中，可以在云智能体生成的拉取请求中观察这项标准的效果。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更有效 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
