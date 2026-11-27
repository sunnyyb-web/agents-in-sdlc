---
title: "演習 3 - カスタム エージェント"
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

カスタム エージェントを使って、次の内容を学習します。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、現在のダーク テーマはテキストと背景色のコントラストが不十分で、読みにくいというフィードバックが寄せられました。このアクセシビリティの課題に対応するため、ユーザーが切り替えられる高コントラスト モードを実装するよう、デザイン チームから依頼されました。

アクセシビリティは重要なので、できるだけ早く実装します。機能の生成にはカスタム エージェントを利用します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ向けのカスタム エージェントは、すでに用意されています。内容を確認し、Copilot をどのようにガイドするかを理解しましょう。

Codespace に戻り、アクセシビリティ カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルなウェブサイトのコードを生成する際の主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作するための中心的な場所です。Copilot cloud agent にタスクを割り当てたり、タスクを監視したり、実行中のセッションに追加の指示を与えたりできます。まず、高コントラスト モードの作成を Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![cloud agent の割り当て画面。カスタム エージェントとアクセシビリティ エージェントを強調表示しています](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

特定のタスクや分野に合わせた AI アシスタントである、GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントにチームの専門知識や標準を取り入れ、特定の作業をより効果的に進めるよう Copilot をガイドできます。

このレッスンでは、次の内容を学習しました。

- カスタム エージェントの定義方法。
- タスクをカスタム エージェントに割り当てる方法。

Copilot が高コントラスト モードを実装している間に、[Mission control からエージェント セッションを監視し、方向を示します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを利用する準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
