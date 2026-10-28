---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づくフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。より大きな変更には、より確かなプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って適切なものを適切な方法で構築できるよう設計されています。このレッスンから、Issue を起点に機能を実装し、コードの妥当性と機能の動作を確認して、プロジェクトにマージするまでの、エージェントを使った一般的な開発プロセスを進めます。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい Copilot の会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで方針を決めたうえで、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディングエージェントを開発フローに加えても、基本は変わりません。むしろ、基本を守ることが一層重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を記述した Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーション (CI) が成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、ほとんどの場合は上記の流れを基にしています。

この標準的な進め方に従うことで、AI が生成したコードも要件を満たし、手書きのコードと同じレビューを受けられます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: 開発者とエージェントが協力して作業します。エージェントは変更案を提示し、続行前に入力を待ちます。
- **Plan**: エージェントは最初に計画を作成し、プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントはコードを書き、テストを実行し、タスクが完了したと判断するまで自律的に反復します。

まず Plan モードで計画を作成してレビューし、その後 Autopilot で実装します。

## Issue から始める

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って、**Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認してください。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) を会話のコンテキストに追加できます。

## フィルタリング機能を計画する

Copilot に機能やタスクを任せる前に、実装方針を決めるための計画を立てます。複雑な作業では、時間をかけて計画するのが有効です。Plan モードに切り替え、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望する動作に基づいて回答します。回答に唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して、計画を展開します。
5. 計画を上下にスクロールしてレビューします。
6. 決めた方針と一致しない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。Autopilot では、Copilot が完了したと判断するまでタスクを反復できます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様の名前が付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があるため、先に計画をレビューしてください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードも、ほかのコードと同様にマージ前のレビューが必要です。コードを確認し、サイトを実行して動作を確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画時に決めた内容と照らし合わせます。
4. レビューが終わったら、<kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を確認する

コードがよさそうでも、実際に動くことを確認する必要があります。前回と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot に次のプロンプトを送ってアプリを起動します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めた内容に従って、カテゴリ、発行元、およびその組み合わせで絞り込みます。
4. リセット時と結果がない場合の動作が、Issue と決定内容に一致することを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って、機能を構築してレビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から新しい Copilot の会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、コードが文書化されたプラクティスに従うよう、[カスタム指示を使って][next-lesson]コード生成の進め方を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI のコマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
