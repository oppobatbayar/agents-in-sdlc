---
title: "演習 1 - Custom instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に伝えておく背景情報がある場合は、そのコンテキストを参照できるようにします。[Instruction file][instruction-files] で、コード生成の指針を伝えます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file が Copilot に適用される様子を確認する。
- 現在の instruction を使って、フィルター機能の最初のデータ部分となる publishers helper を作る。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加する。
- 同じプロンプトを再実行し、生成コードが新しい標準に沿う様子を確認する。
- instruction と helper を `main` に commit し、次の演習で Copilot が最新のガイダンスを使えるようにする。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習の目的は、instruction を更新した後の傾向の変化を確認することであり、出力を 1 文字ずつ一致させることではありません。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。データ レイヤーには unit test が必要で、UI はダーク モードで現代的な印象にし、TSDoc doc comment とファイルの役割を説明するコメントをコードに追加します。Instruction file を使って Copilot にこれらの情報を伝えます。

### Custom instruction

Custom instruction を使うと、コーディング規約、ライブラリ、要件などを Copilot に伝えられます。リポジトリ全体向けの `.github/copilot-instructions.md` と、特定のタスクやファイル種別向けの `.github/instructions/*.instructions.md` があります。後者は `applyTo` frontmatter で対象ファイルを指定できます。IDE では instruction file は Copilot Chat のコード生成に使われ、コード補完や next-edit suggestion には使われません。

Instruction は自然言語で明確に記述し、望ましい例と避けるべき例を示します。大きな指示はファイル種別やタスクごとに分割すると再利用しやすくなります。Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準を使った instruction file][custom-instructions-support]もサポートしています。

このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの instruction が含まれています。Copilot に instruction file を作成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、surface ごとに方法が異なります。テンプレートを探す場合は [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

1. Codespace に戻り、`.github/copilot-instructions.md` を開きます。プロジェクトの概要、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** を確認します。**Code standards** の **GitHub Actions Workflows** のガイダンスにも注目します。
2. `.github/instructions` を開き、Astro、Drizzle データ レイヤー、テストなどの instruction を確認します。
3. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` が repo root からの相対 glob を指定することを確認します。たとえば `**/*.test.ts` に一致する TypeScript test file が対象です。
4. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction や既存ファイルへのリンクを確認します。パスは repo root ではなく、この instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内ドキュメントの要件はありません。後で TSDoc doc comment とファイル コメント ヘッダーを追加します。

## 変更用ブランチを作成する

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する前に Copilot Chat を使う

最初に現在の instruction を使ってプロンプトを送信し、効果を確認します。後で instruction を更新して同じプロンプトを送信し、違いを見ます。

1. 前の演習のエディター タブを閉じます。
2. `src/lib/publishers.ts` を開き、helper の配置場所を Copilot に伝えます。
3. Chat ビューの agent 選択欄から **Agent** を選びます。

   ![Chat ビューで agent を選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認し、helper とテストを更新します。helper が型付き関数で、最初の引数に `db` client を受け取り、型付き publisher 配列を返すことを確認します。これは `.github/instructions/drizzle.instructions.md` の規約によるものです。
6. 生成コードに TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、instruction がなくても doc comment を追加することがあります。その場合も問題ありません。instruction 更新後に一貫性が上がることが重要です。

## リポジトリ標準を追加する

1. `.github/copilot-instructions.md` を開き、27 行目付近の **Code formatting requirements** を探します。
2. 既存の標準の下に次の行を追加し、ファイル ヘッダーと TSDoc doc comment を指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. ファイルを保存します。

> [!TIP]
> リポジトリ レベルの `.github/copilot-instructions.md` はプロジェクト全体のガイダンス向けです。特定の言語、ファイル種別、タスクには `*.instructions.md` を使います。

## プロンプトを再実行して変化を確認する

1. Copilot Chat で **New Chat** を選びます。
2. `src/lib/publishers.ts` を開き、Copilot が適切なファイルに注目していることを確認します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc comment があることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

更新したファイルはそのまま残し、次の演習で使います。

## instruction を commit してブランチを push する

1. ターミナルで変更を確認します。

   ```bash
   git status
   ```

2. instruction と helper の変更を stage して commit します。テストなどの補助的な変更があれば、それらも追加します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチを push したら、バックログにあるドキュメント標準の issue に関連付けて PR を作ります。GitHub のツールが接続されているので、Copilot に依頼できます。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのはこの演習が初めてです。テンプレートで GitHub のツールが構成済みです。初回の利用時に VS Code からサインインを求められた場合は、案内に従います。後の演習で Model Context Protocol について説明します。

1. Codespace で Copilot Chat を開き、**Agent** を選択します。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) で Chat ビューを開きます。
3. 次のプロンプトで、instruction file の issue を探し、現在のブランチから PR を作成するよう依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. GitHub の操作を許可するよう求められたら、コマンドを確認して必要な操作を許可します。
5. PR が作成されたら、次のプロンプトでマージしてローカル ブランチを `main` に戻し、最新状態にするよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作や shell command の許可を求められたら、内容を確認して許可します。

## まとめと次のステップ

このプロジェクトで Copilot が instruction file からコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の instruction でコード生成 prompt を送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じ prompt を再実行し、新しい標準がコードに反映されることを確認しました。
- instruction と helper を `main` に commit し、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に機能を追加する[agent mode][next-lesson]で instruction を活用します。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot の custom instruction をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
