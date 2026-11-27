---
title: "演習 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、フィルター機能の issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールと連携できます。すべてコマンド ラインから行えるため、タスクを任せたり、変更を依頼したりしながら、作業に集中できます。まずはツールをインストールしましょう。幸い、使い慣れたツールを使ってインストールできます。

この演習では、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の issue を見つける。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が日常的に作業するターミナルでエージェントの機能を利用できます。この演習では、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI から GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められたら、デバイス コードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して「はい」と答えます。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 使い方を確認する

通常のシェル プロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語でエージェントに指示し、スラッシュ コマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力し、インストール済みのバージョンで利用できるコマンドを確認してから、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、すべてが動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答の前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. Copilot にフィルター機能の issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、issue を読みます。最初の変更を行った後、この issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。worktree は自動的に作成されません。変更ごとに feature branch を作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、設定済みの権限の範囲外でツールを使う前に確認を求めます。このワークショップでは、ユーザーが承認したショートカットとして `--yolo` を付けて再起動します。これにより、Codespace 内で確認を省略でき、演習に集中できます。

> [!CAUTION]
> `--yolo` は、すべての自動権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM などの隔離された環境でのみ使用し、日常の開発で既定の設定として alias にしないでください。詳しくは[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` を使って、後の演習で issue や pull request を操作するための読み取り / 書き込み用 GitHub MCP ツールを有効にします。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは引き続き実際のものです。公開またはマージする前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう一度簡単な質問をし、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後の演習では、指示を変更したりエージェントを追加したりした後、`copilot --resume` を実行して同じ feature の会話とブランチに戻ります。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールした。
- GitHub アカウントで認証した。
- ワークショップのリポジトリを信頼し、簡単な会話を試した。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の issue を見つけた。

次は [最初の小さな変更][next-lesson] を始め、Copilot CLI を使ってゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
