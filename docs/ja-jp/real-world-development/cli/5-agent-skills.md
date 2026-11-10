---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズしてフィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。これまではコードを手動で検証し、instruction file を使って標準に沿うようにしてきました。テスト、lint、その他の継続的インテグレーション (CI) の作業はどうでしょうか。

このようなタスクには **agent skill** が適しています。Skill は、テストなどの操作を正しく実行する方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認する。
- 結果の表示形式をカスタマイズする。
- skill を再読み込みして実行する。

## シナリオ

Tailspin Toys は unit test、lint、型チェックに `quality-checks` skill を使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## Instruction、script、resource

Agent skill は再利用可能なタスク instruction、実行可能な script、補助 resource をまとめたもので、エージェントが必要に応じて読み込みます。基本的には、skill 名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown には、skill の名前と説明を定義する frontmatter、機能の概要、呼び出すタイミングのガイダンスを記載します。必要に応じて script や resource 用のサブフォルダーも追加できます。

> [!NOTE]
> Skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトにある既存のコマンドを使うため、`SKILL.md` のみで構成されています。

Skill は、チームで共有・再利用するためにプロジェクトの `.github/skills` に置くことも、ユーザーの `~/.copilot/skills` に置くこともできます。

## Skill を確認する

Tailspin Toys のチームが unit test、lint、型チェックを実行するために作成した `quality-checks` skill を確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明によって、Copilot が skill を呼び出すタイミングを判断します。
3. instruction を読み、テストと lint の手順をどうガイドしているか確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

Copilot CLI から直接 skill を呼び出したり、自然言語で依頼したりできます。まず 3 つのチェックを実行します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次の prompt を送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 完了後のレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した内容を明確に示すレポートにします。

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

変更を反映したレポートを確認します。Copilot CLI は会話を再起動せずに、編集後の skill を再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと**同じ prompt**を送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。このレッスンでは、次のことを行いました。

- unit test、lint、型チェックを実行する `quality-checks` skill を確認しました。
- 結果の表示形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更はフィルター機能の PR に含まれます。次は [Playwright MCP server][next-lesson] を使って、Copilot がサイトを直接操作できるようにします。

## Skill の例

次のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills specification][skill-spec]
- [Contribution workflow: `make-repo-contribution`][contribution-example]
- [Requirements documents: `prd`][prd-example]
- [Diagrams and a bundled export script: `drawio`][drawio-example]
- [Browser testing: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
