---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルタリング機能を構築しました。プルリクエストを開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開し、PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルタリング機能を手動テストしてもらいます。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にある機能を利用すると、さらに便利になります。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準的な方法です。

![Model Context Protocol (MCP) サーバーや VS Code 拡張機能が提供するツールを含め、エージェントモードがコンテキスト、大規模言語モデル (LLM)、ツールと連携する仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信する方法を提供します。MCP を使うと、エージェントは外部サービスとリアルタイムに通信し、リソースから最新情報を取得したり、ツールを使って開発者に代わって操作したりできます。

これらのツールやリソースには、AI エージェントと外部サービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、既存のアプリケーション プログラミング インターフェイス (API) や npm パッケージのようなローカルツールなど、AI エージェントと外部ツールとの通信を管理します。各サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。AI エージェントは Web ページへの移動、フォームへの入力、ボタンの選択などを行えます。

ほかにもさまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、コントリビューションを促すために [MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> セキュリティの観点では、MCP サーバーをプロジェクト内のほかの依存関係と同様に扱ってください。使用前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は慎重に判断してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースが利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに次の `github` サーバーが設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` の設定はプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はなく、VS Code が HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、機能のテストに必要な、Copilot が操作できるブラウザーを提供します。

1. `.vscode/mcp.json` で `github` と並べて `playwright` の設定を追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、画面を表示せずにブラウザーを実行します。デスクトップ環境のない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンドテストに Playwright を使うため、Playwright が必要とするブラウザーは通常すでにインストールされています。Copilot が後でブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` 設定の上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか尋ねられた場合は、構成を確認し、サーバーを起動するために信頼する選択肢を選びます。

手動で起動する操作は省略できます。その場合、次の手順で Copilot がツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。開発者がアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って要約します。会話を離れずに機能を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目などの構造化されたラベル付き要素を確認します。そのため、簡単な機能チェックがアクセシビリティの簡易確認にもなります。

サーバーを接続し、アプリを起動したら、次のプロンプトを Copilot に送ってフィルタリング機能を確認します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。報告内容を Issue の受け入れ条件と照らし合わせます。問題があれば、追加で質問するか、プルリクエストを開く前にコードの修正を依頼します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、プロンプト送信前に再起動します。Copilot が Playwright MCP サーバーを初めて使う際、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認できたので、チームがレビューできるようプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを使って commit メッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリに push します。

## プルリクエストを作成する

GitHub.com や GitHub コマンドライン インターフェイス (CLI) など、PR の作成方法はいくつかあります。GitHub Copilot を使っているので、関連 Issue を探して紐付けた PR を作成するよう依頼します。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. エージェント選択メニューで **Agent** が選ばれていることを確認し、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の取得や操作を Copilot に許可するため、必要に応じて **Continue** を選びます。GitHub ツールの初回使用時に認証を求められた場合は、案内に従います。
5. Copilot が Issue を探し、適切なものを見つけて PR を作成する様子を確認します。
6. Copilot が表示したリンクを選び、プルリクエストをレビューします。**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Copilot にブラウザーでフィルタリング機能を手動テストしてもらいました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

機能の*テスト*に MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つです。次はアクセシビリティレビューなどの作業を効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
