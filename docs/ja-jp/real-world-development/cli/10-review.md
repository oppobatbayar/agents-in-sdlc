---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、さらに学ぶためのリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連の開発ワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されているフィルター issue を見つけました。
- star rating を追加し、転送ブラウザーで結果を確認し、最初のプル リクエスト (PR) を手動でマージしました。
- フィルター issue から始め、Plan モードで進め方を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- custom instruction でエージェントをガイドし、既存の `quality-checks` skill をカスタマイズして unit test、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を試しました。
- quality assurance (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、skill の結果、ブラウザーでの検証情報を確認しました。
- フィルター機能全体の変更をレビューし、フィルター PR に対して Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意の cloud agent 委任などのスラッシュ コマンドを確認しました。

## 作成したもの

ワークショップには、更新済みの `main` からそれぞれ別のブランチを作成する、2 つの PR の節目があります。

1. **Star rating:** ゲーム カードに既存の `starRating` と、評価がないことを示す状態を表示します。
2. **フィルターと品質ワークフロー:** フィルター機能を実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` skill は unit test、lint、型チェックを実行し、新しい形式で報告します。QA エージェントは PR の前に、要件とテスト カバレッジのレビューにこれらの結果をまとめました。

追加したテストで実際の不足が解消される必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響を受ける根拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストやツールは、作業結果に影響します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。instruction は標準を設定し、skill は繰り返し実行する作業を記述し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に方法を考えるには **Plan**、範囲を絞った変更に自分も関わるには **Interactive**、明確に範囲を定めたタスクを実行するには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

基盤となる仕組みと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを終えたので、次の CLI 機能も試してみましょう。

- `/review` でコード レビュー エージェントに変更を分析してもらう。
- `/rubber-duck` で問題を相談し、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して実行する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

どのツールも、使い続けることが上達への近道です。製品コードや趣味のコード、ずっと作りたかった小さなアプリで試してください。学んだことをチームと共有し、チームからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
