---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、登録済みのフィルタリング Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールを使えます。タスクを任せたり、変更を依頼したりしながら、コマンド ラインから集中して作業できます。最初のステップとして、ツールをインストールします。すでに使い慣れているツールを使って簡単にインストールできます。

このレッスンでは、次の操作を行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、Copilot と簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI は、開発者が日常的に使うターミナルにその機能を提供します。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、このレッスンでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイス コードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Tailspin Toys リポジトリのパスであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 使い方を確認する

通常のシェル プロンプトに入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、スラッシュ コマンドで会話を操作します。

1. `/model` を入力します。矢印キーで **Auto** を選び、<kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力し、インストール済みのバージョンで利用できるコマンドを確認します。<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、正しく動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答する前にリポジトリを調べることを確認します。
5. `/mcp list` を実行し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. Copilot に、フィルタリングの Issue を見つけるよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開き、Issue の内容を読みます。最初の変更を行った後、この Issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使います。worktree を自動的に作成することはありません。変更を行う前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は既定の権限の範囲外でツールを使う前に確認を求めます。このワークショップでは、`--yolo` を使って再起動します。Codespace 内の承認プロンプトを省略できるため、演習に集中できます。

> [!CAUTION]
> `--yolo` は、すべての権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を自動的に許可します。Codespace や VM などの分離された環境でのみ使い、日常の開発で既定の設定にしないでください。詳細は[ツールの使用を許可または禁止する方法][allow-all-warning]を参照してください。

後のレッスンで Issue やプルリクエストを操作するため、`--enable-all-github-mcp-tools` を指定すると、読み取りと書き込みの GitHub MCP ツールが有効になります。Codespace からローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースには引き続きアクセスできます。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう 1 つ簡単な質問をし、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後に、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次の操作を行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの Issue を見つけました。

次は[最初の変更に取り組み][next-lesson]、Copilot CLI を使ってゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
