---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、焦点を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に伝えます。チームの専門知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどです。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` フォルダーにある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。最もわかりやすい違いは、**カスタム エージェントが作業者で、スキルがツール**であることです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを組み合わせて作業を進めます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プル リクエストのワークフローを扱うスキルや、テストを実行、管理するスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI のほかの使い方と同様に、利用環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、現在のダーク テーマはテキストと背景色のコントラストが不十分で、読みにくいというフィードバックが寄せられました。このアクセシビリティの課題に対応するため、デザイン チームから、ユーザーが切り替えられる高コントラスト モードの実装を依頼されました。

アクセシビリティは重要なので、できるだけ早く実装します。この演習では、アクセシビリティ向けのカスタム エージェントを使って機能を生成します。

この演習では、次の内容を学習します。

- 既存のアクセシビリティ カスタム エージェントを確認する。
- Copilot Chat でアクセシビリティ エージェントを使って、高コントラスト モードを実装する。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ向けのカスタム エージェントは、すでに用意されています。内容を確認し、Copilot をどのようにガイドするかを理解しましょう。

Codespace に戻り、ターミナルを開いて、Exercise 3 のフィルタリング PR とは分けてアクセシビリティ作業用の新しいブランチを `main` から作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルなウェブサイトのコードを生成する際の主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` で定義したすべてのカスタム エージェントが表示されます。カスタム エージェントを選択すると、そのエージェントの指示とツールに基づくチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで、**Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェント ドロップダウンを選択します。既定では、現在のエージェントとして **default** が表示されます。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。編集ごとに、ファイル パスと差分がチャットに表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、エージェントが変更した作業ファイルを確認できます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリに変更を加えながら、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、作業中の内容をレビューし、エージェントに追加の指示を送ります。

## まとめと次のステップ

特定のタスクや分野に合わせた AI アシスタントである、GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントにチームの専門知識や標準を取り入れ、特定の作業をより効果的に進めるよう Copilot をガイドできます。

このレッスンでは、次の内容を学習しました。

- カスタム エージェントの定義方法。
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法。

次は[エージェントの作業を監視して方向を示し][next-lesson]、変更を確認しながら、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
