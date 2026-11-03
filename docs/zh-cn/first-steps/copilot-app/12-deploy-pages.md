---
title: "练习 12 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审阅拉取请求，并验证 Space Quiz 是否已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开访问入口。Copilot 可以准备工作流并引导部署，但仍需由你审阅更改并确认网站已上线。

在本课中，你将：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审阅工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已经存储在 GitHub 仓库中，但用户还需要一种简单的试用方式。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖，即可通过公开 URL 访问。

## 让 Copilot 部署网站

在 Copilot app 的 `space-quiz` 会话中，发送以下提示：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；GitHub Actions 的 Deploy website 运行成功，其中包含 “Build website”、“Upload artifact” 和 “Deploy to Pages” 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求代理创建并合并拉取请求，以及更改仓库的 Pages 设置。继续操作前，请审阅差异、部署权限和 GitHub 的审批提示。

## 审阅工作流并验证部署

1. 确认只有在仓库中尚无 README 时才添加 README。
2. 审阅拉取请求中的更改，包括工作流的构建和部署步骤。
3. 只有必需的拉取请求检查全部通过后，才合并拉取请求。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告中的 URL，确认 Space Quiz 已加载。

如果代理无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。只有部署成功且网站可以正常加载后，任务才算完成。

## 总结和后续步骤

Space Quiz 现在已作为在线网站发布，而且你亲自验证了部署，而不是只依赖代理的报告。继续学习[练习 13：审阅和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
