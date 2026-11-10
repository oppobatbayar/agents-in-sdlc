---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または構成し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱い、Copilot が利用できる自動化機能を増やす必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を超えて、さまざまなツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解する。
- まだ利用できない場合は Playwright MCP サーバーを追加する。
- エージェントにブラウザーを操作させ、フィルター機能を試す。

## シナリオ

unit test と end-to-end test は重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot が開発中の Web サイトをユーザーのように操作できるようにし、期待どおりの動作を確認しながら、変更プロセスをさらに自動化します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントはリアルタイムで外部ツールやサービスと通信し、最新情報にアクセスしたり、ユーザーに代わって操作したりできます。

これらのツールやリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスをつなぎ、それぞれ異なるツールやリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プル リクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub が提供する [MCP registry][mcp-registry] で、エコシステム内のサーバーを検索したり、貢献したりできます。

> [!CAUTION]
> MCP サーバーは、ほかの依存関係と同じように扱ってください。利用する前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーのように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP をすでに構成している場合は、セットアップをスキップして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を試すよう Copilot に依頼する

これまでは機能が期待どおりに動くか手動で確認しました。今度は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. Copilot に Playwright MCP サーバーで機能を検証するよう、次のプロンプトを送信します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は Copilot が MCP サーバーを自動的に判断するため、明示的に指定する必要はありません。ただし、使用するサーバーが分かっている場合は、プロンプトで示すと動作が安定し、トークンも節約できます。

2. Copilot がブラウザーを操作して機能を確認する手順を表示するので、進行状況を確認します。
3. 結果の報告を読み、すべて期待どおりに動作していることを確認します。

Copilot はアプリを起動して Playwright で操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

Playwright MCP サーバーを使い、Copilot CLI から実際のブラウザーで機能を試しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- 未構成の場合に Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を試しました。

次は、skill とブラウザー ツールを組み合わせる[カスタム QA エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
