---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プル リクエスト自動化を学びながら、Tailspin Toys に 2 つの変更を加え、検証してリリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** は、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できるようにします。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業の流れを保てます。

このワークショップでは、Tailspin Toys の一連の開発ワークフローを進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして使い方を確認します。
2. star rating の小さな変更を加え、ブラウザーでレビューし、最初のプル リクエスト (PR) を手動でマージします。
3. フィルター issue から始め、Plan モードで方法を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの instruction を更新し、フィルター機能の作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を試します。
7. quality assurance (QA) カスタム エージェントを作成して、要件、カバレッジ、検証の根拠を確認します。
8. フィルター機能全体の変更をレビューし、フィルター PR に Agent Merge を使います。
9. コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュ コマンドを確認します。

ワークショップを簡潔にするため、star rating と、instruction の更新、skill の更新、QA プロファイル、テストを含むフィルター機能の 2 つの PR を作成します。各ツールを学びながら作業を続けられるよう、フィルター機能と品質ワークフローでは同じ会話とブランチを使います。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証する |
| [2. star rating を追加する][ex2] | 最初の変更 | 既存の評価と評価なしの状態を表示し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画し、Autopilot で構築して Interactive でレビューする |
| [4. カスタム instruction で Copilot をガイドする][ex4] | コンテキスト | instruction を確認して更新し、フィルター機能に適用する |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し実行するチェック | 既存 skill を確認し、レポート形式を変えて実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を構成して、フィルターの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門プロファイルを作成・選択して、最終検証の根拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体を確認して PR を作成し、Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、次の学習方法を振り返る |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づいた Backer Concierge を構築してデプロイし、Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] ターミナル / コマンド ライン操作の基本的な知識
- [ ] インストール済みで設定済みの Git

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、code review、Copilot CLI の機能が含まれているため、すべての harness を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
