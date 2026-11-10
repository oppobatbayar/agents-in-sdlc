---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: Custom agent →][next-lesson]

Agent mode でフィルター機能を作成しました。プル リクエストを開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに **Playwright MCP server** を接続し、Copilot に実際のブラウザーで機能を試させます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP server が Copilot にツールを追加する仕組みを学ぶ。
- Playwright MCP server をワークスペースに追加する。
- Copilot にブラウザーで機能を手動テストさせる。
- ブランチを公開し、フィルター機能の PR を作成する。

## Model Context Protocol (MCP) とは

MCP を使うと、エージェントはエディターの外にある外部ツールやサービスに接続できます。AI エージェントは最新の情報などのリソースを取得し、ツールを通じてユーザーに代わって操作できます。

![agent mode がコンテキスト、LLM、ツール (MCP server と VS Code 拡張機能が提供するツールを含む) と連携する図](../../../_images/ex1-mcp-diagram.png)

MCP server は AI エージェントと外部ツールやサービス (既存の API や npm package など) の間をつなぎます。たとえば [GitHub MCP Server](https://github.com/github/github-mcp-server) は GitHub repository、issue、PR を管理する API を提供し、[Playwright MCP Server](https://github.com/microsoft/playwright-mcp) は Web ページの移動、フォーム入力、ボタン選択などのブラウザー自動化を提供します。利用できる server はほかにもあり、GitHub は [MCP registry](https://github.com/mcp) を提供しています。

> [!CAUTION]
> MCP server はほかの依存関係と同様に扱ってください。利用前にソース コードと発行元を確認し、セキュリティへの影響を検討します。信頼できる server のみを使い、機密情報や操作へのアクセスを慎重に許可してください。

## MCP の構成を確認する

`.vscode/mcp.json` は、VS Code ワークスペースで利用する MCP server を構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次の `github` server がすでに設定されています。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この設定はプロジェクト テンプレートに含まれているため、前の演習で Copilot が backlog の issue を読めました。[remote GitHub MCP server][remote-github-mcp-server]を使い、VS Code は HTTP で接続します。ツールを初めて使うときに GitHub で認証します。

## Playwright MCP server を追加する

[Playwright MCP server][playwright-mcp-server] は、Copilot が操作できるブラウザーを提供します。

1. `.vscode/mcp.json` の `github` と並べて `playwright` を追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

`github` はリモート server ですが、`playwright` はローカル server です。VS Code が `npx @playwright/mcp@latest` を実行して起動します。Codespace にはブラウザーを表示するデスクトップがないため、`--headless` を指定します。

> [!NOTE]
> Tailspin Toys では end-to-end test に Playwright を使うため、ブラウザーは通常インストール済みです。ブラウザーがないと Copilot から報告された場合は、`npx playwright install chromium` を実行して再試行します。

## Server を起動して信頼する

VS Code は必要に応じて MCP server を起動します。初めてツールを使うときに server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` の上にある **Start** を選択し、server を起動して構成を確認します。
2. VS Code から信頼を求められたら、構成を確認して server を信頼します。

ここで起動しなくても、次の手順で Copilot がツールを必要としたときに VS Code が起動し、信頼を確認します。

## フィルター機能をテストする

Playwright MCP server を接続し、アプリが実行中であることを確認してから、次の prompt を Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server 経由でブラウザーを起動し、手順に沿って操作して結果を報告します。issue の受け入れ条件と照らし合わせます。動作に問題があれば、PR を作成する前に追加の質問をするか、コードを修正させます。

内部では Playwright MCP server はスクリーンショットではなく、ページの [アクセシビリティ ツリー][playwright-mcp-server]を使います。agent はボタン、リンク、リスト項目などの構造化されたラベル付き要素を調べます。これは支援技術と同様の方法で、機能確認と簡単なアクセシビリティ チェックを兼ねます。

> [!NOTE]
> テスト時にアプリが `http://localhost:4321` で実行されている必要があります。dev server を停止した場合は、プロンプトを送る前に再起動します。初回利用時にブラウザーが必要だと報告されたら、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

1. Codespace の **Source Control** パネルに移動し、Copilot の変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンで commit message を生成します。

   ![Source Control パネルに Copilot の変更が表示されている画面](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリに push します。

## プル リクエストを作成する

1. Copilot Chat パネルに移動し、**New Chat** を選択します。
2. agent の選択欄で **Agent** を選びます。
3. 次の prompt で PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が必要な情報を集めて操作を行う際は、必要に応じて **Continue** を選択します。GitHub tool の初回利用時は認証を求められることがあります。
5. Copilot が issue を探して PR を作成したことを確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。**まだマージしないでください。**

## まとめと次のステップ

この演習では、MCP server が Copilot にツールを追加する仕組みを学び、Playwright MCP server をワークスペースに追加し、ブラウザーでフィルター機能をテストしてからブランチを公開し、PR を作成しました。

MCP は Copilot を拡張する方法のひとつです。次は、アクセシビリティなどの作業に使える[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
