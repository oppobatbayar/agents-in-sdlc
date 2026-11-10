---
title: "演習 1 - Custom instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に伝えておく背景情報がある場合は、そのコンテキストを参照できるようにします。[Instruction file][instruction-files] を使うと、何をしてほしいかだけでなく、どのように進めるかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通して、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に適用される様子を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加する。

> [!NOTE]
> VS Code や CLI の harness とは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期で処理するため、その影響をリアルタイムに並べて示すのが難しいためです。後の演習で cloud agent が作成した PR をレビューするときに、instruction file の影響を確認します。

## Instruction file

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには必ず unit test を用意します。
- UI はダーク モードで、現代的な印象にします。
- コードのドキュメントは TSDoc の doc comment として追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

Instruction file を使うと、Copilot がこれらのプラクティスに沿うための情報を得られます。

### Custom instruction

Custom instruction を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコードを得られます。コーディング規約、ライブラリ、コメントの要件などを指定できます。リポジトリ全体向け、または特定のファイル種別向けに instruction を作成できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信されるファイルです。技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。たとえば TypeScript や Astro、UI コンポーネントの作成、unit test の追加などです。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成に使われます。コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準を使った instruction file][custom-instructions-support]もサポートしています。

### Instruction file を管理するためのベスト プラクティス

Instruction file の作成に関する詳しい説明はこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な方法が含まれています。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体に適用するコーディング標準などを記載します。
- `*.instructions.md` は、unit test、Astro コンポーネント、データ レイヤーなどのファイル種別や、特定のタスクに関する具体的な指示に使います。
- 自然言語で明確に書き、望ましいコードと望ましくないコードの例を示します。

Instruction file の作成にも AI の利用にも唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file を用意すると役立ちます。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction file を作成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、surface ごとに方法が異なります。詳しくは各 surface のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、instruction file、custom agent、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

リポジトリに用意されている instruction file を確認します。`.github/copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目します。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの instruction を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これは repo root からの相対 glob を指定します。この例では `**/*.test.ts` に一致する TypeScript の test file が対象です。
5. このプロジェクトの unit test 作成に特化した instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の instruction file やプロジェクト内ファイルへのリンクを確認します。instruction を再利用可能な小さな単位に分け、コード生成の参考例を示せます。記載されているパスは repo root ではなく、この instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、現在のコーディング標準が記載されていますが、コード内ドキュメントの要件はありません。次の手順で TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ標準を追加する

この演習で編集するのは `.github/copilot-instructions.md` です。コード内に TSDoc doc comment とファイル レベルのコメント ヘッダーを置くルールを追加します。後の演習で cloud agent が issue に取り組むとき、この標準を読み取ります。

1. Codespace のターミナルから新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き、**Code formatting requirements** セクション (27 行目付近) を探します。既存の標準にコード内ドキュメントのルールがないことを確認します。
3. 既存の標準の下に次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> Instruction file は、リポジトリ全体向けの `.github/copilot-instructions.md` と、言語、ファイル種別、タスク向けの `*.instructions.md` のどちらにも作成できます。ここで追加した doc comment のようなプロジェクト全体の標準は、リポジトリ レベルのファイルに置きます。

## instruction の更新を commit、push、マージする

Cloud agent は、issue が対象とするブランチの instruction file を読みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。変更した instruction を cloud agent が使えるよう、`main` にマージします。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを開いてマージします。次の演習で cloud agent がこの instruction を読み取れるようになります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを省略して `main` に commit してもかまいません。ブランチを使う手順は、実際のプロジェクトの流れを再現しています。

## まとめと次のステップ

このプロジェクトで Copilot が instruction file からコンテキストを取得する方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。残りの harness で cloud agent が作成する PR をレビューすると、この標準の効果を確認できます。

次は、Copilot cloud agent に最初の issue を[割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot の custom instruction をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
