---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べて変更を加え、プルリクエストを作成する間、別の作業を続けられます。

このワークショップでは、まず cloud agent が従うカスタム指示を追加し、GitHub Issue を割り当てて実装を依頼します。カスタムエージェントをレビューして使い、エージェントのダッシュボードからセッションの進行を監視して方向を調整します。最後に、作成されたプルリクエストをレビューし、結果を改善します。

## レッスン

| レッスン | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加します |
| [2. Cloud agent][ex2] | 非同期エージェント | GitHub Issue を Copilot cloud agent に割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントをレビューして使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントセッションを監視し、方向を調整します |
| [5. 反復して改善する][ex5] | レビュー | プルリクエスト (PR) をレビューし、Copilot の作業を改善して次の手順を選びます |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、Model Context Protocol (MCP)、コードレビュー、Copilot CLI の機能が含まれています。

[student-plan]: https://github.com/education/students

> [!NOTE]
> Copilot cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free は対象外です。Copilot Business または Copilot Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> この環境では MCP を扱いません。Copilot で MCP サーバーを使用する方法は、[CLI 環境][cli]または [VS Code 環境][vscode]を確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
