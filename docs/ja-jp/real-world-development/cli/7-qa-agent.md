---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザーのフィルター機能を確認しました。今度は、それらを明確な QA プロセスを備えたカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが instruction、skill、MCP ツールと連携する方法を確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選び、フィルター機能の issue に照らして報告内容を確認する。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。カスタム エージェントに QA プロセスを調整させることで、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的なバージョンです。プロファイルにはエージェントの目的、instruction、使用できるツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチームの標準を説明し、`quality-checks` skill は繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、これらの機能を使って要件を評価し、結果を報告するよう Copilot に指示します。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。プロファイルでは、QA が実施するチェックとその制約を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA の流れと制約が反映されているか確認します。これにより、検証だけを行いたいときに、エージェントが不完全な作業をしたり、範囲を超えた変更を行ったりすることを防げます。

1. `/diff` を入力して `.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須で、`name` は任意ですが、表示名として設定できます。
3. プロファイルの instruction を読み、QA が要件を起点とし、リポジトリの instruction に従い、`quality-checks` skill を実行して Playwright MCP を使うことを確認します。
4. QA が根拠を示し、実装コードを変更する前に確認を求め、commit やプル リクエストの作成を行わないことを確認します。
5. 必要な責務や制約が抜けていたら、続ける前に既定のエージェントへ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択して issue と計画の内容を利用します。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> プロファイルを作成した直後は、利用可能なエージェントの一覧に表示されないことがあります。上記のコマンドを入力すると有効にできます。

2. 次のプロンプトで、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を進める様子を確認します。
4. 完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門エージェントを追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが instruction、skill、MCP ツールと連携する方法を確認しました。
- QA プロファイルを作成して確認しました。
- QA エージェントを選択し、issue に照らして結果を確認しました。

実装、skill の更新、QA プロファイル、テスト、検証レポートがそろいました。次は、[変更をまとめて機能の PR を作成し、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
