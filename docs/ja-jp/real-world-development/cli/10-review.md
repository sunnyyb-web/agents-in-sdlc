---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の継続的なワークフローで GitHub Copilot CLI を使いました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター issue を見つけた。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージした。
- フィルター issue から始め、Plan モードで方法を決め、Autopilot モードで機能を構築し、Interactive モードでレビューした。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認した。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証の根拠を評価した。
- フィルター機能全体をレビューし、Agent Merge によるフィルター PR のマージを許可した。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュ コマンドを確認した。

## 公開した内容

このワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2 つの PR を行いました。

1. **星評価**: 既存の `starRating` と未評価の状態をゲーム カードに表示する。
2. **フィルターと品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを含める。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、これらの作業は 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザーの確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果を、要件とテスト カバレッジの確認とともにまとめて PR のレビューに使いました。

追加するテストは実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断されることもあります。コードと根拠をレビューしてからマージを許可し、変更後は影響する検証結果を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストやツールが、作業の結果に影響します。このワークショップでは、指示の更新、スキルのカスタマイズ、QA プロファイルの作成、MCP サーバーの設定を行いました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの概要だけでなく、実際の変更やツールの結果をレビューしてください。

タスクに合った**モードとモデル**を選びます。構築の前に方法を検討するときは **Plan**、対象を絞った変更を対話的に進めるときは **Interactive**、範囲の明確なタスクを任せるときは **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を作るのか、なぜ必要か、どのように実装するかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローを完了しました。Copilot CLI のほかの機能も確認してみてください。

- `/review` でコード レビュー エージェントに変更を分析してもらう。
- `/rubber-duck` で問題を説明し、別の視点を得る。
- `/fleet` で独立したサブタスクを並列に調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

ツールの活用を上達させるには、使い続けることが一番です。本番コード、趣味のコード、何年も作ろうと思いながら手を付けていなかった小さなアプリなどに利用してみてください。チームで学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境も確認する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
