---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案、使用 Autopilot 模式构建，再通过 Interactive 模式审查。
- 使用自定义说明引导智能体，自定义现有 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索了管理上下文、模型、共享和可选云委派的斜杠命令。

## 已交付的更改

本工作坊包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的无评分状态。
2. **筛选和质量工作流**：实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一对话和分支。为简化工作坊，这些更改汇总到一个 PR 中。

## 不同类型的验证

你使用多种方式检查代码：自动化测试、亲自检查浏览器，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求及测试覆盖率审查结合起来，再交由 PR 审查。

新增测试应解决实际缺口；如果 QA 检查后认为无需添加测试，也可能是合理结果。合并前请审查代码和证据；代码更改后，应重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。在本工作坊中，你更新了说明、自定义技能、创建 QA 配置文件并配置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明定义标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑实现方案，使用 **Interactive** 处理需要密切参与的局部更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可选择更快的模型；复杂工作则可选能力更强的模型。

上下文与基础设施同样重要。清楚说明想构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出。

## 继续探索

核心工作流已完成。还可以了解一些 CLI 功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种视角。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最好方式是持续实践。将它用于生产代码、个人项目，或者一直想做却尚未开始的小应用。与团队分享经验并向团队学习，也别忘了查阅文档。

若要从终端继续扩展 Tailspin Toys，可选修 [Foundry Backer Concierge 系列课程][foundry]。若要比较其他工作环境，可查看 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
