---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動します。また、エディターで GitHub Copilot Chat が使えることを確認します。

## ラボ用リポジトリを設定する

作成するコードのために、[テンプレート][template-repository]からリポジトリのインスタンスを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれ、演習を通して利用できます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択してリポジトリのコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. ラボの後半で使うため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> **バックログは準備済みです**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue を使います。自分で issue を登録する必要はありません。

## Codespace を作成する

次に、ラボの演習を完了するための Codespace を作成します。

[GitHub Codespaces][codespaces]は、ブラウザーで直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、機能豊富な IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりも早く完了します。その間に GitHub Copilot の機能を確認してもかまいません。次の演習で詳しく扱います。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。しばらくの間、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers]で実行できます。どちらの環境にも、スムーズに進めるために必要な前提条件があらかじめインストールされています。ローカル環境を使う場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェント モードを使う

GitHub Copilot Chat のエージェント モードを使うには、IDE に GitHub Copilot Chat 拡張機能をインストールする必要があります。GitHub Codespace を使っている場合、通常はすでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールした後、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、Codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウで「Hello world」のようなメッセージを入力して Enter キーを押し、Copilot Chat を有効にします。
4. 認証されていない場合は、GitHub アカウントへのサインインが求められます。画面の案内に従って認証します。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ用リポジトリのコピーを作成できました。コードを書き始めるための Codespace も作成しました。

## 次のステップ

Copilot を使い始めましょう。次は[演習 1 - カスタム指示][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
