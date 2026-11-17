---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、手間のかかるタスクを任せて開発を加速し、開発者はより大きな課題に集中できます。

このレッスンでは、Copilot cloud agent を使って次の操作を行います。

- コード生成環境をカスタマイズします。
- 操作が安全に行われるようにします。
- Issue の範囲を明確に定義することの重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には、対応したい技術的負債があります。最初のサイトを開発した委託先がドキュメントを残していません。つまり、ドキュメントがまったくありません。まず、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメント コメントを追加したいと考えています。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを関連ゲームとして表示し、利用者が引き続きゲームを見つけられるようにします。現時点では、UX の受け入れテストに使える簡単な実装があれば十分です。この作業はブロッカーですが、今は優先度の高いほかの Issue があります。

どちらも、優先度が下がりやすい作業の例です。Copilot cloud agent に割り当てると、開発者がほかのタスクに集中している間、バックグラウンドで作業させることができます。その後、戻ってきて Copilot の作業をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境をセットアップして、タスクに取り組み始めます。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、プルリクエストでメンションして知らせます。

## 範囲が明確な指示の重要性

GitHub Copilot に魔法のような力はありません。考える手間を省けるわけではありません。一見簡単そうな作業でも、詳しく見ると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示とその役割を確認しました。カスタム指示の演習をすでに終えている場合は、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、ここで簡単に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを参照し、後でその影響を確認します。

リポジトリの GitHub Web UI で次のファイルを開くか、起動済みの Codespace で開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成されるプルリクエストで TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーを確認してください。後の演習で PR をレビューするときに、改めて取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードの作成では、誰が作業する場合でも、通常は環境やセットアップが必要です。必要なツールと依存関係を使えるようにします。Copilot にタスクを割り当てる場合も、ソフトウェア エンジニアのように作業するため、同様の準備が必要です。

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に実行するツールや依存関係を用意できます。このラボでは、学習に集中できるよう、あらかじめ構成されています。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションと初期データ投入済みの SQLite データベースを Copilot が使えるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Copilot は操作用のトークンを別に受け取るため、セットアップに必要な最小限の権限を設定します。
    permissions:
      # たとえば依存関係のインストール時に、セットアップ手順でリポジトリをクローンする場合は `contents: read` が必要です。セットアップ手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリは Astro + Drizzle/libSQL に移行済み)
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

      # ビルドとテスト用に、ローカル SQLite データベースをマイグレーションして初期データを投入します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフロー ファイルと同じ構成ですが、次の点を確認してください。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストで作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、まったくなかったりします。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、cloud agent に Issue を割り当てるのに適しています。バックグラウンドで必要なドキュメントを作成してもらい、後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が作成した ToDo リストを確認します。

> [!NOTE]
> Copilot が PR に ToDo リストを追加するまで、数分かかる場合があります。前に説明したワークフローの実行、プロジェクトの分析、取り組む方法の検討を行うためです。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 状態が更新されていない場合は、ウィンドウを更新してください。

16. ライブ セッションをスクロールして、Copilot が問題にどう取り組んでいるかを確認します。コードを調べて状態を理解し、考えながら適切な計画を決めて、コードを作成する様子がわかります。

この作業には数分かかる場合があります。Copilot cloud agent の主な利点は、作業を非同期に実行し、その間に別の作業を進められることです。この利点を活かすため、もう 1 つのタスクを cloud agent に割り当ててから、アプリへの機能追加に取り組みます。

## ゲームの詳細ページに関連ゲームを追加する

前に説明したように、Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
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
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに、Copilot が作業中であることを示す 👀 が表示されます。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組み始めました。Copilot cloud agent はソフトウェア エンジニアと同じように作業するので、常に監視する必要はありません。作業が完了したらレビューできます。次は、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期に作業を進められます。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などに利用できます。

このレッスンでは、次の操作を行いました。

- コード生成環境をカスタマイズしました。
- 操作が安全に行われるようにしました。
- Issue の範囲を明確に定義することの重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
