---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot に具体的なコンテキストを渡す方法はいくつかあります。特に重要なのが、コード生成のガイダンスを提供する[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- *現在の*指示を使って、フィルタリングに必要な最初のデータ部分 (publishers helper) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従うことを確認します。
- 指示の更新とフィルタリングの作業を `main` に commit し、次の演習で Copilot が最新のガイダンスを利用できるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習の目的は、指示を更新した後の動作の*傾向*を確認することであり、出力を 1 文字ずつ一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データレイヤーには必ずユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードにし、現代的な印象にします。
- コード内のドキュメントには TypeScript Documentation (TSDoc) の doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案を得られます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル種別に適用するタスク単位の指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中のものの概要、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクトレベルのコンテキストを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けの指示を記載します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに使えます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの標準に準拠した指示ファイル][custom-instructions-support]もサポートします。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく扱うことは、このワークショップの範囲外です。サンプルプロジェクトの例から、一般的な方法を確認します。

- `copilot-instructions.md` には、開発中のもの、プロジェクト構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスク向けの具体的な指示に使います。
- 自然言語で明確に記述し、望ましいコードと望ましくないコードの例を示します。

指示ファイルの作成方法にも、AI の使い方にも唯一の正解はありません。プロジェクトに合う方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意します。このプロジェクトにも、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルの作成を依頼することもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、方法は環境によって異なります。現在の環境で関係する方法は、各レッスンで案内します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された中核の `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルを、エディターまたは GitHub の Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。repo root からの相対 glob を指定します。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を小さく再利用しやすいファイルに分け、コード生成時に Copilot が従う例を示せます。ここに記載されたパスは repo root ではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイルコメントヘッダーの規則を追加します。

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に* Copilot Chat を使う

カスタム指示の効果を確認するため、まず現在の指示でプロンプトを送ります。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得できるようにします。
2. `src/lib/publishers.ts` を開き、helper の配置場所を Copilot に示します。
3. Chat ビューのエージェント選択メニューで **Agent** を選び、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、helper ファイルやテストなどのコードを更新する様子を確認します。
6. 提案された helper が、最初の引数に `db` クライアントを受け取り、型付きの publisher 配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc の doc comment とファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。この演習の要点は、指示を更新した後に一貫性が向上することです。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供するファイルです。コード提案の質を高めるため、リポジトリのコーディング標準を文書化します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存のコーディング標準を確認し、コード内ドキュメントの規則がまだないことを確認します。
3. 既存の標準の直後に、ファイルコメントヘッダーと TSDoc の doc comment を追加するよう Copilot に指示する次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは全体的なガイダンスを記載するリポジトリレベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。doc comment の規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

指示に doc comment の規則を追加したため、Copilot Chat で同じプロンプトを再送信します。

1. Copilot Chat に戻り、**New Chat** を選んでバッファーをクリアします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot の対象ファイルにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

次の演習で使うため、更新されたファイルはそのまま残します。

## 指示とブランチを commit して push する

指示ファイルも、ほかのリポジトリアセットと同じようにソース管理します。変更を commit してブランチをリポジトリに push します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、指示と helper の変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と helper の基盤、および Copilot が helper のために変更したすべてのファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (たとえばテスト) がある場合は、そのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したら、バックログのドキュメント標準に関する Issue に関連付けてプルリクエストを作成します。GitHub ツールはすでにプロジェクトに接続されているため、Issue を探して PR を作成し、マージするよう Copilot に依頼できます。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼する最初の演習です。プロジェクトテンプレートでは GitHub のツールがワークスペースに設定済みなので、追加のセットアップは不要です。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインが求められる場合があります。案内に従って許可してください。後の演習では、この接続に使われる Model Context Protocol について学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新と、今後のフィルタリング作業に使う publishers helper の基盤を説明し、現在のブランチから PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索や操作を行う際、必要に応じて **Continue** を選びます。GitHub ツールの初回使用時に認証を求められた場合は、案内に従います。
5. Copilot が Issue を探し、PR を作成する様子を確認します。
6. PR が作成されたら、次のプロンプトでマージしてローカルのブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドの実行を Copilot に許可するよう求められた場合は、操作内容を確認してから **Allow** を選びます。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- *既存の*指示でコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 指示の更新と helper の基盤を `main` に commit し、次の演習で使えるようにしました。

次は[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体へ新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のよりよいカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
