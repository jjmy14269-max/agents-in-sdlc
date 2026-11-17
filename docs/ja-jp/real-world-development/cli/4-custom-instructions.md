---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント作成の基準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルタリング機能を構築したので、Copilot が使った指示を確認し、ドキュメント作成の基準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスを対象にした指示ファイルが、エージェントにどのように渡されるかを確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントには、コードの繰り返しではなく、目的や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数についても記載します。
- 再利用可能な Astro コンポーネントでは、`Props` の契約を説明し、関連するコードの変更時にはコメントを最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定のファイルを対象にする指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、またはタスクに関するガイドラインを指定します。

> [!NOTE]
> その他の指示形式への対応状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

プロジェクトには、作業を始めやすいよう指示ファイルが用意されています。変更を加える前に、すでにある指示を確認して、その影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、リポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示の適用対象を決める glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。大きな指示セットを小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

すでにあるファイルはよい出発点ですが、追加できるガイダンスがあります。`copilot-instructions.md` の中核ファイルを変更し、新しく生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` を開きます。
2. 35 行目付近にある **Code formatting guidance** セクションを探します。
3. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を適用するには、機能のコンテキストを保ったまま会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を実行して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行し、変更された TypeScript ファイルを確認します。新しく生成された TSDoc コメントがあり、内容を正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がカスタム指示を参照する方法を確認し、機能にチームの基準を適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの指示とパスを対象にした指示ファイルが、エージェントにどのように渡されるかを確認しました。
- コーディング標準に従うよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は再利用可能な `quality-checks` スキルをカスタマイズして実行し、lint とテストが一貫して実行されるようにします。[スキルをカスタマイズする][next-lesson]に進みます。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
