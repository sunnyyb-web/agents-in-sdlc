---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルター機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことが開発のすべてではありません。データや外部サービスを扱い、Copilot が追加の自動化を利用できるようにすることも必要です。ここで MCP サーバーが役立ちます。MCP サーバーは Copilot CLI に組み込まれていない機能を追加し、利用できるツールやサービスを増やします。

このレッスンでは、次の内容を学習します。

- Model Context Protocol (MCP) と、Copilot CLI での利用方法を理解する。
- Playwright MCP サーバーがまだ利用できない場合に追加する。
- エージェントにブラウザーを操作させ、フィルター機能を確認する。

## シナリオ

単体テストやエンドツーエンド テストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように作業中のウェブサイトを使えるようにし、変更の自動化を進めて、期待どおりに動作する確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post]を使うと、AI エージェントが外部のツールやサービスと通信できます。最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

これらのツールやリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスをつなぎます。各 MCP サーバーは、AI エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プル リクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーの検索やコントリビューションを促すため、[MCP レジストリ][mcp-registry]を公開しています。

> [!CAUTION]
> MCP サーバーはほかの依存関係と同様に扱ってください。使用前にソース コードをレビューし、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ使えない場合は追加し、Copilot がユーザーのようにウェブサイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、ここでのセットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力してから、<kbd>Tab</kbd> を押します。
4. サーバーの種類として **STDIO** を確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を確認してもらう

前の手順では、機能が期待どおりに動作することを手動で確認しました。今度は Copilot から利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は Copilot が MCP サーバーを自動的に判断するため、明示的に指定する必要はありません。ただし、使用するサーバーがわかっている場合は、プロンプトで案内すると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザー上で機能の動作を確認するために実行する手順を確認します。
3. 結果を読み、すべて期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright でウェブサイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。次の内容を学びました。

- Model Context Protocol (MCP) と、Copilot CLI での利用方法を理解した。
- Playwright MCP サーバーが未設定の場合に追加した。
- エージェントにブラウザーを操作させ、フィルター機能を確認した。

次は、スキルとブラウザー ツールを専門的な役割として組み合わせる[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
