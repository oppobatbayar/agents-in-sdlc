---
title: "演習 5 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agent][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースを調べ、変更を提案し、ファイルを編集し、コマンドを実行します。VS Code の Copilot Chat では、各 tool call と file 編集をリアルタイムで確認できます。diff を確認して変更ごとに採用または却下し、追加の prompt で作業を調整できます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムで監視する。
- 提案されたファイル編集を確認して採用する。
- 実行中のセッションにライト モードの切り替えを追加するよう依頼する。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ agent は、ハイ コントラスト モードを実装しています。作業中、デザイン チームから既定のテーマが暗すぎると感じるユーザー向けに、ライト モードの切り替えも追加してほしいという依頼がありました。新しい会話を始める代わりに、現在のセッションを誘導します。

## Copilot の進捗を監視する

Copilot Chat ビューには、agent が調査したファイル、実行したコマンド、提案した編集が順に表示されます。編集は diff と **Keep** / **Undo** の操作とともに表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. agent の作業中に Chat をスクロールし、tool call、file 読み取り、編集、実行を求める terminal command を確認します。
3. 変更を採用するには **Keep**、元に戻して再度試させるには **Undo** を選択します。
4. Chat 上部の **Files changed** を開くと、セッション中に編集されたファイルをまとめて確認できます。

> [!NOTE]
> terminal command の実行など、tool call の許可を求められた場合、agent は **Allow** を表示して一時停止します。内容を確認し、許可してよい command だけを承認してください。

5. agent がハイ コントラスト モードを実装し終えるまで待ち、最終メッセージで変更の概要を確認します。

## Agent にライト モードを追加するよう依頼する

同じ会話を続けると、agent は変更したコンポーネント、style の方針、local storage の使い方など、前の作業のコンテキストを保持しています。

1. Copilot Chat の入力欄に次の prompt を送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. agent が既存の変更を確認し、ライト モードを追加する様子を見ます。
3. 各 diff を確認し、**Keep** または **Undo** を選びます。
4. agent が意図しない方向に進んだ場合は、Chat 入力欄の横にある **Stop** で停止し、追加の説明を送ります。

> [!TIP]
> 実行中のセッションを誘導すると、最初からやり直すより効率的です。たとえば「CSS を簡潔にし、selector の重複ではなく CSS variable を使う」「3 つ目の toggle を作らず、既存の設定パネルにライト モードを追加する」「両方の toggle にキーボード ショートカットを追加する」と依頼できます。

## 変更されたファイルをレビューする

commit する前に、agent が変更したファイルを確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイル一覧を確認します。Astro コンポーネント、style、関連テストなどが含まれます。
3. 変更されたファイルを開き、diff を確認します。ARIA 属性、キーボード操作、意味のある HTML、local storage による設定の保存など、アクセシビリティのパターンを確認します。
4. 問題がなければ Source Control パネルで変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat agent mode で作業中のセッションを監視して誘導し、各ファイルの編集を確認して採用または却下し、コンテキストを維持したまま追加の prompt で作業を拡張しました。

この演習では、agent の作業の監視、個々のファイル編集の採用と却下、追加 prompt による実行中セッションの誘導を行いました。次はアクセシビリティの変更をプル リクエストとして公開し、このラボで local custom agent が作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode を使う][agent-mode]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
