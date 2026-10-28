---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、解決されていないセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望など、技術的負債を抱えています。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を委任し、開発者はより大きな課題に集中できます。

このレッスンでは、Copilot cloud agent を使って次のことを確認します。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 範囲を明確に定めた Issue の重要性
- Copilot への Issue の割り当て

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを作成した開発者が、ドキュメントを十分に整備しませんでした。まず、アプリケーションのすべてのエクスポート関数に TypeScript Documentation (TSDoc) の doc comment を追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの別のゲームを関連ゲームとして表示し、閲覧を続けられるようにします。現時点では、ユーザー体験 (UX) の受け入れテストに使える実装があれば十分です。この作業はブロッカーですが、ほかに優先度の高い Issue があります。

このような作業は優先順位が下がりやすく、Copilot cloud agent に任せるのに適しています。cloud agent が非同期に作業している間、開発者はほかのタスクに集中し、後から結果をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境を準備してタスクを開始します。作業中または完了後に Copilot のセッションを確認できます。提案された解決方法のレビュー準備ができると、PR で開発者に通知します。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に任せれば考える必要がなくなるわけではありません。一見簡単な作業でも、詳しく見ると複雑さが明らかになります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習をすでに完了していれば `.github/copilot-instructions.md` を実際に確認しています。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認して、後でその影響を見られるようにします。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc の doc comment に関する要件に注目します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` に一致するファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成された PR に TSDoc の doc comment、TypeScript の規約、コメントヘッダーがあるか確認します。これらの指示ファイルが影響しているため、後の演習で PR をレビューするときにもう一度確認します。

## Copilot cloud agent の開発環境を準備する

コードを作成するには、通常、開発環境の準備とセットアップスクリプトの実行が必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェアエンジニアと同じように作業します。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用セットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を準備できます。このラボの流れを円滑にして学習機会を提供するため、ワークフローはあらかじめ設定されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションと初期データ投入が済んだ SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最低限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # セットアップ手順の一部としてリポジトリを clone する場合 (たとえば依存関係のインストール) は、`contents: read` 権限が必要です。clone しない場合は、手順の完了後に Copilot が自動的に clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリは Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションし、初期データを投入します。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローファイルと同じ形式ですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot が PR に取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、Copilot の作業を待たずにリポジトリの **Actions** タブから手動で実行し、ワークフローが成功するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古いか、ドキュメントが不足しています。このような技術的負債を放置すると、生産性が低下し、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメントの作成にも役立つため、cloud agent に任せるのに適した Issue です。バックグラウンドでドキュメントを作成してもらい、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選びます。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などになります。新しい PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot が先ほどのワークフローを実行して環境を準備し、プロジェクトを分析して、問題への取り組み方を決めているためです。

13. todo リストと、Copilot が実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が出るまで、ページの更新が必要な場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を考えて決定した後、コードを作成します。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的は、非同期に作業して開発者がほかのタスクに集中できるようにすることです。この機能を使い、もう 1 つのタスクも cloud agent に割り当てます。その間に、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザインチームが求める関連ゲームの表示は長時間かからないかもしれませんが、その間に別の作業を進められます。cloud agent に Issue を割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、最初の Issue のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![Copilot が作業中であることを示す eyes 絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に Issue を割り当てると、非同期で作業を依頼できます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などに使えます。

このレッスンでは、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確に定めた Issue の重要性を確認しました。
- Copilot に Issue を割り当てました。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用を確認します。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
