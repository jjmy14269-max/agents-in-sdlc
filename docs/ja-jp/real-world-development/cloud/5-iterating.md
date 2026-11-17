---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードへのドキュメント追加、デザイン チームが改善を重ねられる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝え、作業を改善してもらいましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。そこで、チームのメンバーにレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能の作成に関する GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督者なしでタスクを実行するため、安全性を保つためのセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権は作業に使用するブランチに**のみ**あります。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーなど、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードへのドキュメント追加に関する最初のプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。これは**想定どおり**です。Copilot は、外部 MCP サーバーへのアクセスなど、外部リソースへのアクセスが既定で制限されています。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだタスクを実行中の場合は、プルリクエストに **[WIP]** の表示があります。表示がある場合は、Copilot の作業が完了するまで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメント コメントなど、更新されたコードとドキュメントを確認します。正確な変更内容は状況によって異なります。

   変更を確認しながら、TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーを探します。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新をレビューし、問題がなければ **Conversation** タブに戻って下へスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。チーム メンバーに依頼するのと同じように、プルリクエストのコメントやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ずこう依頼すべきという文面はありません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの内容を簡単に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例や、コードベースに追加したいドキュメントのアイデアを使って、**@copilot** をメンションし、生成されたドキュメントの修正をコメントで依頼します。ほかのユーザーをメンションするのと同じ方法です。
2. **View Session** を選択して、Copilot が作業する様子を確認します。更新のために Copilot が新しいセッションを開始することに注目してください。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

問題がなければ、満足するまで改善を繰り返してください。その後、PR をドラフトからレビュー可能な状態に変更して、main ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する Issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新をレビューし、問題がなければ **Conversation** タブに戻って下へスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます)。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を実装しました。前と同じように、Copilot cloud agent に更新を依頼して、反復的に改善できます。たとえば、表示する関連ゲームの数の変更や、コメント ヘッダーと TSDoc ドキュメント コメントの追加を依頼できます (この Issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブでコメントを追加すると、Copilot が内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装したアクセシビリティ機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の Mission control で追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだタスクを実行中の場合は、プルリクエストに **[WIP]** の表示があります。その場合は、Copilot の作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント。
   - 利用者の設定を保存するためのローカル ストレージの使用。
   - ハイ コントラストとライト モードに関する CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モードの切り替えを管理する JavaScript/TypeScript コード。

6. 更新をレビューし、問題がなければ **Conversation** タブに戻って下へスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントによって、Copilot がアクセシビリティのベスト プラクティスに沿って機能を実装できたことを確認します。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログ項目を任せて別の作業に移れるときに効果を発揮します。習慣にするため、リポジトリに Issue を追加して Copilot に割り当ててみてください。たとえば、次のような作業があります。

- ゲームの詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力の検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンでは、次の操作を行いました。

- **このリポジトリに用意されたカスタム指示ファイルを確認し、cloud agent の出力への影響を確認しました。**
- **Issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。**
- **アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。**
- **Copilot agents ページを Mission control として使い、アクセシビリティ セッションを監視して作業中に指示を追加しました。**
- **cloud agent のプルリクエストをレビューして反復し、`@copilot` に変更を依頼してワークフローを承認しました。**

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、次のハーネスも役立ちます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリで、次のアイデアも試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てます。
- 別の分野のカスタム エージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
