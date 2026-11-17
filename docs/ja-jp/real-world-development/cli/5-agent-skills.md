---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、結果レポートの形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、開発にはさまざまな作業があります。これまで、コードを手動で検証し、指示ファイルを使ってコーディング標準に従うようにしてきました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうした作業には、**エージェント スキル**が適しています。スキルを使うと、Copilot が作業の実行方法を理解できます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の書式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースがまとめられており、エージェントは必要に応じて読み込みます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown ファイルには、スキルの内容を定義する `name` と `description` を含むフロントマター、概要、呼び出すタイミングのガイダンスを記述します。フォルダーには、呼び出されたスキルが使用するスクリプトやほかのリソース用のサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにあるコマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有して再利用できるリポジトリ資産としてプロジェクトの `.github/skills` フォルダーに配置できます。または、ユーザー スキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。`description` は、Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI では、スキルを直接呼び出すことも、自然言語で依頼することもできます。3 種類のチェックを実行するよう Copilot に依頼します。

1. Interactive モードで、フィルタリングの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、成功したかどうか、ツールが報告した内容を含む、よりわかりやすいレポートにします。スキルを更新して、その形式でレポートが作成されるようにしましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを実行してみましょう。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、次の操作を行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の書式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能の PR に含めます。次は [Playwright MCP サーバーを使って][next-lesson]、Copilot がサイトを直接操作できるようにします。

## スキルのその他の例

以下のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
