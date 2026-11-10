---
title: "演習 3 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発ワークフロー内の特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、目的、ベスト プラクティス、コーディング パターン、専門知識を記述します。たとえば、[WCAG][wcag] に沿うアクセシビリティ agent、安全なコーディングを確認するセキュリティ agent、一貫したテスト パターンを適用するテスト agent などがあります。

Custom agent はプロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その後に動作や専門性を定義する Markdown prompt が必要です。

### Custom agent と agent skill の違い

Custom agent と [agent skill][agent-skills-concept] はどちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**custom agent は作業者、skill はツール**です。

Custom agent は専用のコンテキスト ウィンドウを持ち、skill や他のエージェントを使って作業を進められます。このラボのアクセシビリティ agent は、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。作業中に、PR ワークフローやテスト実行を管理する skill を呼び出すこともできます。

> [!NOTE]
> Custom agent の作成に唯一の正解はありません。AI を使う他の作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Custom agent について、次のことを学びます。

- custom agent の定義方法。
- タスクを custom agent に割り当てる方法。

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディング サイトを使えるようにしたいと考えています。現在のダーク テーマは、文字色と背景色のコントラストが十分でないため、読みにくいというフィードバックが寄せられています。デザイン チームは、オン/オフを切り替えられるハイ コントラスト モードを求めています。アクセシビリティは重要なので、custom agent に機能を実装させます。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに作成されています。Copilot をガイドする内容を確認しましょう。

1. Codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> Custom agent には `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイトを作るための責務、ベスト プラクティス、HTML/CSS/JavaScript の例、よくある誤りを確認します。

## issue を作成して割り当てる

Mission control は、この環境のエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加のガイダンスを与えられます。まず、ハイ コントラスト モードの作成を依頼します。

1. リポジトリに移動し、**Issues**、**New issue**、**Blank issue** の順に選択します。
2. **Title** に `Add high contrast mode to website` と入力します。
3. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

4. **Create** を選択して issue を作成します。
5. 右側にある **Assign to Copilot** を選択し、一覧から **Accessibility agent** を選びます。

   ![Accessibility custom agent を選択した Copilot の割り当て画面](../../../_images/ex5-select-custom-agent.png)

6. **Assign** を選択します。Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

[GitHub Copilot の custom agent][custom-agents] を確認しました。Custom agent はタスクや分野に特化した AI アシスタントで、チームの専門知識や標準を再利用できる形にできます。

この演習では、次のことを学びました。

- custom agent の定義方法。
- タスクを custom agent に割り当てる方法。

Copilot がハイ コントラスト モードを実装している間に、mission control から[エージェントのセッションを監視して誘導します][next-lesson]。

## リソース

- [Custom agent について][custom-agents]
- [組織で custom agent を利用する準備][org-custom-agents]
- [Enterprise で custom agent を利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視・管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
