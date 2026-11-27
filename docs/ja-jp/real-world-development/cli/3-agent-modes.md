---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方に合意し、Autopilot で issue に基づくフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。大きな変更には、より確かなプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業し、適切なものを適切な方法で構築できるようにします。このレッスンから、エージェント主導の開発プロセスを段階的に体験します。issue から新しい機能を作成し、コードが有効で期待どおりに動作することを確認して、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主要な概念に集中するため手順を簡略化します。

このレッスンでは、次の内容を学習します。

- GitHub issue から新しい Copilot 会話を開始する。
- Plan モードで要件を定める。
- Autopilot モードで新しい機能を実装する。
- コードをレビューする。
- ポート転送されたブラウザーで機能を手動で検証する。

機能の作業を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリとパブリッシャーでゲームを絞り込めるようにする必要があります。バックログの issue には機能の概要がありますが、カテゴリを組み合わせるなどの詳細は、コーディング前に合意する必要があります。Plan モードで判断を確定してから、範囲を限定した実装を Autopilot に許可します。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、基本がさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 実装する内容を説明した issue から開始する。
2. 構築するものの計画を立てる。
3. コードを作成してレビューする。
4. テストを実行し、コードを検証する。
5. 新しい機能を手動で検証する。
6. プル リクエスト (PR) を作成する。
7. コードのレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> 正確な手順はチームや組織によって異なりますが、多くの場合は上記の流れに沿っています。

標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー プロセスを通るようにします。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを設定します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、ユーザーの入力を待ちながら共同で作業します。
- **Plan**: エージェントが最初に計画を作成し、プロジェクト ファイルの編集は行いません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から開始する

フィルター作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押して、すべての issue を表示します。
8. 矢印キーを使って、**Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) から始まることを確認します。`#` を使うと、GitHub 上の issue またはプル リクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に任せる前に機能の実装方法やタスクの進め方を決められます。複雑な作業は、最初に計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** が表示されていることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. 作業中、フィルターの動作について質問されることがあります。希望に基づいて回答してください。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上下にスクロールして確認します。
6. 決定した内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot を使うと、タスクが完了したと判断するまで Copilot が問題に取り組み、反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様の名前が付いたオプションを選択します。
2. プロンプトの下に **Autopilot** が表示されていることを確認します。
3. Copilot が計画に沿って作業し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まることがあります。先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、セットアップの問題を解決してからチェック結果を確認します。

## 実装をレビューして検証する

コードの生成後、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを実行してすべてが正しく動作することも検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードにします。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を issue および計画時に決めた内容と比較します。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を確認する

コードは問題なさそうですが、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces のポート転送を通じてサイトを開きます。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画時に決めたとおりにカテゴリ フィルター、パブリッシャー フィルター、およびその組み合わせを試します。
4. リセットと結果が空の場合の動作が、issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot に開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub issue から新しい Copilot 会話を開始する。
- Plan モードで要件を定める。
- Autopilot モードで新しい機能を実装する。
- コードをレビューする。
- ポート転送されたブラウザーで機能を手動で検証する。

次は[カスタム指示を使って][next-lesson]、コードが文書化されたプラクティスに沿うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
