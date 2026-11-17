---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、実行してほしいことだけでなく、その方法も Copilot に伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスを対象にした指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメントの標準が Copilot にどのように渡されるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、この演習では指示を追加する前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ レイヤーには、必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルを対象にする指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。Copilot への多くのチャットや CLI リクエストに関係する、技術スタック、開発対象の概要、ベスト プラクティス、全体的なガイダンスなどを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類を対象に作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成やユニット テストの追加といったタスクのガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイルもサポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成に関する詳しい説明は、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法を示す例が含まれています。基本的には、次のようにします。

- `copilot-instructions.md` には、構築するものの説明、プロジェクトの構造、全体的なコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスクに関する指示に使います。
- 自然な言葉で、明確なガイダンスとコードの例を示します。

AI の使い方に決まった形がないのと同じように、指示ファイルの作り方にも決まった方法はありません。プロジェクトに合う方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトの指示を確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクに関するファイルがあることに気付くでしょう。
>
> Copilot に指示ファイルの作成を依頼することもできます。各環境での方法は異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。詳細が関係する場合は、その環境のレッスンで説明します。
>
> テンプレートや作成の出発点を探す場合は、指示ファイル、カスタム エージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを読みます。中心となる `copilot-instructions.md` が 1 つあり、さまざまなタスクに適用される `*.instructions.md` ファイルもあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** に関する指針もあります。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリ ルートからの glob で適用対象を決めることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. このプロジェクトのユニット テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にあるほかの指示ファイル (たとえば `unit-tests.instructions.md`) やプロジェクト内のファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、コードを生成する Copilot が参考にできる例を示せます。ここにあるパスはリポジトリ ルートではなく、指示ファイルを基準とした相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル先頭のコメント ヘッダーを追加します。

## リポジトリ全体に適用する標準を追加する

この演習で行う編集は 1 つです。コード内の TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーを必須にするルールを、プロジェクト全体に追加します。後続の演習で Issue を cloud agent に割り当てると、この指示が使われます。

編集する前に、作業ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、ここでは変更をきれいにコミットします。

1. Codespace のターミナルで新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるためのファイルです。リポジトリのコーディング標準を記述して、コードの提案を改善します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにはドキュメント コメントがありませんでした。
3. 既存の標準のすぐ下に、次の Markdown を追加して、ファイル コメント ヘッダーと TSDoc ドキュメント コメントを追加するよう Copilot に指示します。

   ```markdown
   - エクスポートする関数には、その目的、パラメーター、戻り値を説明する TSDoc コメントを記述します。
   - import 文やコードより前に、ファイルの目的を説明するコメント ブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは、全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイルの種類、タスクに使う `*.instructions.md` のどちらにも作成できます。今回追加したドキュメント コメントのルールのように、プロジェクト全体に適用する標準は、リポジトリ レベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作成されます。cloud agent が指示を読み取れるよう、変更を `main` にマージする必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。これで、次の演習で Issue に割り当てたとき、cloud agent が指示を読み取れるようになります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せず、`main` に直接コミットしてもかまいません。ここでは、実際のプロジェクトで一般的な方法に沿って作業するため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルを Copilot が参照する方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストに、この標準が反映されます。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot をカスタマイズする指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
