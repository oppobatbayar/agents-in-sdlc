---
title: "演習 5: GitHub Copilot の作業を反復改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加について GitHub Copilot と作業しました。コードへのドキュメント追加、関連ゲーム機能の作成、ハイ コントラストとライト モードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要なら Copilot に改善を依頼しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームがレビューを始める前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に対する Copilot の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督者なしでタスクを実行します。安全性を保つため、次の制約があります。

- Copilot はリポジトリを読み取れますが、書き込みは作業用ブランチに限られます。
- Cloud agent は GitHub Actions 内に一時的な別環境を作成して作業します。
- GitHub Actions workflow は、実行前に人の承認が必要です。
- 既定では [外部リソースへのアクセス][agent-firewall]が制限されており、MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加した最初の Copilot cloud agent のプル リクエスト (PR) を、GitHub.com の標準 PR 画面で確認します。

> [!NOTE]
> PR に Copilot がファイアウォールでブロックされたという警告が表示されることがあります。外部リソース (外部 MCP サーバーへの接続など) へのアクセスは既定で制限されているため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻り、**Pull Requests** を選択します。
2. **Add missing documentation** などのタイトルの PR を開きます。Copilot が作業中の場合、PR に **[WIP]** が付いています。完了するまで数分待ちます。
3. **Files changed** タブを選び、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

4. TSDoc doc comment やその他のドキュメントを確認します。変更内容は状況によって異なります。Exercise 2 で確認した custom instruction に由来する TSDoc、TypeScript の規約、コメント ヘッダーにも注目します。
5. 問題がなければ **Conversation** タブに戻り、workflow の承認待ちを確認します。表示されている場合は **Approve and run workflows** を選択します。

   ![workflow の実行を承認する「Approve and run workflows」ボタン](../../../_images/shared-approve-workflows.png)

6. PR の checks に workflow が追加され、単一 Astro アプリのチェックが成功するまで待ちます。数分かかることがあります。

## Copilot に変更を依頼する

PR のコメントやコードの inline comment でチーム メンバーに依頼するのと同じように、Copilot にもコメントで変更を依頼できます。Copilot はコメントを確認し、対応する別のセッションを開始します。結果は確率的なので、決まった文面はありません。次のような依頼ができます。

- 各コード ファイルの先頭に、役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc doc comment を追加する。
- Astro アプリの構成を説明する README を作成する。

1. PR に **@copilot** を含むコメントを追加し、ドキュメントの変更を依頼します。
2. **View Session** を選択して Copilot の作業を確認します。更新は新しいセッションで行われます。
3. **Back to pull request** を選択して PR に戻ります。
4. 作業が完了すると、新しい commit が PR に追加されます。**Files changed** で変更をレビューします。

満足するまで反復できます。問題がなければ PR をドラフトからレビュー可能な状態にし、main ブランチにマージします。

![ドラフト PR をレビュー可能な状態に変更する操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. リポジトリの **Pull Requests** タブで、**Show related games on the game details page** のようなタイトルの PR を選択します。
2. **Files changed** を選択してコードをレビューします。
3. **Conversation** タブで workflow の承認待ちを確認し、必要に応じて **Approve and run workflows** を選択します。
4. checks で単一 Astro アプリのチェックが成功することを確認します。
5. **任意:** Copilot が作成したブランチ名に置き換えて、Codespace でブランチをチェックアウトし、関連ゲーム機能を手動テストします。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot にコメントを追加して変更を依頼し、cloud agent と反復作業を続けられます。たとえば表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc doc comment を追加したりできます。後者を依頼する場合は、この issue が custom instruction 更新前に割り当てられたことに注意してください。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用の custom agent が実装した機能を確認します。この PR には、Exercise 3 で依頼したハイ コントラスト モードと、Exercise 4 で mission control から追加依頼したライト モードが含まれます。

1. リポジトリの **Pull Requests** タブで、**Add high contrast mode to website** のようなタイトルの PR を開きます。作業中なら **[WIP]** が消えるまで待ちます。
2. **Files changed** を選択し、生成されたコードをレビューします。次の点を確認します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存する local storage。
   - ハイ コントラスト モードとライト モードの CSS / style。
   - ARIA label やキーボード操作などのアクセシビリティ属性。
   - モード切り替えを管理する JavaScript / TypeScript。
3. **Conversation** タブに戻り、workflow の承認待ちがあれば **Approve and run workflows** を選択します。
4. PR の checks で、単一 Astro アプリのチェックが成功することを確認します。
5. **任意:** Copilot が作成したブランチを Codespace でチェックアウトし、アプリを起動して両方のモードを試します。ページを再読み込みしても設定が保持されることを確認します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Custom accessibility agent がアクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot をガイドしたことを確認します。問題や改善点があれば、前の PR と同様に **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent に実際のバックログ項目を割り当て、その間に別の作業を進める習慣を身につけましょう。リポジトリに issue を追加して Copilot に割り当てます。たとえば次のような作業があります。

- ゲーム詳細ページに backer interest form を作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent harness を完了しました。各レッスンを通じて、次のことを行いました。

- リポジトリに含まれる custom instruction を確認し、cloud agent の出力にどう反映されるかを学びました。
- issue を Copilot cloud agent に割り当て、非同期に環境を準備し、計画して、作業する様子を確認しました。
- アクセシビリティ用の custom agent を作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、実行中のアクセシビリティ セッションを監視して誘導しました。
- Copilot の PR をレビューして反復し、**@copilot** に変更を依頼して workflow を承認しました。

## レビューと次のステップ

Cloud agent harness を完了しました。さらに学びたい場合は、ほかの harness で関連する機能を試せます。

- 🖥️ **[VS Code harness](../../vscode/)** — IDE で Copilot Chat agent mode と MCP 統合を試します。
- 💻 **[CLI harness](../../cli/)** — Plan、Autopilot、agent skill、QA agent、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリで、次のことも試してください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野向けの custom agent を作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
