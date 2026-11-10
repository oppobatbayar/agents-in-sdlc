---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試したので、**エージェント セッション**を開始して、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータにはすでに星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、その構成を学ぶ。
- 対象を絞った小さな変更を依頼する。
- ワークスペースの diff ビューで変更をレビューする。
- アプリをローカルで実行し、ブラウザーで変更を確認する。
- 最初のプル リクエストを開いてマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードにはタイトル、カテゴリ、発行元、説明しか表示されていません。最初の練習として、既存の評価を各カードに表示します。最初のセッションにぴったりの、小さく独立した変更です。

## セッションの構成

**セッション**は、独立したワークスペースで実行されるエージェントとの会話です。各セッションには専用の git worktree とブランチが割り当てられます。そのため、あるセッションで機能を追加しながら、別のセッションでバグを修正するなど、変更を競合させずに複数の作業を並行できます。セッションはサイドバーでリポジトリごとにまとまり、選択して切り替えられます。

セッションには、エージェントとの**会話**、ファイルの調査や編集で行ったエージェントの**ツール操作**、diff を含む**変更されたファイル**の一覧が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを開始し、プロジェクトを確認して機能を実装します。[アプリをセットアップしたレッスン][prior-lesson]では、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリに新しいセッションを作成して変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選び、次のプロンプトを送信します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. <kbd>Enter</kbd> を押してプロンプトを Copilot に送信します。

Copilot app は最初にプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調べ、新機能に必要なファイルを探してコードを作成します。Copilot app を使って新機能を追加できました。

## diff をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューします。Copilot app 内で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選ぶと、変更されたファイルと diff が表示されます。

   ![Create PR ボタンの右側に Toggle review panel ボタンがある GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム詳細の表示に使われる中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` なら `No rating yet` を表示する、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot はすべての生成 AI ツールと同様に確率的に動作するため、実際のコードは上記と異なる場合があります。動作が同じであることを確認してください。

## 変更を確認する

ブラウザーを開く前に、エージェントが実行した自動チェックの結果をレビューします。数値の `starRating` と、`null` の場合の代替表示の両方がテストされていることを確認してください。前提条件が不足していたり、チェックがスキップされたりしている場合は成功ではありません。インストールを求められたら、承認する前に内容を確認します。

コードを読むだけで動くと判断せず、Copilot に Web サイトを起動してもらい、ブラウザー キャンバスで更新後の UI を確認します。

> [!TIP]
> Canvas は Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタム Canvas を調べて作成します。ここでは組み込みのブラウザー Canvas を使います。

1. 次のプロンプトで、アプリを起動してブラウザー Canvas に開くよう依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価があるゲーム カードに 5 段階評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使い、このセッションで起動した開発サーバーを停止するよう依頼します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプル リクエストを開いてマージする

機能を作成できました。次は、新しいコードを既存のコードベースにマージするプル リクエスト (PR) を作成します。

1. 右上にある **Create PR** を選択します。
2. 認証を求められた場合は **Sign in with your browser** を選び、表示される手順に従います。
3. Copilot から、リポジトリ ポリシーにより PR 作成前に関連する Issue が必要だと説明された場合は、その手順に従うことを確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビュー パネルで PR を開きます。ここで PR を確認できます。
6. 準備ができたら **Ready to merge** を選択します。
7. 表示されたダイアログで **Merge pull request** を選択して PR をマージします。

## まとめと次のステップ

GitHub Copilot app で最初の変更をリリースしました。このレッスンでは、次のことを行いました。

- エージェント セッションを開始し、その構成を学びました。
- ゲーム カードに対象を絞った小さな変更を依頼しました。
- ワークスペースの diff ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして明示的にマージしました。

次は、Plan モードと Autopilot モードを使って[フィルター Issue から開始し][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプル リクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
