---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审阅工作

在本实验中，你与 GitHub Copilot 合作完成了多项改善用户体验和添加功能的任务。你让 Copilot 为代码添加文档、实现供设计团队迭代的相关游戏功能，以及添加无障碍功能，包括高对比度和浅色模式切换。现在一起检查代码更改，必要时向 Copilot 提供反馈，进一步改进成果。

### 场景

如前所述，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审阅生成的代码，并遵循正常的 DevOps 流程。基于这一点，在允许团队其他成员开始审阅前，先检查 GitHub Copilot 提出的文档、相关游戏功能和无障碍功能更改。

## GitHub Copilot 云智能体的安全性

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此设置了一些安全限制：

- Copilot 对仓库只有读取权限，并且仅对其用于编写代码的分支具有写入权限。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境来处理任务。
- GitHub Actions 工作流每次运行前都需要经过人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审阅生成的文档

先查看 GitHub Copilot 云智能体生成的第一个拉取请求 (PR)，其中包含代码文档。通过 GitHub.com 的标准 PR 界面完成审阅。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以选择[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求，标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等 Copilot 完成工作。这可能需要几分钟，可以利用这段时间休息或回顾已学内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审阅更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

   浏览更改时，留意 TSDoc 文档注释、TypeScript 规范和文件头部注释。这些要求来自练习 2 开始时查看的自定义指令文件。

6. 审阅更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在拉取请求的检查区域中。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像 @提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新的会话来处理请求。由于结果具有非确定性，无法提供唯一固定的提示示例。你可以要求 Copilot：

- 在每个代码文件开头添加简短说明文件用途的注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像提及用户一样标记 **@copilot**。可以使用上面的任一示例，也可以提出其他代码文档建议。
2. 选择 **View Session**，观察 Copilot 执行修改。留意 Copilot 会启动新会话来更新内容。
3. 选择 **Back to pull request** 返回拉取请求。

   ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡，审阅更改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿转换为准备审阅状态，并合并到 main 分支。

![将 PR 转为准备审阅状态](../../../_images/ex7-ready-for-review.png)

## 审阅相关游戏功能

现在查看 Copilot 为解决游戏详情页显示相关游戏议题而创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR，标题也可能更具体。
4. 选择 **Files changed** 选项卡，审阅生成的代码。
5. 审阅更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会出现在拉取请求的检查区域中。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。进入 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已实现相关游戏功能。和之前一样，可以迭代请求云智能体更新，例如调整相关游戏显示数量，或添加文件头部注释和 TSDoc 文档注释（请记住，分配此任务时尚未更新自定义指令）。与前一项 PR 一样，可在 **Conversation** 选项卡中添加评论，Copilot 会据此启动新会话。

## 审阅无障碍功能

最后，审阅使用无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心提出的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR，标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审阅生成的代码。
5. 审阅实现，特别留意：
   - 用于切换模式的 UI 组件。
   - 是否使用本地存储保存用户偏好。
   - 高对比度和浅色模式相关的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审阅更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在拉取请求的检查区域中。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。进入 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认模式能按预期工作，且在重新加载页面后仍然保留。

可以看到，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派

当你可以把真实待办事项交给云智能体处理、自己转而做其他事情时，云智能体的价值最大。为养成这种习惯，可以再为仓库创建几个议题并分配给 Copilot。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。在这些课程中，你：

- **检查了仓库自带的自定义指令文件**，以便之后观察这些文件对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**进行无障碍工作，添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话。
- **审阅并迭代改进云智能体创建的拉取请求**，通过标记 `@copilot` 请求更改，并批准工作流。

## 回顾和后续步骤

你已完成云智能体学习路径。若想继续探索，其他学习路径可以延伸本次练习的内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中交付两项经过审阅的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的仓库中，可以尝试以下后续练习：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
