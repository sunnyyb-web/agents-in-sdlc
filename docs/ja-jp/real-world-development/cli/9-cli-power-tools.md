---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ほかの CLI ツールと同様に、GitHub Copilot CLI にも多くのスラッシュ コマンドがあります。これらのコマンドを使って、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` で利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の主なワークフローは完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有など、追加機能を確認します。また、必要に応じて[Copilot cloud agent][about-cloud-agent]に作業を委任することもできます。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルが処理できるコンテキスト ウィンドウの上限に達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、スラッシュ コマンドで状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。無関係なタスクに切り替えるために新しい会話を始める場合は、代わりに `/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意なことが異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルの一覧を表示し、使うモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` オプションを確認します。
2. セッションを共有する場合は、目的に合った方法を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を誰かに送信したり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の規模を広げる、または委任する

ワークショップの主要な部分は完了しました。Copilot CLI には大きなタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分割し、並列で実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送信します。cloud agent は非同期に作業し、プル リクエストを作成する場合があります。

これらのコマンドを使うと worktree やリモート作業が追加される場合があるため、任意の内容として紹介しています。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェント作業を詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次の内容を学習しました。

- `/context` と `/compact` を使って、Copilot のコンテキスト管理を確認した。
- `/model` で利用可能なモデルを確認した。
- `/share` でセッションをエクスポートまたは共有する方法を学んだ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認した。

Copilot CLI には、さらに多くのスラッシュ コマンドがあります。次は[学んだ内容を振り返り][next-lesson]、学習を続ける方法を確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
