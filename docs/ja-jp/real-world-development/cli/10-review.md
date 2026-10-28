---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルタリング Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証証拠を評価しました。
- フィルター機能の変更全体をレビューし、フィルター機能の PR に Agent Merge を使用しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## 公開した変更

このワークショップには、更新済みの `main` からそれぞれ作成する 2 つの PR があります。

1. **星評価:** ゲームカードに既存の `starRating` と未評価状態を表示します。
2. **フィルター機能と品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルター機能の計画から PR 作成までは、同じ会話とブランチを使いました。作業を簡潔にするため、これらの変更を 1 つの PR にまとめています。

## 検証方法の違い

自動テスト、自分でのブラウザー確認、MCP 経由の Copilot によるブラウザー確認を組み合わせてコードを検証しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告しました。QA はこれらの結果を、要件とテストカバレッジのレビューにまとめました。

追加のテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証の証拠を確認し、変更後は関係する証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズを別の会話でも再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し行う作業を記述し、カスタムエージェントは専門の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に方針を検討するには **Plan**、範囲を絞った変更を対話しながら進めるには **Interactive**、範囲が明確な作業には **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

環境と同じくらい、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えると、出力が大きく変わります。

## さらに試す

主要なワークフローを終えました。Copilot CLI の次の機能も確認できます。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して処理します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。本番コード、趣味のコード、長い間作りたいと思っていた小さなアプリなどで活用してください。チームと学びを共有し、ドキュメントも確認しましょう。

ターミナルから Tailspin Toys をさらに拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も比べるには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI のコマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
