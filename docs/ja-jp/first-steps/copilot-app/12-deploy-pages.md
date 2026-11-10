---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app で Pages workflow を構成し、PR をレビューして公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開しましょう。Copilot は workflow の準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは自分の役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages の構成を依頼する。
- workflow とプル リクエストをレビューしてからマージする。
- デプロイの進行状況を確認し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、ほかの人が簡単に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すると、サーバーやランタイム依存関係を追加せずに公開 URL から利用できます。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: space-quiz の PR がマージされ、Build website、Upload artifact、Deploy to Pages の各段階を含む GitHub Actions の Deploy website 実行が成功し、yourname.github.io/space-quiz/ で Space Quiz が公開されている様子](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、エージェントにプル リクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行前に diff、デプロイ権限、GitHub の承認プロンプトを確認してください。

## Workflow をレビューしてデプロイを確認する

1. README はリポジトリにまだ存在しない場合にのみ追加されたことを確認します。
2. workflow のビルド手順とデプロイ手順を含め、プル リクエストの変更をレビューします。
3. 必須のプル リクエスト チェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで確認します。
5. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化や PR のマージを行えない場合は、提示された手順に従います。デプロイが成功し、公開サイトが読み込まれるまで完了と判断しないでください。

## まとめと次のステップ

Space Quiz をライブ サイトとして公開し、エージェントの報告だけに頼らずデプロイを確認しました。次は[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
