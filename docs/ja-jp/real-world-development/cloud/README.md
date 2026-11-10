---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub でタスクを割り当てると、cloud agent はバックグラウンドでリポジトリを調べて変更を行い、プル リクエストを作成します。その間、別の作業を続けられます。

各演習では、cloud agent が従う custom instruction を追加してから、GitHub issue を割り当て、実装を依頼します。custom agent を確認して使い、エージェントのダッシュボードからセッションを監視・誘導し、最後にプル リクエストをレビューして結果を反復改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Custom instruction][ex1] | コンテキスト | cloud agent が使う custom instruction を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てる |
| [3. Custom agent][ex3] | 専門エージェント | custom agent を確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して誘導する |
| [5. 反復改善][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能が含まれているため、どの harness でも完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free 以外の **Copilot Student、Pro、Pro+、Business、Enterprise** プランで利用できます。Copilot Business または Enterprise では、管理者が組織に対して有効にする必要があります。

> [!NOTE]
> この harness では MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI harness](../cli/) または [VS Code harness](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
