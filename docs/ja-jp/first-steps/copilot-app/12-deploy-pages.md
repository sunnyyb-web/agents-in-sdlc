---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app で Pages ワークフローを設定し、プル リクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages でデプロイして、Space Quiz を誰でもアクセスできるようにします。Copilot はワークフローを準備し、デプロイを案内できますが、変更をレビューしてサイトが公開されたことを確認するのはユーザーの役割です。

このレッスンでは、次の内容を学習します。

- Copilot に GitHub Pages のデプロイ設定を依頼する。
- マージする前にワークフローとプル リクエストをレビューする。
- デプロイの状況を確認し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ簡単に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイム依存関係を追加せずに公開 URL からアクセスできるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> プル リクエストを作成すると、コードのコミットとプッシュが行われる場合があります。このプロンプトを送る前に、承認設定と送信先リポジトリを確認してください。**すべてを承認**に設定している場合、個別の確認は表示されません。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローとプル リクエストをレビューする

1. README が、リポジトリにまだ存在しない場合にのみ追加されていることを確認します。
2. ワークフローのビルドとデプロイの手順、トリガー、権限を含め、プル リクエストの変更をレビューします。
3. ワークフローが、マージ前のプル リクエストではなくデフォルト ブランチからデプロイすることを確認します。
4. 必要な修正を依頼し、更新された差分をレビューします。マージを許可する前に、必須のプル リクエスト チェックが成功するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトを送信すると、レビュー済みのプル リクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプル リクエストを確認してから送信してください。承認設定によっては、これらの操作に別途確認が求められない場合があります。

`<PR_NUMBER>` をレビューしたプル リクエストの番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図。Space Quiz のプル リクエストをマージし、Build website、Upload artifact、Deploy to Pages の各ステップを含む GitHub Actions の Deploy website 実行を成功させ、yourname.github.io/space-quiz/ で公開された Space Quiz を表示します。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプル リクエストのマージを実行できない場合は、案内された手順に従います。デプロイが成功し、公開サイトが読み込まれるまで、タスクを完了したと見なさないでください。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告に頼らずにデプロイを確認できました。[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
