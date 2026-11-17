---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習では、ユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、これらのタスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。この Mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行中の操作を調べたり、セッションの進行中に方針を伝えたりできます。

このレッスンでは、次の操作を行います。

- エージェント ページを使って cloud agent のタスクを監視します。
- 実行中のセッションに指示を追加し、機能を増やすよう依頼します。

## シナリオ

エージェントにハイ コントラスト モードの作成を依頼した後、チームはライト モードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能を実装しているので、ライト モードも追加するのが適切です。ハイ コントラスト モードだけでなくライト モードも実装するよう、エージェントに指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方に表示されるタスクの一覧を確認します。たとえば、Copilot に割り当てた次のタスクがあります。
   - コードベースのドキュメントを更新する。
   - ゲームを変更するための API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを 1 つ選択し、Copilot が行った操作を確認します。次のような操作があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するための各種作業を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、追加の監視に使える PR と変更ファイルを確認できます。

## cloud agent の作業方針を調整する

実行中のタスクを確認できたので、Copilot がハイ コントラスト モードを実装している間にライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。正確なタイトルは Copilot が付けた名前と現在の作業状況によって異なります。

   ![Mission control に表示されたアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで、数分間セッションを確認します。次のようなメッセージが表示されたら作業を始めています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを追加します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot が作業中のエージェント タスクと、Steer active session while Copilot is working ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業手順に追加したことを確認します。

## Copilot の作業を待つ

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順が終わると、新しい依頼を作業の流れに反映します。

今回も完了まで数分かかります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、方針を伝える中心的な場所である Copilot agents ページを確認しました。この Mission control を使うと、実行中や完了したタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を出したり、方針を変更したりできます。

このレッスンでは、次の操作を行いました。

- Mission control とエージェント ページを使って、cloud agent のタスクを監視しました。
- 実行中のセッションに指示を追加し、機能を増やすよう依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
