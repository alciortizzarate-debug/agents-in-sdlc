---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタムインストラクション][previous-lesson] |
|:--|

技術的負債に悩まされていない組織はほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残り続けている機能リクエストなど、さまざまな課題があります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。これにより、面倒な作業を任せて開発の速度を上げ、開発者はより大局的な課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- スコープが明確な issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初の Web サイトを作成した委託先が、ドキュメントを十分に作成しませんでした。まず、アプリケーション内のエクスポートされたすべての関数に TSDoc doc comment を追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトル、つまり関連ゲームを表示し、利用者がゲームを見続けられるようにします。まだ洗練された実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、ほかに優先度の高い issue があります。

どちらの作業も、優先度が下がりやすいタスクです。Copilot cloud agent に割り当てれば、非同期に作業を進めている間に別のタスクに集中し、あとから戻って作業をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当てる][start-session]方法があります。割り当てると、Copilot は進行状況を示すドラフトのプルリクエストを作成し、環境をセットアップして、タスクに取り組み始めます。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決方法のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲のタスクを指定する重要性

GitHub Copilot に魔法があるわけではありません。考える作業を省略できるわけではありません。一見簡単そうな作業も、詳しく調べると複雑さが見えてきます。そのため、[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学習、実験、適応しましょう。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタムインストラクション

前の演習では、カスタムインストラクションと Copilot のガイドに使う方法を紹介しました。カスタムインストラクションの演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに用意されたインストラクションファイルを読むと、あとでその効果がわかります。

リポジトリの GitHub Web UI、または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の慣例と TSDoc doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `**/*.test.ts` にインストラクションを適用する `applyTo` フロントマターを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、プルリクエストに含まれる TSDoc doc comment、TypeScript の慣例、ファイルヘッダーに、これらのインストラクションが反映されます。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、作業に必要な環境やセットアップスクリプトを用意する必要があります。同様に、Copilot にタスクを割り当てる場合もセットアップが必要です。Copilot はソフトウェアエンジニアのように作業します。

cloud agent は、作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業を始める前に環境をカスタマイズできます。必要な開発ツールや依存関係を使えるようになります。演習の流れをわかりやすくするため、あらかじめ設定してあります。このワークフローでは、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一の Astro アプリ用にマイグレーションとシードを適用した SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各手順*に必要な最小限のアクセス許可を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローに似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という1つのジョブがあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できるため、Copilot の作業を待たずに正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古かったり、まったくなかったりするプロジェクトも少なくありません。このような技術的負債は対応されないままになり、開発の生産性を落とし、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、この issue は cloud agent に任せるのに適しています。バックグラウンドで必要なドキュメントを作成し、後の演習でその結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR がない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が ToDo リストを作成したことを確認します。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで数分かかることがあります。Copilot は、先ほどのワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストを確認して、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールしながら、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を考えて、コードを作成するまでの作業が表示されます。

この処理には数分かかる場合があります。Copilot cloud agent の主な目的は、作業を非同期に実行して別のタスクに集中できるようにすることです。その機能を活用するため、もう1つのタスクを cloud agent に割り当ててから、アプリケーションに機能を追加するコードを作成します。

## ゲームの詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分けられることです。デザインチーム向けに関連ゲームのセクションを追加する作業は長時間かからないかもしれませんが、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことがわかります。

![issue のコメントに目の絵文字が表示され、Copilot が作業中であることを示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目のリクエストにも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアのように動作するため、作業を常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当てると、非同期で処理できます。技術的負債への対応、新機能の作成、コードの別フレームワークへの移行などを Copilot に依頼できます。

次の内容を確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- スコープが明確な issue の重要性。
- issue を Copilot に割り当てる方法。

Copilot がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。また、前のモジュールで確認したカスタムインストラクションも使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
[start-session]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
