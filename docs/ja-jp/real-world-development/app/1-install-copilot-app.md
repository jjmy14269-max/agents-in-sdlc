---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。すべての作業を自分で行うのではなく、複数のエージェントをそれぞれ独立したワークスペースで並行して動かし、作業を指示するワークフローに適しています。繰り返し作業の自動化にも使えます。Node.js のインストールとプロジェクトのコピーができたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次の操作を行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートで用意されたバックログなど、ワークスペースの使い方を確認します。
- クイックチャットを試して、アプリについて質問します。

## シナリオ

チームは、増え続けるバックログに対応するため、AI エージェントを導入しています。Copilot app では、Issue を選び、エージェントを実行し、変更をレビューし、プルリクエストをマージするまで、一か所から作業を指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise を使う場合、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディング ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って GitHub.com または GHE.com のアカウントでサインインします。データ所在地に対応した GitHub Enterprise Cloud を使う場合は **GitHub Enterprise Cloud** を選択し、求められたら `*.ghe.com` のホスト名を入力します。

   GitHub Enterprise Server を使う場合は、まずクラウド アカウントでオンボーディングを完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインイン ガイド][ghes-sign-in]に従い、ブラウザーでアプリを承認します。

5. オンボーディング中にリポジトリを選択できる場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択をスキップして、オンボーディング後に追加することもできます。
6. テーマを選ぶよう求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** の下にリポジトリが表示されているか確認します。すでに表示されている場合は、もう一度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を入力します。
9. サイドバーにリポジトリが表示されることを確認します。この画面では、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとめられ、個々のセッションは各リポジトリの下に表示されます。

## ワークスペースの使い方を確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには主に次の領域があります。

- **New** — ここから Copilot との新しいチャット セッションを開始できます。
- **Pull requests** — アプリの GitHub ネイティブ統合を通じてプルリクエストを表示します。アプリを離れずに PR の参照や絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** — 同じように Issue を表示します。Issue の参照や絞り込み、Issue からのセッション開始を行えます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。ToDo リストの管理、定期的なプロジェクト保守、その他の面倒な作業を任せるのに役立ちます。振り返りでは次のステップとして紹介しますが、別の演習にはしません。
- **Customize** — MCP サーバー、プラグイン、スキルなどを追加して、Copilot app に機能を追加します。Playwright MCP の設定に使います。
- **Chats** — ブランチや専用ワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを始めると、この項目が表示されます。表示されていない場合は、**Sessions** の横にある **+** を選択し、最初のチャットを始めます。
- **Sessions** — リポジトリ フォルダーと、その中のエージェント セッションをまとめます。`tailspin-toys` などのリポジトリ名を選択するとリポジトリ ビューが開き、リポジトリの下にあるセッションを選択すると、その会話に戻ります。セッションごとに独立したワークスペースを使うため、変更が競合せず、複数のセッションを同時に実行できます。星評価を追加するレッスンで、最初のセッションを開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法がわからない場合や、何かが可能かどうか知りたい場合は、Copilot に尋ねると案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認します。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリ フォルダーを選択します。次に、リポジトリ上部のナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue はエージェント セッションの開始点にもなります。最初の小さな変更を完了した後、フィルタリングの Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。ほかのリポジトリの作業項目を表示するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問するのが効果的です。その用途には**クイックチャット**が適しています。クイックチャットではブランチや worktree を作らずに質問やブレインストーミングができます。セッションを必要としない、すぐに終わる質問に最適です。

1. サイドバーの **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを始めます。その後、**Chats** が表示されます。
2. アプリのセッションの仕組みについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが独立した Git worktree で実行されるため、変更が競合せず複数のエージェントを並行して実行できることがわかります。会話はいつでも続けられ、新しいチャットも開始できます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次の操作を行いました。

- アプリをインストールして GitHub にサインインしました。
- GitHub リポジトリからプロジェクトを追加しました。
- ワークスペースの使い方を確認し、**Issues** で用意されたバックログを見つけました。
- クイックチャットを使って、その場限りの質問をしました。

次は[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app を使い始める][getting-started]
- [GitHub Copilot app でエージェント セッションを使う][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
