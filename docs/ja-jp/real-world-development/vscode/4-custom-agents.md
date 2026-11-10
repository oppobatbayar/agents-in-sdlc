---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発ワークフロー内の特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown file で agent を定義し、目的、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えます。たとえば [WCAG][wcag] に沿うアクセシビリティ agent、安全なコーディングを確認するセキュリティ agent、一貫したテスト パターンを適用するテスト agent などがあります。

Custom agent はプロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には `name` と `description` を含む YAML frontmatter と、その後に動作、専門性、instruction を定義する Markdown prompt が必要です。

### Custom agent と agent skill の違い

Custom agent と [agent skill][agent-skills-concept] はどちらも主に Markdown file で定義し、AI に作業方法を伝えます。分かりやすく言うと、**custom agent は作業者、skill はツール**です。

Custom agent は独自のコンテキスト ウィンドウを持ち、skill や他の agent を調整して作業します。このラボのアクセシビリティ agent は、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その過程で、PR ワークフローやテスト実行を管理する skill を呼び出す場合があります。

> [!NOTE]
> Custom agent の作成に唯一の正解はありません。AI を使う他の作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディング サイトを使えるようにしたいと考えています。現在のダーク テーマは文字と背景のコントラストが不十分で、一部のユーザーには読みにくいというフィードバックがありました。デザイン チームは、オン/オフを切り替えられるハイ コントラスト モードを求めています。この演習では、custom agent に機能を実装させます。

次のことを学びます。

- 既存のアクセシビリティ custom agent を確認する。
- Copilot Chat でアクセシビリティ agent を使ってハイ コントラスト モードを実装する。

## アクセシビリティ custom agent を確認する

1. Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用のブランチを作成します。Exercise 3 のフィルター PR とは分けて作業します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開き、`name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> Custom agent の frontmatter には `name` と `description` が必要です。

3. アクセシブルなサイトを作成する際の責務、ベスト プラクティス、HTML/CSS/JavaScript の例、よくある誤りを確認します。

## Copilot Chat で custom agent を使う

VS Code の Copilot Chat では、`.github/agents` に定義した custom agent を agent の選択欄から選べます。選択した agent の instruction とツールを使うチャット セッションになります。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。

1. Chat ビューの agent 選択欄で **Agent** を選びます。
2. 選択欄を開き、一覧から **Accessibility agent** を選びます。

   ![Chat ビューで agent を選択する画面](../../../_images/shared-chat-mode-selector.png)

3. 次の prompt をアクセシビリティ agent に送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

4. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを更新します。編集内容は file path と diff とともに Chat に表示されます。
5. agent が編集したファイルに合わせて **Files changed** の一覧が更新されます。

> [!NOTE]
> 作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連テストなど既存ファイルを変更します。

次の演習では、進行中の作業をレビューして追加の指示を出します。

## まとめと次のステップ

[Custom agent][custom-agents] は特定のタスクや分野に特化した AI アシスタントです。チームの専門知識と標準を再利用できる形にして Copilot をガイドします。

この演習では、custom agent の定義方法と Copilot Chat agent mode での利用方法を確認しました。次は[agent の作業を監視して誘導し][next-lesson]、変更を確認しながら同じセッションにライト モードの切り替えを追加します。

## リソース

- [Custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の構成][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視・管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
