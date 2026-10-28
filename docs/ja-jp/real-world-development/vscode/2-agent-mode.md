---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストなどの操作が必要です。開発者は、必要なファイルを探し、変更し、テストを実行し、デバッグし、変更漏れに気づいて再度更新する、という作業を繰り返します。

このような作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、統合開発環境 (IDE) 内でより自律的に作業します。既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、問題が見つかれば自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じて連携します。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定して、複数ファイルを連携して変更します。
- GitHub Copilot Agent Mode でユーザーインターフェイス (UI) とデータレイヤーにまたがる機能を実装します。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリや発行元で絞り込めるようにします。前の演習で発行元 helper を追加しました。次は Copilot Agent Mode を使って、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者向けテーマのボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的な HyperText Markup Language (HTML) として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス helper は `src/lib/` にあり、インタラクティブな動作には標準 DOM API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクで Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後なので、`main` ブランチにいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルの Astro サーバーアドレス `http://localhost:4321` を選択してサイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace 上で実行中の Web サーバーに接続するためのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、次の機能を確認します。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作していますが、新しい機能を追加します。まずバックログを確認します。テンプレートからプロジェクトを設定すると Issue のバックログが自動作成されています。GitHub Copilot に項目を表示してもらいましょう。

1. Chat ビューのエージェント選択メニューから **Agent** を選びます。**Agent** は複数ファイルにわたる変更を自律的に計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、進めやすくすることを意図していますが、大規模言語モデル (LLM) は確率的に動作するため、ワークショップの例とは異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使うことがありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信して、リポジトリの Issue バックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue 一覧を確認します。

Copilot が、ユーザーに最も役立つと考えられる項目を優先順位付けしていることも確認できます。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを与える指示ファイルを確認します。Web サイトに機能を追加する方法が記載された [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などのファイル種別に関する指示へのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用しやすい小さな単位に分け、コード生成の例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つを更新します。

- データレイヤー (`src/lib/`) にフィルタリングのロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

コードベースにマージする前にテストを実行し、成功させる必要もあります。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを探して、データレイヤー helper、UI、テストを見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトを調べるスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースによっては数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** ボタンが表示される場合があります。完了するとすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat がテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーがないと判断するまで作業します。

   ![Copilot Agent Mode の完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode で Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる機能を実装します。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定して、複数ファイルを連携して変更します。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューします。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 - ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだ方法を使い、Copilot にサイトへのページネーション追加を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定値にするかを決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更やコンテキストを伝えながら反復する場合があります。Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
