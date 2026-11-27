---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな問題があります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を学習します。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- スコープを明確にした issue の重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの初期バージョンを開発した委託先は、ドキュメントを十分に整備しませんでした。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示し、閲覧を続けられるようにします。現時点では完成度の高い実装ではなく、UX の受け入れテストに使えるものを求めています。この作業はブロッカーですが、さらに優先度の高い issue もあります。

どちらも、優先度が下がりやすいタスクです。Copilot cloud agent に割り当てると非同期で処理されるため、開発者は別の作業に集中し、あとから結果をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進捗を追跡するドラフト プル リクエストを作成し、環境をセットアップして、タスクに取りかかります。実行中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プル リクエストで通知されます。

## スコープを適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな作業も、詳しく見ると複雑な場合があります。[Copilot cloud agent に依頼するタスクのスコープを慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot のガイド方法を確認しました。カスタム指示の演習を完了していれば `.github/copilot-instructions.md` の動作を確認したはずです。まだの場合は、今のうちに確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを参照し、後から影響を確認できるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、指示が `**/*.test.ts` ファイルに適用されることを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、作成されたプル リクエストに TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーがあるか確認します。これらは指示ファイルによって適用されます。後の演習で PR をレビューするときに、もう一度取り上げます。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は適切な状態を保つために特定の環境やセットアップ スクリプトが必要です。ソフトウェア エンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成し、作業開始前に実行することで、必要な開発ツールと依存関係を利用できるようにします。このラボの流れを円滑にし、学習に集中できるよう、あらかじめ設定されています。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードが済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるよう、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同様の形式ですが、次の点に注目してください。

- `copilot-setup-steps` というジョブが 1 つあります。このジョブは、Copilot がプル リクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、ドキュメント自体がなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を低下させ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成に役立つため、Copilot cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを生成し、後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。リストに表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成したことを確認できます。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を準備し (前述のワークフローを実行し)、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. リストと、実行するタスクを確認します。
14. プル リクエストのタイムラインを下へスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されるまで、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールし、Copilot が問題を解決している様子を確認します。コードを調査して状態を把握し、計画を考えて判断を下し、コードを作成する手順を確認できます。

完了まで数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に処理して、ほかの作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションの機能を追加する作業を進めます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を始めたことを示す 👀 が issue の最初のコメントに表示されます。

![目の絵文字で Copilot が作業中であることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当て、非同期で処理できます。技術的負債の解消、新しい機能の作成、フレームワーク間のコード移行などに利用できます。

このレッスンでは、次の内容を学習しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- スコープを明確にした issue の重要性を理解する。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と利用に進みます。[Copilot cloud agent では MCP サーバー][cloud-agent-mcp]も利用でき、前のモジュールで学んだカスタム指示も使用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
