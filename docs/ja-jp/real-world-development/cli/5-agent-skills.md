---
title: "演習 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、コードを書くだけではありません。これまで、コードが動作することを手動で検証し、指示ファイルを使って標準に従うようにしてきました。テストや lint、継続的インテグレーション（CI）のほかの作業はどうでしょうか。

このようなタスクには、**agent skill** が適しています。スキルを使うと、テストなどの操作を正しく実行する方法を Copilot に伝えられます。

この演習では、次のことを行います。

- 既存の `quality-checks` スキルを確認する。
- 結果のレポート形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skill は、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたもので、エージェントが必要に応じて読み込みます。基本的には、スキル名をフォルダー名とし、その中に `SKILL.md` という Markdown ファイルを置きます。この Markdown ファイルには、スキルの名前と説明を定義する frontmatter、スキルの概要、呼び出すタイミングのガイダンスを記述します。フォルダーには、スキルが呼び出されたときに使用するスクリプトやその他のリソースを含むサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、プロジェクトの `.github/skills` フォルダーに置いてチームで共有・再利用できるリポジトリ資産にするか、ユーザーのスキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys のチームがユニット テスト、lint、型チェックに使う `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。`description` は、Copilot がスキルをいつ呼び出すかを判断するのに役立ちます。
3. 指示を読み、テストや lint の手順をどのように案内しているか確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを呼び出せます。まず、Copilot に 3 つのチェックを実行するよう依頼します。

1. Interactive モードのフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが実際に報告した内容がわかる、より良いレポートにしましょう。スキルを更新して、その形式でレポートを作成するようにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを実行してみましょう。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。この演習では、次のことを行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` スキルを確認した。
- 結果のレポート形式をカスタマイズした。
- スキルを再読み込みして実行した。

この変更は、フィルター機能と一緒に feature PR に含めます。次は [Playwright MCP サーバー][next-lesson]を使って、Copilot がサイトと直接やり取りできるようにします。

## スキルの例

次のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [`make-repo-contribution` による貢献ワークフロー][contribution-example]
- [`prd` による要件ドキュメント][prd-example]
- [`drawio` による図と同梱のエクスポート スクリプト][drawio-example]
- [`webapp-testing` によるブラウザー テスト][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
