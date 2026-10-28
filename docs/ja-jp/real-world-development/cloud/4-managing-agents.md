---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザー体験の改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。この mission control では、オープンなプルリクエスト (PR) があるエージェントタスクを確認できます。作業内容を調べ、進行中のセッションに追加の指示を出すこともできます。

このレッスンでは、次のことを行います。

- エージェントページを開き、cloud agent のタスクを監視します。
- 進行中のセッションに指示を出し、機能を追加で依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加することにしました。サイトのスタイルを更新し、切り替え機能を追加しているため、同じ作業に含めるのが合理的です。ハイコントラストに加えてライトモードも実装されるよう、エージェントの作業に追加の指示を出します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスク一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントの更新
   - ゲーム変更用 API の生成
   - Web サイトへのハイコントラストモードの追加
3. 実行中のタスクを選択し、Copilot が実施した作業を確認します。次のような手順が含まれる場合があります。
   - リポジトリからのコードのチェックアウト
   - Copilot が作業する環境の作成
   - MCP サーバーのセットアップ
   - 割り当てられたタスクを完了するための各種作業

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、作業をさらに監視できます。

## cloud agent に追加の指示を出す

進行中のタスクを確認したので、ハイコントラストモードを作成中の Copilot にライトモードの切り替えも依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。表示されるタイトルは Copilot が付けた名前や作業状況によって異なります。

   ![mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページの cloud agent タスクで、作業中のセッションに指示するダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業に取り込むことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中の手順が完了すると、新しい依頼をワークフローに取り込みます。

作業には数分かかるため、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して案内する中心的な場所である Copilot agents ページを確認しました。mission control から進行中と完了済みのタスクを追跡し、実行された作業をレビューし、進行中のタスクに追加の指示を出して範囲を調整できます。

このレッスンでは、次のことを行いました。

- mission control とエージェントページを使って cloud agent のタスクを監視しました。
- 進行中のセッションに追加の指示を出して機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
