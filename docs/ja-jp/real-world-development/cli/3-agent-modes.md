---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で issue からフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作するため、正しい方法で必要なものを作れます。このレッスンから、エージェント主導の一般的な開発プロセスをいくつかのレッスンにわたって実践します。issue を使って機能を作り、コードの妥当性と機能の動作を確認し、最後にプロジェクトへ正常にマージします。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常は作業対象ごとに異なるブランチや PR を使いますが、ここでは中核となる概念に集中するため、手順を簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を始める。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- Codespaces の転送ブラウザーで機能を手動検証する。

この機能の開発を続けながら、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリや発行元で絞り込める必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードでその判断を整理し、Autopilot に範囲を限定した実装を許可します。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、基本はさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 実装する内容を説明した issue から始めます。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新しい機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合はここに挙げた流れを調整したものになります。

この標準的な進め方を守ることで、AI が生成したコードも、手書きのコードと同じように要件を満たし、検証されるようになります。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントは最初に計画を作成し、プロジェクト ファイルを編集できません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. star rating の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub の issue やプル リクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に作業を任せる前に、機能やタスクの実装方法を決められます。複雑な作業では、時間をかけて計画を立てることをお勧めします。Plan モードに切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した issue の後ろに、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルターの動作について質問されたら、希望する方法で回答します。正解はひとつではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上から下まで確認します。
6. 自分の決定と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成して確認したので、Autopilot を使って実装します。

Autopilot を使うと、Copilot は完了したと判断するまで問題に取り組み、反復できます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンにある同様のラベルの選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行しながら反復する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画を確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

ほかのコードと同様に、生成されたコードはマージ前にレビューします。コードを確認し、サイトを実行して問題がないか調べましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** がないことを確認します。
2. `/diff` を入力して、フィルターの実装とテストを確認します。
3. 結果を issue と計画時の決定と照らし合わせます。
4. レビューが終わったら、<kbd>Esc</kbd> を押して diff ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードに問題がないことを確認したら、アプリを起動し、Codespaces の転送ポートを通じてサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. カテゴリ、発行元、および計画で合意した組み合わせでフィルターを試します。
4. リセットと結果が空の場合の動作が、issue と合意内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能の作成とレビューを行いました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot の会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- Codespaces の転送ブラウザーで機能を手動検証しました。

次は、ドキュメント化されたプラクティスに沿ってコードを生成できるよう、[カスタム instruction を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
