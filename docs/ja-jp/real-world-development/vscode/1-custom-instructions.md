---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。中でも重要なのは、コード生成のガイダンスを伝える[指示ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスを対象にした指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメントの標準が Copilot にどのように渡されるかを確認します。
- 現在の指示を使って、フィルタリングに必要な最初のデータ部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認します。
- 指示の更新とフィルタリングの最初の部分を `main` にコミットし、次の演習で更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが、指定した標準の一部に従わない場合があります。Copilot の動作は確定的ではありません。この演習では、更新後の指示によって動作にどのような傾向が生まれるかを確認します。生成結果が文字単位ですべて一致する必要はありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ レイヤーには、必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得られます。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルを対象にする指示も作成できます。

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
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にあるほかの指示ファイル (たとえば `unit-tests.instructions.md`) やプロジェクト内のファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、Copilot がコードを生成するときに参考にできる例を示せます。ここにあるパスはリポジトリ ルートではなく、指示ファイルを基準とした相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル先頭のコメント ヘッダーを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブをすべて閉じ、Copilot に必要なコンテキストだけを渡します。
2. Copilot にヘルパーの配置先を示すため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント選択メニューから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べて、コードを更新します。ヘルパー ファイルとテストなど、複数のファイルが変更される場合があります。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取る型付き関数であり、発行元の型付き配列を返すことを確認します。これは、`src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確定的ではないため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。この演習では、指示の更新後に一貫性が改善されることを確認します。

## リポジトリ全体に適用する標準を追加する

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

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys のクラウドファンディング プラットフォーム用の発行元データ アクセス ヘルパーです。
    * データベースから発行元の情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * すべての発行元の ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns 発行元オブジェクトの配列に解決される Promise。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使うコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルはリポジトリのほかのファイルと同じように、ソース管理で管理します。指示ファイルをコミットし、ブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次を実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新と、ヘルパーの基盤作成のために Copilot が変更したファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなど追加の関連変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けてプルリクエストを作成します。手動で行う代わりに、プロジェクトに接続済みの GitHub ツールを使って Copilot に任せましょう。Issue を見つけ、Issue をクローズする PR を作成して、マージを依頼します。

> [!NOTE]
> この演習では、Copilot が GitHub に対して操作を行います。プロジェクトのテンプレートでは、GitHub のツールがあらかじめワークスペースに接続されています。そのため、セットアップは不要です。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。指示に従って認証してください。後の演習では、この接続に使われる Model Context Protocol について説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を見つけ、現在のブランチから PR を作成し、指示の更新と発行元ヘルパーの基盤作成の両方を説明するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索し、PR の作成を始めます。
5. Copilot に GitHub 操作を許可するよう求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ブランチを `main` に戻します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作とシェル コマンドの実行を許可するよう求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援を使って、プルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- コード生成プロンプトを送信し、**既存の**指示を使った Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[agent mode][next-lesson]を使い、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot をカスタマイズする指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
