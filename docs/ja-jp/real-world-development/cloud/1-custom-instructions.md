---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に求める作業だけでなく、その進め方も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、ここではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムで並べて効果を示すのが難しいためです。cloud agent が作成するプルリクエストをこの環境の後半でレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
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
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意します。このプロジェクトにも、[ユーザーインターフェイス (UI) の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
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

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、TSDoc の doc comment とファイルレベルのコメントヘッダーをコードに記載するプロジェクト全体の規則です。cloud agent は、この後の演習で Issue に取り組む際にこの指示を読み込みます。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み込みますが、変更を整理して commit します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` にコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準を確認し、コード内ドキュメントの規則がまだないことを確認します。
3. 既存の標準の直後に、ファイルコメントヘッダーと TSDoc の doc comment を追加するよう Copilot に指示する次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のセクションで確認したように、指示ファイルは全体的なガイダンスを記載するリポジトリレベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。doc comment の規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の更新を commit、push、マージする

cloud agent は、Issue が対象とするブランチの指示ファイルを読み込みます。次の演習で Issue を割り当てると、Copilot は `main` からブランチを作成します。cloud agent が更新した指示を読み込めるよう、変更を `main` にマージする必要があります。

1. 更新を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. GitHub.com で `custom-instructions-cloud` から `main` に向けてプルリクエストを作成し、マージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業することもできます。その場合はブランチを省略して `main` に直接 commit します。ここでは実際のプロジェクトの進め方に近づけるため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。cloud agent がこの環境で作成するプルリクエストで、その標準が使われることを確認します。

次は[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のよりよいカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
