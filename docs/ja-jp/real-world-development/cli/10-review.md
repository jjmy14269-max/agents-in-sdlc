---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルタリング Issue を見つけました。
- 星評価を追加し、Codespaces のポート転送を使ってブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーで得た根拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR に Agent Merge を使うことを承認しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使えるスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップには、更新した `main` からそれぞれ別のブランチで進める、2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と、評価がないことを示す明確な表示をゲームカードに追加します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして、QA プロファイルと関連テストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した書式で結果を報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加え、PR を作成する前の確認をまとめました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。QA を実行しても新しいテストが不要な場合、それが正しい結果であることもあります。マージを承認する前にコードと検証の根拠を確認し、変更後は影響を受ける根拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は構築前に方針を検討するとき、**Interactive** は変更を確認しながら進めるとき、**Autopilot** は範囲が明確なタスクを任せるときに使います。定型的な編集には高速なモデルを、複雑な作業には推論の強さを高めた、より高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように構築したいかを明確に説明すると、結果が大きく変わります。

## さらに確認すること

主要なワークフローを完了しました。ほかにも、次の CLI 機能を確認できます。

- `/review` でコードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることで使いこなせるようになります。製品コード、趣味のコード、長年作りたいと思いながら手をつけられなかった小さなアプリなどで活用してください。チームと学んだことを共有し、お互いの経験から学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境での作業と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
