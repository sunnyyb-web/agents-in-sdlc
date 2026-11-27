---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザイン チームが反復して改善できる関連ゲーム機能を作成し、高コントラストとライト モードの切り替えなどのアクセシビリティ機能を実装するよう Copilot に依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も確認したとおり、生成 AI が加わっても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監視されていない状態でタスクを実行するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスと、コードを作成するブランチへの書き込みアクセス**のみ**を持ちます。
- cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーなど、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードへのドキュメント追加に関するプル リクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しなど、既定では外部リソースへのアクセスが制限されているため、これは想定内です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プル リクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプル リクエストを開きます。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、プル リクエストに **[WIP]** が表示されます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるので、休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードと、追加された TSDoc のドキュメント コメントなどを確認します。変更内容は状況によって異なります。

   変更を確認するときは、TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルによって追加されています。

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローが承認待ちになっていることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プル リクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プル リクエストで Copilot と作業することは、一方的なやり取りではありません。チーム メンバーに依頼するのと同じように、プル リクエストやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確定的ではないため、必ず使える定型文はありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを使い、コードベースに必要なドキュメントの変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して、プル リクエストに戻ります。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プル リクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更を確認します。

満足するまで作業を反復できます。問題がなければ、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下へスクロールします。
6. ワークフローが承認待ちになっていることを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プル リクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) を `<branch-name>` に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同じように、cloud agent にコメントを追加して更新を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc のドキュメント コメントを追加したりできます (この issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブにコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 で Mission control から追加したライト モードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、プル リクエストに **[WIP]** が表示されます。その場合は、Copilot が作業を完了するまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 特に次の点を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存するためのローカル ストレージの使用方法。
   - 高コントラスト モードとライト モードに対する CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript または TypeScript コード。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローが承認待ちになっていることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プル リクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名を `<branch-name>` に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーで高コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot をガイドしたことがわかります。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 作業の委任を続ける

cloud agent を活用するときは、実際のバックログ項目を任せて別の作業に取り組めます。この習慣を身につけるために、リポジトリにいくつか issue を追加して Copilot に割り当ててみてください。たとえば、次のようなタスクがあります。

- ゲーム詳細ページにバッカー向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンでは、次の作業を行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、cloud agent の出力に与える影響を確認しました。
- **issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **アクセシビリティ向けのカスタム エージェントを作成して利用し**、高コントラスト モードとライト モードを追加しました。
- **Copilot agents ページを Mission control として使い**、アクセシビリティのセッションを監視して作業中に指示を追加しました。
- **cloud agent のプル リクエストをレビューして反復し**、`@copilot` に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。ほかのハーネスでは、ここで学んだ内容をさらに実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から離れずに Copilot Chat のエージェント モードと MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent と連携する任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューして公開します。

自分のリポジトリでは、次のような作業も試せます。

- リファクタリングまたはテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
