---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザー体験の改善と機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも大切です。

GitHub.com の [agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。ここでは、開いているプル リクエスト (PR) があるすべてのタスクを確認し、実行された操作を調べ、進行中のセッションに追加の指示を送れます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視する。
- 実行中のセッションに依頼し、機能を追加する。

## シナリオ

ハイ コントラスト モードを作成させた後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能を変更している最中なので、エージェントにライト モードの作業も追加するよう依頼します。

## Copilot cloud agent のタスクを確認する

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインのタスク一覧を確認します。コードベースのドキュメント更新、ゲームを変更する API の生成、サイトのハイ コントラスト モード追加など、割り当てたタスクが表示されます。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。コードのチェックアウト、環境の作成、MCP サーバーのセットアップ、タスクの実行などが含まれます。

> [!NOTE]
> 表示される手順は、作業の状態や Copilot が選んだ方法によって異なります。

4. 右側の PR ペインで、PR と変更されたファイルを確認します。

## Cloud agent に指示を追加する

1. ハイ コントラスト モードのタスクを選択します。正確なタイトルは Copilot が付けた名前や作業状態によって異なります。
2. 数分間セッションを確認します。セットアップが完了し、作業が始まると、メッセージに変化が表示されます。
3. **Steer active session while Copilot is working** ダイアログで、次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents ページで実行中のセッションを誘導するダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して送信します。
5. Copilot が依頼を認識し、作業に加えたことを確認します。

![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

## Copilot に作業を続けてもらう

Copilot は、メッセージを送信した時点で作業中だったステップを終えると、新しい依頼を取り込んで作業を続けます。完了まで数分かかることがあります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使い、cloud agent のタスクを監視してガイドしました。この mission control では、実行中および完了したタスクを追跡し、作業を確認し、進行中のタスクに追加の指示を送れます。

このレッスンでは、次のことを行いました。

- mission control と agents ページでタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、Copilot が作成した PR を[反復改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [Custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
