---
title: "演習 2 - Copilot agent mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなど、さまざまな作業が必要です。開発者は、必要なファイルを探し、変更を加え、テストを実行し、デバッグし、変更が漏れたファイルを見つけて、さらに更新するという流れで作業します。作業は次々に続きます。

ここで役立つのが Copilot agent mode です。

Copilot agent mode は IDE でより自律的に作業します。開発者と同様に、まず既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。agent mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。Agent mode は IDE 内で作業するため、短いフィードバック サイクルで対話できます。Cloud agent はチーム メンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次の操作を行います。

- Copilot agent mode がプロジェクトを調べ、関連ファイルを特定して、複数のファイルにわたる変更を行う方法を確認します。
- GitHub Copilot agent mode で UI とデータ レイヤーに新機能を実装します。
- コードベースにマージする前に、Copilot agent mode が生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリやパブリッシャーで絞り込めるようにします。前の演習でパブリッシャーヘルパーを追加しました。次は GitHub Copilot agent mode を使って、データレイヤー、UI、テストの残りを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者向けテーマのボードゲーム クラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。サイトは単一の Astro アプリで、ビルド時に静的 HTML としてページを生成します。ページは Drizzle ORM を使ってフロントマターからローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、必要なインタラクティブ機能は標準 DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を行います。

1. Codespace に戻ります。前の演習の後なので、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーをホストしている Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなど、詳しい情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトをセットアップしたときに、Issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント選択メニューから **Agent** を選択します。**Agent** は自律的に変更を計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順に含めています。コードの提案がなるべく近くなることを期待していますが、LLM の動作は確率的です。ワークショップの例とは異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の動作は確率的なため、Copilot が別の MCP コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が利用者にとって役立つ度合いに基づいて、項目に優先順位を付けたことを確認します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを伝えるために使う指示ファイルを確認します。Web サイトに機能を追加する方法に関するコンテキストを含む[ユーザー インターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加する方法に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - ファイルの種類ごとの指示ファイルへのリンク。対象には次のものがあります。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、複雑な内容を小さく分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の3つをアプリケーションで更新します。

- データレイヤー (`src/lib/`) に、残りのフィルタリングロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功することを確認します。Copilot agent mode にこれらのタスクを任せます。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関連するファイルを見つけてから作業を始める様子を確認します。データ レイヤーのヘルパー、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを確認する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行します。タスクやコードベースの複雑さによっては、数分かかる場合があります。作業中にコード エディターで **Keep** と **Undo** が表示される場合があります。作業が完了すると、すべての変更に対して **Keep** または **Undo** を選択できます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが見つからなくなるまで作業します。

   ![Copilot agent mode の Chat セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動しているブラウザーに戻り、新しい機能を試します。
11. すべてが動作し、コードのレビューも終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot agent mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot agent mode で UI とデータ レイヤーに新機能を実装する方法。
- Copilot agent mode がプロジェクトを調べ、関連ファイルを特定して、複数ファイルにわたる変更を行う方法。
- コードベースにマージする前に、Copilot agent mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### ボーナスの演習 — ページ分割を実装する

ゲーム一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilot にサイトへのページ分割機能の実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ページ分割の方法を検討します。ページ サイズを利用者が選択できるようにするか、固定値にするか決めます。
- 希望どおりの実装を作成できるよう、必要なガイダンスをプロンプトに含めます。
- GitHub Copilot にコンテキストを伝えて変更を依頼し、反復する必要がある場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
