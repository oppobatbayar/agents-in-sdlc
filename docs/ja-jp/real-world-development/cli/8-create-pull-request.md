---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、instruction、skill、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、instruction の更新、skill の更新、quality assurance (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プル リクエストを作成しましょう。最初の star rating のプル リクエスト (PR) は自分でマージしました。今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は、機能、instruction の更新、skill の更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルター機能と品質ワークフロー全体を 1 つの会話とブランチで進め、この PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学ぶ。
- 機能全体の変更と検証の根拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能のワークフローを通じて、Copilot で機能の計画、実装、検証を行いました。Tailspin Toys は、開発者がマージを許可する権限を保ったまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プル リクエストのマージに必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に取り組みます。継続的インテグレーション (CI) のチェック失敗を修正し、レビュー コメントに対応し、必要に応じて rebase します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

ここまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。有効にする前に、変更をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードを作成できたので、まとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションの状態を監視します。すべて成功すると、マージが実行されます。
7. Copilot から `"PR #14 was squash-merged successfully."` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストのプロセスなど、開発作業のいくつかを自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意の cloud agent への委任に使う[便利な Copilot CLI のスラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
