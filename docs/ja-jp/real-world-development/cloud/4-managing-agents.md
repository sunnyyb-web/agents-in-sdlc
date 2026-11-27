---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、Copilot cloud agent にユーザー エクスペリエンスの改善や機能追加を目的とする 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェント ページ][agents-page]では、オープンなプル リクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、実行中のセッションに指示を追加したりできます。

このレッスンでは、次の内容を学習します。

- cloud agent のタスクを監視するために、エージェント ページを確認する。
- 実行中のセッションに、追加機能を依頼する。

## シナリオ

高コントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加するよい機会だと気付きました。サイトのスタイルを更新し、切り替え機能も追加する作業を進めているため、ライト モードも含めるのが合理的です。高コントラストに加えてライト モードも実装するよう、エージェントの作業に指示を追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方にあるタスク一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - ウェブサイトに高コントラスト モードを追加する。
3. 実行中のタスクを 1 つ選択し、Copilot が実行したタスクを確認します。次のような操作が含まれます。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するための手順を実行する。

> [!NOTE]
> 表示される手順は、作業の状態や Copilot が選択した方法によって異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。PR や変更されたファイルを表示し、さらに詳しく監視できます。

## cloud agent に指示を追加する

実行中のタスクを確認できたので、高コントラスト モードに取り組んでいる Copilot に、ライト モードの切り替えも追加するよう依頼します。

1. 高コントラスト モードを追加しているセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状態によって異なります。

   ![Mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セッションがセットアップを完了し、作業を開始したことがわかるまで数分待ちます。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Agents ページで、実行中のセッションを誘導するダイアログが強調表示された cloud agent タスク](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めることを確認します。

## Copilot に作業を任せる

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しいリクエストを作業の流れに取り込みます。

この作業にも数分かかります。これまでに学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視し、ガイドするための中心的な場所である Copilot agents ページを確認しました。この Mission control では、実行中と完了済みのタスクを追跡し、作業を確認できます。また、実行中のタスクに指示を追加してスコープを調整することもできます。

このレッスンでは、次の内容を学習しました。

- Mission control とエージェント ページを使って cloud agent のタスクを監視する。
- 実行中のセッションに指示を追加し、機能の追加を依頼する。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプル リクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
