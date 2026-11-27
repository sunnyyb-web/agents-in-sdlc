---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、変更漏れがあれば別のファイルを更新します。作業は続いていきます。

ここで役立つのが Copilot エージェント モードです。

Copilot エージェント モードは IDE 内でより自律的に作業します。開発者と同じように既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードを使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けに作られています。エージェント モードは IDE 内でタスクを実行するため、すばやくフィードバックを得て対話できます。cloud agent はチーム メンバーのように非同期で作業し、issue とプル リクエストを通じてやり取りします。

この演習では、次の内容を学習します。

- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する。
- UI とデータ レイヤーにわたって機能を実装する。
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。残りのデータ レイヤー、UI、テストの作業を Copilot エージェント モードで完了します。

## Tailspin Toys ウェブサイトを実行する

変更を加える前に、Tailspin Toys ウェブサイトの現在の機能を確認します。

このウェブサイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示できます。Astro アプリはビルド時にページを静的 HTML としてレンダリングします。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接クエリします。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、操作性は標準 DOM API を使う小さな Astro `<script>` で追加します。

### ウェブサイトを起動する

ウェブサイトを簡単に実行するため、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば、`main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行してウェブサイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くように促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、ウェブサイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動でリダイレクトされます。これはウェブ サーバーをホストしている Codespace へのプライベート トンネルです。

### ウェブサイトを確認する

ウェブサイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で説明します。

ウェブサイトの初期実装は動作しますが、さらに機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると issue のバックログが自動作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** はファイル全体の変更を自律的に計画、実装し、ターミナル コマンドやツールを実行します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップを作成した際に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案内容をある程度一貫させ、よい体験を提供することが目的です。ただし、LLM の動作は確定的ではないため、ワークショップの例とは異なる提案が表示される場合があります。これは通常の動作です。

> [!NOTE]
> LLM の動作は確定的ではないため、Copilot が異なる MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと思われる項目を優先順位付けしたことも確認できます。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを提供する指示ファイルを確認します。ウェブサイトに機能を追加する方法を説明する[ユーザー インターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト可能性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類ごとの指示へのリンク。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルを基準とします。これにより、再利用や複雑な指示の分割が容易になり、Copilot に例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルター処理を追加または調整する。
- フィルターの動作を確認するテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot エージェント モードにこれらの作業を任せます。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを指定します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログ内の関連 issue の要件に基づいて機能を実装するよう、次のプロンプトで依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探します。データ レイヤーのヘルパー、UI、テストを見つけた後、ファイルの変更とテストの実行を開始します。

   ![Copilot がプロジェクト ファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイル変更、テストなどを実行します。タスクやコードベースの複雑さによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** が表示される場合があります。作業が完了するとすべての変更に対して操作を選べるので、作業中に選択する必要はありません。

7. Copilot の指示に従い、**Continue** を選択してテストを実行します。

   ![Copilot Chat パネルに表示される、テスト実行の確認ダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストに失敗したりする場合があります。問題ありません。Copilot はタスクを完了してエラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot エージェント モードのチャット セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. ウェブサイトを開いているブラウザーに戻り、新しい機能を確認します。
11. 動作とコードのレビューが完了したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

Tailspin Toys ウェブサイトに機能を追加するために、GitHub Copilot エージェント モードを使う方法を学びました。

- Copilot エージェント モードで UI とデータ レイヤーにわたって新しい機能を実装する。
- プロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する。
- コードベースにマージする前に、エージェント モードが生成した変更とテストをレビューする。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### おまけの演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装するよう依頼してみてください。次の点を検討します。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従う。
- ページ サイズをユーザーが選ぶか、固定にするかなど、実装方法を検討する。
- 希望する実装に必要なガイダンスをプロンプトに含める。
- 変更やコンテキストを追加して、GitHub Copilot と反復する。Copilot と作業する際の通常の進め方です。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
