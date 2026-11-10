---
title: "レッスン 2 - star rating を追加する: 最初の小さな改善"
description: "Copilot CLI でゲーム カードを変更し、転送ブラウザーでレビューして、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータにはすでに星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を始める。
- エージェントに小さな変更を依頼する。
- `/diff` で変更をレビューする。
- 転送ブラウザーでアプリを実行し、変更を確認する。
- 最初のプル リクエストを開いてマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードにはタイトル、カテゴリ、発行元、説明しか表示されていません。最初の練習として、既存の評価を各カードに表示します。最初のセッションにぴったりの、小さく独立した変更です。

## 会話の構成

**会話**は、タスクについて Copilot CLI と作業する場所です。Copilot app と異なり、通常の CLI の会話は専用の worktree を作るのではなく、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話を後から再開できますが、ファイルとブランチはディスク上の通常の Git 状態として残ります。

会話では、プロンプトとエージェントの応答、ファイルを探索・編集する際のツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始めて変更を依頼する

新しい会話を始めて機能を実装します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot CLI が起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細表示に使うファイルを見つけて必要なコードを作成します。Copilot CLI で新機能を追加できました。

## diff をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI 内で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. 評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. 両方の状態がテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して diff 画面を終了します。

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot は確率的に動作するため、コードは例と異なる場合があります。ひとつの実装に一致するかではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動くと判断せず、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーで更新後の UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを開いてマージする

機能を作成できました。次は、プロジェクトに変更をマージするためのプル リクエスト (PR) を作成します。

1. 既定のエージェントに変更を commit するよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存の commit を push し、PR の URL を表示します。
3. Copilot CLI に表示された URL を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 準備ができたら **Merge pull request** を選択して、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースしました。このレッスンでは、次のことを行いました。

- 機能ブランチで Copilot の会話を始めました。
- エージェントにゲーム カードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送ブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプル リクエストを開いてマージしました。

次は、Plan モードと Autopilot モードを使って[フィルター issue から始め][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
