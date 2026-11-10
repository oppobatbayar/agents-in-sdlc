---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作り、[Codespace][codespaces] を起動します。Codespace で instruction file を編集し、cloud agent が作成した作業をレビューします。

## 演習用リポジトリをセットアップする

コードを作成するためのリポジトリのコピーとして、[テンプレート][template-repository]から新しいインスタンスを作成します。新しいリポジトリには演習に必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys のテンプレート][tailspin-template]に移動します。
2. ラボ リポジトリのページで **Use this template** を選択し、**Create a new repository** を選択して自分用のコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft のイベントで受講している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後で使うため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

## Codespace を作成する

次に、Codespace を使って演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする IDE を利用できます。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりも早く完了します。その間に GitHub Copilot の機能を確認しておきましょう。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。しばらくはブラウザーのタブを閉じずに残してください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動くように作られています。どちらも必要な前提条件がインストールされた環境です。ローカルで作業する場合は、クローンしたリポジトリを VS Code で開き、求められたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

演習用リポジトリを作成し、Codespace の作成を開始しました。以降の演習では、Copilot cloud agent と一緒に作業するためにこの Codespace を使います。

## 次のステップ

次は、cloud agent が従う custom instruction を追加します。[演習 1 - Custom instruction][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
