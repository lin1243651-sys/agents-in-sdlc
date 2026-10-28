---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験を改善して機能を追加する複数のタスクについて GitHub Copilot と作業しました。コードへのドキュメント追加、デザインチームが改善できる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝えて改善します。

### シナリオ

生成 AI を加えても、ソフトウェア設計と開発・運用 (DevOps) の基本は変わりません。生成コードをレビューし、通常の DevOps プロセスに沿って作業します。チームメンバーがレビューを開始する前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監督なしで非同期にタスクを実行するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスだけを持ち、書き込みできるのは作業に使うブランチ**のみ**です。
- cloud agent は GitHub Actions 内で動作し、作業用の一時的な独立環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- Model Context Protocol (MCP) サーバーを含む[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が最初に作成した、コードへのドキュメント追加に関するプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が付きます。その場合は作業完了まで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc の doc comment など、更新されたコードを確認します。変更内容は環境によって異なります。

   変更を確認するときは、演習 2 の最初に見た指示ファイルによる TSDoc の doc comment、TypeScript の規約、コメントヘッダーに注目してください。

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する操作](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに入ったことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する方法は、レビューを受けるだけではありません。チームメンバーと同様に PR のコメントやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確率的に変わるため、決まった依頼文はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を説明するコメントヘッダーを追加します。
- TypeScript と Astro ファイルに TSDoc の doc comment を追加します。
- Astro アプリの構造を説明する README を作成します。

1. 上記の例などを使い、生成されたドキュメントへの変更を依頼するコメントを追加します。ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択すると PR に戻れます。

   ![プルリクエストに戻る操作](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了したら、PR に新しい commit が追加されていることを確認します。
5. **Files changed** タブを選び、変更をレビューします。

納得できるまで改善を繰り返せます。問題がなければ、PR をドラフトからレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選び、生成されたコードをレビューします。
5. 変更内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があることを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する操作](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローがキューに入ったことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動テストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前の PR と同様に、Copilot cloud agent にコメントして変更を依頼し、反復して改善できます。たとえば、表示する関連ゲームの数を変更したり、コメントヘッダーや TSDoc の doc comment を追加したりできます。この Issue を割り当てたのはカスタム指示を更新する前だったことに注意してください。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で mission control から追加依頼したライトモードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が付きます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選び、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保存する local storage の使い方
   - ハイコントラストモードとライトモードの Cascading Style Sheets (CSS) またはスタイルの変更
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作、セマンティックな HyperText Markup Language (HTML) など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する操作](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに入ったことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動テストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ブラウザーでハイコントラストとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の作業を導いたことを確認できます。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションして変更を依頼します。

## 任意の演習 - さらに作業を委任する

cloud agent に実際のバックログ項目を割り当て、別の作業に集中すると、委任の習慣を身につけられます。リポジトリに Issue を追加して Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向けの関心フォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセス helper に入力検証とエラー処理を追加します。

## まとめ

Cloud agent 環境のワークショップを完了しました。各レッスンを通じて、次のことを行いました。

- このリポジトリに用意されたカスタム指示ファイルを確認し、cloud agent の出力への影響を確認しました。
- Copilot cloud agent に Issue を割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、進行中のアクセシビリティセッションを監視して指示しました。
- cloud agent のプルリクエストをレビューし、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent 環境のワークショップを完了しました。さらに確認したい場合は、次の環境でこれまでの内容を応用できます。

- 🖥️ **[VS Code 環境](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP の連携を確認します。
- 💻 **[CLI 環境](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルからレビュー済みの 2 つの変更を公開します。

自分のリポジトリでは、次のアイデアを試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野向けにカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
