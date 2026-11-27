---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。次は、その機能をカスタム エージェントにまとめて、明確に定義した QA プロセスを作成します。

このレッスンでは、次の内容を学習します。

- カスタム エージェントが指示、スキル、MCP ツールと連携する仕組みを確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選び、フィルター機能の issue と照合した結果を確認する。

## シナリオ

Tailspin Toys は、プル リクエスト (PR) を開く前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザー上の動作を一貫してレビューしたいと考えています。カスタム エージェントでこの QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルには、エージェントの目的、指示、利用できるツールを記載します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、これらの機能を使って要件を評価し、結果を報告するよう Copilot に指示します。既存の機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用できる QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA が実施するチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、想定した QA ワークフローと境界がプロファイルに含まれていることを確認します。これにより、検証だけを依頼したのに不完全なエージェントや範囲の広すぎるエージェントが機能を変更するのを防ぎます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略可能ですが、設定するとエージェントの表示名を明確にできます。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うよう定義されていることを確認します。
4. QA が根拠を示しながら結果を報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプル リクエストを作成したりしないことを確認します。
5. これらの責務や境界が不足している場合は、続行する前に既定のエージェントにプロファイルの修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後、同じフィルター機能の会話を再開し、QA を選択して、issue と計画の決定を引き続き利用できるようにします。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成したばかりの場合、利用可能なエージェントの一覧にまだ表示されないことがあります。上記のコマンドを実行すると有効になります。

2. 次のプロンプトを使って、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したら、作成されたレポートを読みます。

## まとめと次のステップ

ワークフローに再利用可能な専門的な役割を追加し、その作業をレビューしました。このレッスンでは、次の内容を学習しました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する仕組みを確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選び、フィルター機能の issue と照合した結果を確認する。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
