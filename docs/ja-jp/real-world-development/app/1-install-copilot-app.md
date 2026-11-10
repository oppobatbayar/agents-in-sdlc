---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分だけで作業するのではなく、複数のエージェントをそれぞれ隔離されたワークスペースで並行して動かし、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトの準備ができたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインする。
- GitHub リポジトリからプロジェクトをアプリに追加する。
- テンプレートで準備されたバックログなど、ワークスペースの構成を確認する。
- クイックチャットを試してアプリについて質問する。

## シナリオ

チームは、増え続けるバックログに対応するため AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、プル リクエストのマージを一か所から指示できます。このレッスンではアプリをインストールして接続し、プロジェクトについての会話を始めます。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app は Windows、macOS、Linux で利用できます。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用中のプラットフォーム向けアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、表示される手順に従って認証します。データ所在地が有効な GitHub Enterprise Cloud を利用している場合は、**Use GitHub Enterprise** を選択し、プロンプトが表示されたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリを選択する場合は、テンプレートから作成した Tailspin Toys リポジトリを選びます。リポジトリの選択をスキップし、オンボーディング後に追加することもできます。
6. テーマを選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを確認します。すでに表示されていれば、再度追加する必要はありません。
8. まだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選びます。前提条件のレッスンで控えた owner と repository name を入力します。
9. リポジトリがサイドバーに表示されることを確認します。ここで使うレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとまり、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには次の領域があります。

- **New** — ここから Copilot との新しいチャット セッションを開始します。
- **Pull requests** — アプリの GitHub ネイティブ統合を通じてプル リクエストを表示します。アプリを離れずに PR の参照や絞り込み、CI 状態の確認、レビューができます。
- **Issues** — GitHub Issue を参照・絞り込み、Issue からセッションを開始します。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。To-do の管理や定期的な保守などを任せるのに便利です。これは追加の演習ではなく、まとめで次のステップとして紹介します。
- **Customize** — MCP server、プラグイン、skill などのコンポーネントを追加します。後で Playwright MCP を構成するときに使います。
- **Chats** — ブランチや独自のワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初の chat を始めると表示されます。表示されない場合は **Sessions** の横にある **+** を選択して chat を開始します。
- **Sessions** — リポジトリのフォルダーと、そのエージェント セッションをまとめます。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリ ビューが開き、その下のセッションを選ぶと会話に戻ります。セッションはそれぞれ隔離されたワークスペースで実行されるため、変更を競合させずに複数を並行して実行できます。星評価を追加するレッスンで最初のセッションを開始します。

ワークショップを進めながら、ワークスペースを確認していきましょう。

> [!TIP]
> 迷ったときは Copilot に質問してください。操作方法が分からない場合や、何かが可能か知りたい場合に案内してくれます。

### 用意されたバックログを確認する

Tailspin Toys にも、テンプレートからリポジトリを作成したときに用意されたバックログがあります。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリ フォルダーを選択します。次に、リポジトリ上部のナビゲーションで **Issues** を選びます。
2. Issue 番号を決めつけず、タイトルで次の Issue を探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue からエージェント セッションを開始できます。最初の小さな変更が終わったら、フィルター機能の Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが表示されます。他のリポジトリの作業項目を確認するには、そのリポジトリもアプリに追加します。

## クイックチャットを試す

アプリの使い方を知るには、アプリ自体について質問してみましょう。**quick chat** なら、ブランチや worktree を作らずに質問やブレインストーミングができます。セッションを作る必要のない、すぐに終わる質問に適しています。

1. サイドバーの **Chats** の横にある **+** を選択して新しい chat を開きます。**Chats** がまだ表示されない場合は、**Sessions** の横にある **+** で最初の chat を始めます。開始後に **Chats** が表示されます。
2. アプリのセッションについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションは独立した git worktree で実行されるため、変更を競合させずに複数のエージェントを並行して動かせます。会話は続けることも、新しい chat を開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを行いました。

- アプリをインストールして GitHub にサインインしました。
- GitHub リポジトリからプロジェクトを追加しました。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つけました。
- クイックチャットで、すぐに終わる質問をしました。

次は[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app を使い始める][getting-started]
- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
