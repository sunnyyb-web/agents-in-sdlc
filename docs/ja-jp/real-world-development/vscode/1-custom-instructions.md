---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot にコンテキストを伝える方法はいくつかあります。特に重要なのが、コード生成の指針となる[指示ファイル][instruction-files]です。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパス固有の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認する。
- 現在の指示を使って、フィルター機能の最初のデータ スライス (publishers ヘルパー) を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、生成コードに新しい標準が適用される様子を確認する。
- 指示の更新とフィルター機能を `main` にコミットし、次の演習で Copilot が更新されたガイダンスを使えるようにする。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確定的ではありません。この演習では、出力の文字列を完全に一致させるのではなく、指示の更新後に動作がどのように変化するかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには必ず単体テストを用意する。
- UI はダーク モードにし、現代的な印象にする。
- TSDoc のドキュメント コメントをコードに追加する。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加する。

指示ファイルを使って、こうしたプラクティスに沿ってタスクを実行するための情報を Copilot に伝えます。

### カスタム指示

カスタム指示を使うと、Copilot にコンテキストと設定を提供し、コーディング スタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定して、Copilot からより適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のタスクやファイルの種類に指示を適用できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中のものの概要、ベスト プラクティスなど、Copilot へのほとんどのチャットまたは CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類向けに作成できます。TypeScript や Astro などの特定の言語、UI コンポーネントの作成、単体テスト一式の作成といったタスクのガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と `applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの標準に基づく指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

このワークショップでは指示ファイルの作成方法をすべて扱いませんが、サンプル プロジェクトには代表的な方法が含まれています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発中のもの、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト レベルの情報を記載します。
- `*.instructions.md` ファイルは、単体テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスクに関する指示に使います。
- 自然な言葉で、明確なガイダンスとコード例を記載します。

指示ファイルの作成方法に決まった形はありません。AI の使い方と同様に、プロジェクトに合う方法は試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示が見つかります。
>
> Copilot に指示ファイルを作成してもらうこともできます。利用する環境によって方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`)。それぞれの環境を扱うレッスンで、関連する方法を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** のガイダンスもあります。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、適用対象のファイルを決める glob が記載されています。リポジトリのルートからの相対パスで指定され、ここでは `**/*.test.ts` に一致する TypeScript テスト ファイルなどが対象です。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。大きな指示セットを小さく再利用可能なファイルに分割し、コード生成の参考となる例を Copilot に示せます。ここに記載されたパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。ヘルパー ファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取る型付き関数で、publisher の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確定的ではないため、指示がなくてもドキュメント コメントが追加される場合があります。その場合も問題ありません。指示を更新した後、一貫性がどのように変化するかを確認することが目的です。

## リポジトリ標準を追加する

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コード提案を改善するために、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc のドキュメント コメントを Copilot に要求します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に置いて全体に適用することも、特定の言語、ファイルの種類、タスク向けの `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメント コメントのルールのような、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys Crowd Funding プラットフォーム用の publisher データ アクセス ヘルパーです。
    * データベースから publisher 情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべての publisher の ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベース クライアントです。
    * @returns publisher オブジェクトの配列に解決する Promise です。
    */
   ```

プロジェクトに新しい標準を追加し、次の演習で使うコードに適用するよう Copilot をガイドできました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリ内のほかのファイルと同じように、ソース管理で管理します。指示とブランチをコミットして、リポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの関連ファイルが追加で表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue と関連付けたプル リクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に作成を依頼します。issue を検索して、issue をクローズする PR を作成し、マージします。

> [!NOTE]
> この演習では初めて、Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートでは、GitHub ツールがワークスペースにあらかじめ接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインが求められる場合があります。画面の案内に従ってください。この接続の仕組み (Model Context Protocol) は、後の演習で確認します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を検索し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、新しい publishers ヘルパーの基礎の両方を説明に含めます。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を検索して PR を作成します。
5. GitHub の操作を許可するよう Copilot に求められた場合は、コマンドを確認して、適切な操作だけを **Allow** します。
6. PR が作成されたら、次のプロンプトを使ってマージし、ローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェル コマンドを許可するよう Copilot に求められた場合は、コマンドを確認して、適切な操作だけを **Allow** します。

GitHub Copilot の助けを借りて、プル リクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認しました。さらに、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送信し、Copilot が生成する内容を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、生成コードに新しい標準が適用される様子を確認する。
- 次の演習で使えるよう、指示とヘルパーの基礎を `main` にコミットする。

次は[エージェント モード][next-lesson]を使い、コードベース全体にわたって Copilot に機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人向けカスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
