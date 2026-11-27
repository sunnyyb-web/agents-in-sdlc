---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プル リクエストを作成します。星評価のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルター機能と品質ワークフローのすべてを 1 つの会話とブランチで進め、この PR にまとめます。

このレッスンでは、次の内容を学習します。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを理解する。
- 機能全体の変更と検証の根拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能のワークフローを通じて、Copilot を使って機能を計画、実装、検証しました。次は、マージの許可を開発者が管理しながら、PR の残りの作業を自動化します。

## Agent Merge の概要

**Agent Merge** は、プル リクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースなど、PR を妨げている問題を解決します。その後、リポジトリが許可していれば GitHub の自動マージを有効にします。

ここまでは、自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることもできます。Agent Merge を有効にする前に、作業をレビューして問題がないことを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューし、PR を作成して、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視し、すべて成功したらマージします。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリの設定、権限の不足を回避しません。処理がブロックされた場合は報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストの処理など、開発プロセスの一部を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを学習した。
- 機能全体の変更と検証の根拠を確認した。
- フィルター機能の PR を作成した。
- レビュー後にのみ Agent Merge を有効にし、PR のマージを確認した。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使う[便利な Copilot CLI スラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
