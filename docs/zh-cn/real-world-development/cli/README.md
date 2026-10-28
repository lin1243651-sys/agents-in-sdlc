---
slug: zh-cn/real-world-development/cli
title: "GitHub Copilot CLI"
description: "在学习 Copilot CLI 模式、自定义项、MCP 工具和拉取请求自动化的同时，构建、验证并交付 Tailspin Toys 的两项更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** 将 GitHub Copilot 作为智能体式编码助手带入终端。它可以探索代码库、生成代码、运行命令并连接外部工具，整个过程都在命令行中完成，无需切换到图形化编辑器。

本工作坊遵循一个连续的 Tailspin Toys 工作流：先在 GitHub Codespaces 中准备项目、安装 Copilot CLI 并熟悉环境；再完成星级评分更改并手动合并第一个拉取请求 (PR)；随后从筛选议题出发，使用 Plan 模式规划、Autopilot 模式构建、Interactive 模式审查；然后更新存储库说明并将其用于筛选功能，自定义 `quality-checks` 技能并运行项目检查，配置 Playwright 模型上下文协议 (MCP) 服务器以在浏览器中检查筛选行为，创建质量保证 (QA) 自定义智能体，审查完整更改并使用 Agent Merge 合并筛选 PR；最后探索斜杠命令。

整个工作坊只创建两个 PR：一个用于星级评分，另一个用于筛选功能及相关的说明、技能、QA 配置和测试。筛选与质量工作共用一个会话和分支，方便在探索工具时持续迭代。也可以选修[使用 GitHub Copilot CLI 和 Foundry 的课程][foundry]，通过三个模块准备模型、构建并部署托管智能体，再将其连接到网站。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 安装 Copilot CLI][ex1] | 安装 | 安装并验证 Copilot CLI |
| [2. 添加星级评分：快速上手][ex2] | 首次更改 | 显示现有评分和空值回退状态，再手动合并第一个 PR |
| [3. 智能体模式：Plan 和 Autopilot][ex3] | 智能体模式 | 从议题规划功能，使用 Autopilot 构建，再在 Interactive 模式中审查 |
| [4. 使用自定义说明引导 Copilot][ex4] | 上下文 | 探索并更新说明，再将其应用于筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 探索现有技能、更改报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 在 CLI 中配置 MCP 并检查筛选行为 |
| [7. 创建并使用 QA 智能体][ex7] | 需求与覆盖 | 创建并选择专用配置，收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查与合并 | 审查完整更改，创建 PR，并使用 Agent Merge |
| [9. GitHub Copilot CLI 斜杠命令][ex9] | CLI 功能 | 探索上下文、模型、共享及可选的云智能体委派 |
| [10. 总结与后续步骤][ex10] | 总结 | 回顾工作流、可复用的自定义项和后续资源 |
| [可选：集成 Foundry][foundry] | 托管智能体 | 准备模型、构建并部署基于目录的 Backer Concierge，并将其连接到网站 |

## 先决条件

参加本工作坊前，请确保已具备以下条件：

- [ ] 拥有 GitHub 账户，并已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 有权创建存储库和 Codespace
- [ ] 熟悉终端或命令行操作

> [!TIP]
> 没有付费计划？已验证学生可通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊所用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以完整完成所有路径。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 如果使用的是 Copilot Business 或 Copilot Enterprise，请确认管理员已启用 Copilot CLI。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

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
