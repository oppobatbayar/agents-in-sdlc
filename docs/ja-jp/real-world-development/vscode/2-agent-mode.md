---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

簡単な更新でも、複数のファイルを変更し、テストを実行する必要があることがあります。必要なファイルを探し、変更し、テストを実行して問題を直す作業を繰り返すのは大変です。Copilot Agent Mode は、IDE 内でプロジェクトを調べ、関連ファイルを見つけ、変更を行い、テストなどのタスクを実行して、見つかった問題を修正します。サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。Agent mode は IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent はチームの一員のように非同期で作業し、issue とプル リクエストでやり取りします。

この演習では、次のことを学びます。

- Agent Mode でプロジェクトを調査し、関連ファイルを特定して一貫した変更を行う方法。
- UI とデータ レイヤーをまたいで機能を実装する方法。
- Copilot が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習では publishers helper を追加しました。残りのデータ レイヤー、UI、テストを Copilot Agent Mode で仕上げます。

## Tailspin Toys の Web サイトを実行する

Tailspin Toys は、開発者をテーマにしたボード ゲームのクラウドファンディング サイトです。単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接検索します。別の backend API や client-side UI framework はありません。再利用可能なデータアクセス helper は `src/lib/` に置き、必要な interactivity は標準 DOM API を使った小さな Astro `<script>` で追加します。

### Web サイトを起動する

1. Codespace に戻ります。前の演習後は `main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
3. フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro server の準備ができると、`http://localhost:4321` が表示されます。VS Code がブラウザーを開くか尋ねるダイアログを表示した場合は、**x** を選択して閉じます。
5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルの `http://localhost:4321` を選択してサイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace の Web サーバーに接続するプライベート トンネルです。

### サイトを確認する

ホーム ページにはタイトル、画像、説明を含むゲーム一覧があります。ゲームを選択すると、タイトル、説明、発行元、カテゴリなどを含む詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

1. Chat ビューの agent 選択欄から **Agent** を選びます。Agent は複数のファイルをまたぐ変更を計画・実装し、ターミナル コマンドとツールを実行します。

   ![Chat ビューで agent を選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルから **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このモデルの記載は、特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用しました。LLM は確率的に動作するため、提案が例と異なる場合があります。これは通常の動作です。

3. 次の prompt を送信して、リポジトリの backlog を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. issue の一覧を取得するため **Continue** を選択します。
5. 生成された一覧と優先順位を確認します。

Copilot はユーザーに最も役立つと考える順に issue を並べる場合があります。LLM の確率的な性質により、例とは異なる MCP command を使う場合がありますが、タスクを完了できれば問題ありません。

## Instruction file を確認する

コード生成を始める前に、Copilot にコンテキストを与える `.github/instructions/ui.instructions.md` を確認します。ここには Web サイトへの機能追加方法が記載されています。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. アーキテクチャの概要、コンポーネント設計、テスト容易性、アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などのファイル種別向け instruction へのリンクを確認します。

> [!TIP]
> Instruction file から、ほかの instruction やプロジェクト ファイルを参照できます。パスは instruction file からの相対パスです。複雑な指示を分割して再利用したり、例やテンプレートを示したりできます。

## フィルター機能を実装する

フィルター機能では、少なくともデータ レイヤー (`src/lib/`) のロジック、動作を確認するテスト、ゲーム一覧ページの UI を更新します。マージ前にテストが通る必要があります。

1. 現在の会話を続けるか、**New Chat** で新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI instruction file を選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューで **Agent** が選択されていることを確認します。
4. モデルが **Claude Sonnet 4.5** であることを確認します。
5. backlog の関連 issue に基づいて、次の prompt で機能の実装を依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、データ レイヤー、UI、テストを特定して変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot は調査、ファイル編集、テストを行います。タスクによっては数分かかります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。作業が完了すると、変更ごとにどちらかを選べます。途中で選択する必要はありません。

7. Copilot に求められたら **Continue** を選んでテストを実行します。
8. 処理中に一時停止したり、テストが失敗したりする場合があります。Copilot はタスクが完了し、エラーがないと判断するまでコードとテストを反復します。
9. 生成コードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いて新機能を試します。
11. 動作とコードを確認したら、Copilot Chat で **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode で Tailspin Toys に機能を追加しました。Copilot が UI とデータ レイヤーをまたぐ機能を実装し、関連ファイルを調べて連携した変更を行う方法を学びました。コードベースにマージする前に、変更とテストをレビューすることも確認しました。

次は [Playwright MCP server で機能をテストし][next-lesson]、プル リクエストを作成します。

### 追加の演習 — ページ分割を実装する

ゲーム一覧が増えた場合に備え、Copilot にページ分割の実装を依頼します。既存の instruction を使い、ページ サイズをユーザーが選べるようにするか固定値にするかを決め、必要なガイダンスを prompt に含めてください。希望の動作になるまで Copilot に追加の指示を出すのは通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent mode の違いと使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
