---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の演習では、Copilot cloud agent にユーザーエクスペリエンスの改善や機能追加に関する3つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するためのツールは多数あります。たとえば、GitHub.com の[エージェントページ][agents-page]です。この Mission control では、未完了のプルリクエスト (PR) があるエージェントタスクを一覧できます。実行された操作を確認し、進行中のセッションを誘導して、作業を進めることができます。

このレッスンでは、次のことを行います。

- エージェントページから cloud agent のタスクを監視します。
- 進行中のセッションを誘導して、追加機能を依頼します。

## シナリオ

エージェントにハイコントラストモードの作成を依頼したあと、チームはライトモードも追加するよい機会だと考えました。サイトのスタイルと切り替え機能を更新しているため、ライトモードも追加するとよさそうです。ハイコントラストに加えてライトモードも実装するよう、エージェントの作業を誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスクの一覧を確認します。割り当てた次のタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択します。Copilot が実行した手順を確認します。たとえば、次の操作が表示されます。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するためのさまざまな作業を実行する。

> [!NOTE]
> 表示される手順は、Copilot の進行状況や作業方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここから PR と変更されたファイルを確認し、作業を監視できます。

## cloud agent を誘導する

実行中のタスクを確認できたので、エージェントがハイコントラストモードを作成している間にライトモードの切り替え機能も追加するよう Copilot に依頼します。

1. ハイコントラストモードを追加するセッションを選択します。Copilot が付けたタイトルや作業の状態に応じて、正確なタイトルは異なります。

   ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot が作業中のエージェントタスクに追加の指示を送るダイアログを強調表示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に追加することを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、追加のリクエストを作業の流れに取り込みます。

これまでと同様に数分かかります。これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して誘導する中心的な場所である Copilot agents ページを確認しました。この Mission control では、進行中と完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を送ることができます。

次の内容を確認しました。

- Mission control とエージェントページから cloud agent のタスクを監視しました。
- 進行中のセッションを誘導し、追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成した[プルリクエストの反復と改善][next-lesson]に進みましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
