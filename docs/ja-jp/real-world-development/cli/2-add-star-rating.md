---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲーム カードに小さな変更を加え、転送されたブラウザーで確認し、最初のプル リクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えます。小さな変更にしましょう。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次の内容を学習します。

- 機能ブランチで Copilot の会話を開始する。
- エージェントにプロジェクトの小さな変更を依頼する。
- `/diff` で変更をレビューする。
- アプリを実行し、転送されたブラウザーで変更を確認する。
- 最初のプル リクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードには、タイトル、カテゴリ、パブリッシャー、説明しか表示されていません。まずはエージェントに既存の評価をカードに表示してもらいましょう。小さく完結した変更なので、最初のセッションに適しています。

## 会話の構成

Copilot CLI でタスクに取り組む場所が**会話**です。Copilot app とは異なり、通常の CLI 会話ではターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。専用の worktree は作成されません。保存した会話は後で再開できます。ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話では、プロンプトとエージェントの応答、ファイルを調査して編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を開始して変更を依頼する

新しい会話を開始して機能の実装に取りかかります。

1. Codespace に戻ります。
2. 前回のターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. `/new` スラッシュ コマンドを入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に関連するファイルを見つけ、必要なコードを作成します。Copilot CLI で最初の機能を追加できました。

## 差分をレビューする

小さな変更でも、生成 AI が作成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は各ゲーム カードに数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. 両方の状態をテストで確認していることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot の動作は確定的ではないため、実際のコードは例と異なる場合があります。特定の実装ではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断するのではなく、Copilot にウェブサイトを起動してもらい、Codespaces で転送されたブラウザーから更新された UI を確認します。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを作成してマージする

機能を追加しました。次はプル リクエスト (PR) を作成し、新しいコードをプロジェクトにマージします。

1. 既定のエージェントに、変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 問題がなければ **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI で最初の変更を公開しました。具体的には、次の作業を行いました。

- 機能ブランチで Copilot の会話を開始した。
- ゲーム カードに小さな変更を加えるようエージェントに依頼した。
- `/diff` で変更をレビューした。
- 転送されたブラウザーでアプリを実行し、星評価を確認した。
- 最初のプル リクエストを作成してマージした。

次は[フィルター issue から始め、Plan と Autopilot モードを使って][next-lesson]大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
