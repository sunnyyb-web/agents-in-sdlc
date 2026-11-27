---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に実行内容だけでなく、その方法も伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパス固有の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub issue を使って非同期に動作するため、リアルタイムで影響を並べて示すのは困難です。このハーネスの後半で cloud agent が作成したプル リクエストをレビューするときに、指示ファイルの影響を確認します。

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

## リポジトリ標準を追加する

この演習で行う編集は、コード内のドキュメントを TSDoc コメントとファイル レベルのコメント ヘッダーで記述する、プロジェクト全体のルールを追加することです。cloud agent は、このハーネスの後半で割り当てる issue に取り組むときに、このルールを読み取ります。

編集の前に、作業用のブランチを作成します (cloud agent は issue の対象ブランチから指示を読み取りますが、ここでは変更をきれいにコミットします)。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、issue の対象ブランチにある指示ファイルを読み取ります。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。指示を cloud agent に適用するには、変更を `main` に取り込む必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを作成し、マージします。次の演習で issue に取り組むとき、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチ作成を省略して `main` に直接コミットしてもかまいません。実際のプロジェクトでの手順に沿うため、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。このハーネスの残りで cloud agent が作成するプル リクエストを確認すると、その標準が使われていることがわかります。

次は、[Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
