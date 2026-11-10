---
title: "レッスン 4 - カスタム instruction で Copilot をガイドする"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要があるなら、そのガイダンスを Copilot が参照できるようにします。[Instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も伝えられます。フィルター機能を実装したので、Copilot が使った instruction を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction とパス単位の instruction file がエージェントにどう適用されるかを確認する。
- コーディング標準に沿うよう instruction file を更新する。
- instruction file がコードに与える影響を確認する。

## シナリオ

優れた開発チームである Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。例を挙げます。

- コメントでは、コードを言い換えるのではなく意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数は、`db` 引数を注入できる場合はその点も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変わったらコメントも最新に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

Instruction file を使って、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

## Instruction file

Custom instruction を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件を理解させられます。ライブラリやコーディング規約、コメントの要件などを指定することで、より適切な提案やコードを得られます。リポジトリ全体に適用する instruction と、特定のファイル種別に適用する instruction を作成できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一のファイルです。技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。たとえば TypeScript や Astro、UI コンポーネントの作成、unit test の追加などです。

> [!NOTE]
> ほかの instruction 形式やそのサポート状況は、環境によって異なります。特定の形式を利用する前に、[custom instruction のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

このプロジェクトには、変更の効果を確認するための instruction file があらかじめ用意されています。作業を始める前に確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。これらの instruction は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ レイヤー、テストなどの instruction を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、instruction を適用するファイルを指定する glob を設定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file やプロジェクト内の既存ファイルへのリンクを確認します。大きな instruction を小さく再利用可能なファイルに分け、コード生成の参考例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存の instruction file はよい出発点ですが、補う必要があります。`.github/copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを追加する標準を設定します。

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting guidance** セクション (35 行目付近) を探します。
3. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。instruction を保存した後、フィルター機能のコンテキストを保ったまま会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチでリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、instruction を使って機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの instruction とパス単位の instruction file がエージェントにどう適用されるかを確認しました。
- コーディング標準に沿うよう instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用可能な `quality-checks` skill をカスタマイズして実行します][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file を追加する][instruction-files]
- [Custom instruction を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot の custom instruction をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
