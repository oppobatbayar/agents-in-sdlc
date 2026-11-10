---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動します。また、エディターで GitHub Copilot Chat が使えることを確認します。

## 演習用リポジトリをセットアップする

コードを作成するためのリポジトリのコピーとして、[テンプレート][template-repository]から新しいインスタンスを作成します。新しいリポジトリには演習に必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys のテンプレート][tailspin-template]に移動します。
2. ラボ リポジトリのページで **Use this template** を選択し、次に **Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft のイベントで受講している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後で使うため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> **バックログの準備**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue を使います。自分で issue を登録する必要はありません。

## Codespace を作成する

次に、Codespace を使って演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする IDE を利用できます。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかります。その間に GitHub Copilot の機能を確認しておきましょう。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。しばらくはブラウザーのタブを閉じずに残してください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動くように作られています。どちらにも必要な前提条件がインストールされています。ローカルで作業する場合は、クローンしたリポジトリを VS Code で開き、求められたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat と agent mode を使う

VS Code で GitHub Copilot Chat agent mode を使うには、IDE に GitHub Copilot Chat 拡張機能が必要です。GitHub Codespace では通常、すでにインストールされています。

> [!TIP]
> 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]するか、VS Code の Extensions ビューで **GitHub Copilot Chat** を検索して **Install** を選択します。

インストール後、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディター右側に Copilot Chat が表示されていない場合は、Codespace 上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat に `Hello world` のようなメッセージを入力し、Enter を押します。
4. 認証されていない場合は GitHub アカウントへのサインインが求められます。表示される手順に従います。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

演習用リポジトリを作成し、コードの作成に使う Codespace の起動を始めました。

## 次のステップ

次は Copilot に作業を依頼します。[演習 1 - Custom instruction][next-lesson]に進み、プロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
