---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を利用する際は、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、必要なコードの内容だけでなく、その構成方法も記述します。フィルター機能を構築したので、Copilot が使用した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示とパス固有の指示ファイルがエージェントにどのように渡されるかを確認する。
- コーディング標準に従うよう指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

優れた開発組織と同様に、Tailspin Toys にも開発プラクティスのガイドラインと要件があります。内容は次のとおりです。

- コメントはコードを言い換えるのではなく、意図や自明ではない判断を説明する。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記載し、注入可能な `db` 引数があればそれも説明する。
- 再利用可能な Astro コンポーネントには `Props` の契約を文書化し、関連コードが変わったらコメントも最新に保つ。
- 既存のフォーマットと lint のガイダンスを保持する。

指示ファイルを使用して、示されたプラクティスに沿ってタスクを実行するために必要な情報を Copilot に提供します。

## 指示ファイル

カスタム指示を使うと、Copilot にコンテキストと設定を提供でき、コーディング スタイルや要件を理解しやすくなります。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定して、Copilot からより適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のファイルの種類やタスクに指示を適用できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定の言語、ファイルの種類、タスク向けのガイドラインを提供します。

> [!NOTE]
> ほかの指示形式やサポート状況は環境によって異なります。特定の形式を利用する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、あらかじめ一連の指示ファイルが含まれています。変更を加える前に既存の内容を確認し、影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明と、コーディング ガイダンスを確認します。この指示は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示が適用されるファイルを決める glob が記載されています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト内のファイルへのリンクを確認します。大きな指示セットを小さく再利用可能なファイルに分割し、Copilot に参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、新しく生成される TypeScript に TSDoc コメントを追加するため、中心となる `copilot-instructions.md` ファイルを変更します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を使うため、フィルター機能の会話を終了して再開します。これにより、機能のコンテキストを保ったまま新しいガイダンスを適用できます。

1. フィルター機能の会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチにいることを確認し、リポジトリのルートから保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示された場合はフィルター機能の会話を選び、`game-filters-cli` ブランチをチェックアウトしていることを確認します。
3. 更新したガイダンスに沿って生成済みのコードを変更するよう、Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの指示とパス固有の指示ファイルがエージェントにどのように渡されるかを確認した。
- コーディング標準に従うよう指示ファイルを更新した。
- 指示ファイルがコードに与える影響を確認した。

次は、lint とテストを一貫して実行するために、[再利用可能な quality-checks スキルをカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
