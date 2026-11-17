---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、必要に応じた cloud agent への委任に使えるスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが数多くあります。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストをどう管理するかを確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローを完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、任意の [Copilot cloud agent][about-cloud-agent] への委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

規模の大きいタスクや複雑なタスクでは、モデルのコンテキストウィンドウの上限に達する場合があります。必要に応じて Copilot CLI が自動的に会話を圧縮します。また、スラッシュコマンドを使って、コンテキストを確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI がまだ開いていない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように割り当てられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行して、結果を比較します。会話がすでに短い場合、大きな違いはないことがあります。

> [!NOTE]
> コンテキストウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。関係のない別のタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意なことが異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示し、使用するモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択します。または <kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールをより効果的に使えるようになります。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を実行し、インストール済みのバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown にエクスポートする `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を、誰かに送信したり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の拡大や委任

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに割り当て、並列に実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期に作業し、プルリクエストを作成する場合もある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を作成する場合があるため、任意の操作です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業をさらに確認する場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、CLI の構成、セッションの共有、内部動作の確認ができます。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストをどう管理するかを確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI にはほかにもスラッシュ コマンドがあり、学べることがたくさんあります。最後に、[学んだことと次のステップを振り返りましょう][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
