---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub の両方を一元的に扱うデスクトップ アプリです。Issue やプルリクエストにすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、GitHub Copilot app を使い、Astro で構築された Tailspin Toys アプリをローカル環境で更新します。始める前に、ローカルに Node.js をインストールしてから Copilot app をインストールします。

このレッスンでは、次の操作を行います。

- プロジェクトのテストをコンピューターで実行できるように Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を構築させ、Tailspin Toys のテスト スイートをローカルで実行します。そのためには、プロジェクトに必要な唯一のランタイムである **[Node.js][nodejs]** が必要です。現在の **LTS** リリースをインストールしてください。

すべてのプラットフォームで、公式インストーラーを使うのが最も簡単です。

1. Windows Terminal、macOS のターミナル、または普段使用しているターミナルを開きます。
2. 次のコマンドを実行し、インストールされている Node.js のバージョンを確認します。

   ```shell
   node --version
   npm --version
   ```

3. 両方のコマンドが成功し、Node.js がプロジェクトの README と `package.json` に記載された要件を満たしていれば、次のセクションに進めます。

> [!TIP]
> Node.js がインストールされていない場合、または更新が必要な場合にのみ、これらの手順を実行してください。

4. [Node.js のダウンロード ページ][node-download]を開きます。
5. 使用しているオペレーティング システム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選択します。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナルを開きます。
8. 新しいターミナルで次を実行し、インストールを確認します。

   ```bash
   node --version
   npm --version
   ```

9. インストールされた Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot app のエージェントも `node --version` と `npm --version` を実行できる必要があります。普段使っているターミナルで成功しても、アプリから実行できるとは限りません。Node.js のインストール後や PATH の変更後は、アプリを完全に終了してから再起動してください。セッション内でいずれかのコマンドが引き続き失敗する場合は、続行する前に[セッション環境のトラブルシューティング手順][session-environment]を確認します。

> [!IMPORTANT]
> E2E チェックのため、各 worktree にプロジェクトの依存関係と Playwright Chromium も必要です。worktree を準備するときは Tailspin Toys リポジトリの README に従い、インストールの要求は内容を確認してから承認してください。

## ラボ用リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使って作業します。[テンプレート リポジトリ][template-repository]からコピーを作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれます。アプリのインストール時にこのリポジトリを接続します。

1. 新しいブラウザー ウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、自分用のリポジトリを作成します。

   ![ドロップダウンで Create a new repository が選択された Use this template ボタン](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![github-samples/tailspin-toys がテンプレートに設定され、リポジトリ名が入力された Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えてください。この後のラボで使います。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこの Issue を使うため、自分で登録する必要はありません。

ワークショップのテンプレートの新しいコピーを使ってください。リポジトリの指示、アプリケーション コード、テスト、`quality-checks` スキル、既存の Canvas 拡張機能が含まれています。ワークショップではスキルをカスタマイズし、QA エージェントを作成します。古いコピーを使う場合は、必要なファイルがあることをファシリテーターに確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次の操作を行いました。

- プロジェクトをコンピューターでビルドおよびテストできるよう、Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は[GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して、ワークスペースを確認します。

## リソース

- [Node.js をダウンロードする][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
