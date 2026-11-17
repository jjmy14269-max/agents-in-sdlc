---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成してエージェントを定義し、対象の作業を効果的に行うための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に伝えます。チームの専門知識を再利用可能なエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディングのプラクティスに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントを作成できます。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダーか、グローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown のプロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割に重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を伝えます。簡潔に区別すると、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整するように設計されています。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに照らしてサイトをレビューし、更新します。その作業の一部として、プルリクエスト ワークフロー用のスキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI のほかの機能と同様に、環境やシナリオに合わせて試し、調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次の操作を行います。

- カスタム エージェントの定義方法を確認します。
- カスタム エージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを使えるようにしたいと考えています。最近の利用者からのフィードバックによると、テキストと背景色のコントラストが十分でないため、現在のダーク テーマが読みにくいと感じる人がいます。このアクセシビリティの問題に対応するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。機能の生成には、カスタム エージェントを使います。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、すでに作成されています。Copilot をどのように導くか理解するため、内容を確認しましょう。

Codespace に戻り、アクセシビリティ カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の項目を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な役割。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、作業中のセッションに追加のガイダンスを伝えたり、方針を変更したりできます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

   ![cloud agent の割り当て画面。カスタム エージェントの一覧と Accessibility agent が強調表示されています。](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである、GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot が特定の種類の作業をより効果的に行えるようにできます。

このレッスンでは、次の操作を行いました。

- カスタム エージェントの定義方法を確認しました。
- カスタム エージェントにタスクを割り当てました。

Copilot がハイ コントラスト モードを実装している間に、次は Mission control から[エージェント セッションを監視し、方針を調整します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備をする][org-custom-agents]
- [Enterprise でカスタム エージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
