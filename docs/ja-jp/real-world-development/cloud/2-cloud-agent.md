---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Custom instruction][previous-lesson]

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシー コード、バックログに残った機能要望など、技術的負債の解消に苦労しています。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行します。作業が完了すると、人がレビューできるドラフト PR を作成します。時間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- issue の範囲を明確にする重要性を学ぶ。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを作成した開発者がドキュメントを残していないため、エクスポートされたすべての関数に TSDoc doc comment を追加したいと考えています。

さらに、デザイン チームはゲームの発見しやすさを改善しようとしています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、UX の受け入れテストに使える状態にしたいと考えています。優先度の高い issue が別にあるため、この作業は後回しになりがちです。どちらも、cloud agent に非同期で任せるのに向いたタスクです。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent][copilot-agents] は、人の開発者と同様にバックグラウンドで作業できます。たとえば [GitHub issue を Copilot に割り当て][assign-issue]ると、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備してタスクに取り組みます。作業中または完了後にセッションを確認でき、レビューの準備ができると PR で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略することはできません。一見簡単なタスクにも複雑さがあるため、[cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に作業し、学びながら試して調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

前の演習では custom instruction とその効果を確認しました。まだ確認していない場合は、Copilot cloud agent に作業を割り当てる前に、このリポジトリの instruction file を読み取り専用で確認します。

- `.github/copilot-instructions.md` の **Code standards** を確認し、TypeScript の規約と TSDoc doc comment の要件に注目します。
- `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` frontmatter が `**/*.test.ts` に適用されることを確認します。

次のセクションで **Code lacks documentation** issue を割り当てたら、これらの instruction による TSDoc、TypeScript の規約、コメント ヘッダーを確認します。

## Cloud agent の開発環境を準備する

コードの作成には、通常、ツールと依存関係を整える環境が必要です。タスクを Copilot に割り当てる場合も同様です。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に [セットアップ ワークフロー][setup-workflow]を作成すると、必要な開発ツールや依存関係を用意できます。ラボでは、Node.js、プロジェクトの依存関係、end-to-end test 用ブラウザー、単一 Astro アプリ用に migration と seed を済ませた SQLite データベースを利用できるよう、あらかじめ設定されています。

```yaml
name: "Copilot Setup Steps"
# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch
env:
  ASTRO_TELEMETRY_DISABLED: "1"
jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # この workflow で必要な最小限の権限を設定します。Copilot の操作には専用 token が使われます。
    permissions:
      # セットアップ手順で clone が必要な場合 (依存関係のインストールなど) は contents: read が必要です。
      # clone しない場合、Copilot はセットアップ後に自動でリポジトリを clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5
      # Node.js を使うアプリのセットアップ (このアプリは Astro と Drizzle/libSQL を使用します)。
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"
      - name: Install JavaScript dependencies
        run: npm ci
      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium
      # ローカル SQLite データベースを migration して seed し、build と test 用のデータを準備します。
      - name: Set up the database
        run: npm run db:setup
```

この GitHub workflow には、次の特徴があります。

- Copilot が PR で作業を始める前に実行する `copilot-setup-steps` という単一の job があります。
- `workflow_dispatch` を使うと、Copilot の作業を待たずにリポジトリの **Actions** タブからワークフローを手動実行し、動作を確認できます。

## ドキュメントを追加する issue を作成する

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブ、次に **New issue** を選択します。
3. **Blank issue** を選びます。
4. **Title** に `Code lacks documentation` と入力します。
5. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. **Create** を選択し、issue を作成します。
7. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![issue から Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

8. **Assign** を選択します。
9. **Pull Requests** タブを開き、`[WIP]: Code lacks documentation` のようなタイトルの PR を探します。表示されない場合は少し待ってからブラウザーを更新します。
10. 数分後に Copilot の todo リストが表示されたら、内容を確認します。

> [!NOTE]
> todo リストが表示されるまで数分かかる場合があります。Copilot はセットアップ ワークフローを実行し、プロジェクトを分析して、作業方法を検討しています。

11. PR のタイムラインを確認し、Copilot が issue に取り組み始めたことを確認します。
12. **View session** を選択して、コードを調べ、計画を立て、実装する過程を確認します。更新された状態が表示されない場合は、ブラウザーを更新してください。

   ![Copilot session の表示](../../../_images/ex4-view-session.png)

Todo リストが作成されるまで数分かかることがあります。Copilot は前述の workflow を使って環境を準備し、プロジェクトを分析して、作業方法を検討しています。

この作業には数分かかります。cloud agent が非同期に作業する間、別のタスクに取り組めます。次の機能も同時に依頼しましょう。

## ゲーム詳細ページに関連ゲームを追加する

1. github.com のリポジトリに戻ります。
2. **Issues** タブ、次に **New issue** を選択します。
3. **Blank issue** を選び、タイトルに `Show related games on the game details page` と入力します。
4. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

5. **Create** を選択して issue を作成します。
6. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![issue から Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

7. **Assign** を選択します。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。Copilot は 2 つ目のタスクにも取り組んでいるので、完了後にレビューできます。次は custom agent を作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、issue を Copilot に割り当て、非同期で作業させられます。技術的負債の解消、新機能の作成、フレームワーク間のコード移行などに利用できます。

この演習では、次のことを行いました。

- コード生成環境をカスタマイズする方法を確認しました。
- 操作を安全に行う方法を確認しました。
- issue の範囲を明確にする重要性を学びました。
- issue を Copilot に割り当てました。

Cloud agent が作業している間に、[custom agent の作成と利用][next-lesson]に進みます。[Cloud agent は MCP サーバー][cloud-agent-mcp]と custom instruction も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agent →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
