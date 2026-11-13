---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの最も簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は、必要なファイルをすべて探し、変更を加え、テストを実行し、デバッグし、更新し忘れたファイルに気付いて再度変更する、といった作業を繰り返します。

このようなときに役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者のように既存のプロジェクト構造を調査し、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに設計されています。agent mode は IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチームメンバーのようなペアプログラマーとして、issue とプルリクエストを通じてやりとりしながら、非同期に作業します。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、複数のファイルにまたがる変更を行う。
- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる新機能を実装する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリやパブリッシャーで絞り込めるようにします。前の演習でパブリッシャーヘルパーを追加しました。次は Copilot Agent Mode を使って、データレイヤー、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro アプリであり、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接クエリします。独立したバックエンド API やクライアントサイドの UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準の DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に起動できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から起動します。

1. Codespace に戻ります。前の演習の後なので、`main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内で新しいターミナルウィンドウを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Codespace 内で実行している Web サーバーに接続するためのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧と、それぞれのタイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、パブリッシャー、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

初期状態の Web サイトは動作しますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成した際に issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** は自律的に計画し、複数のファイルを変更し、ターミナルコマンドを実行して、ツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップの作成には Claude Sonnet 4.5 を使ったため、比較的一貫した体験になるよう手順に記載しています。ただし、大規模言語モデルの動作は確率的なので、ワークショップに記載されたものと異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> 大規模言語モデルの動作は確率的なため、Copilot が別の MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot は、利用者にとって役立つと考える順に issue を優先順位付けします。

## インストラクションファイルを確認する

エージェントにコードを生成してもらう前に、作業のコンテキストを Copilot に伝えるインストラクションファイルを確認しましょう。この Web サイトに機能を追加する方法を説明した [ユーザーインターフェイス (UI) インストラクションファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能を追加する方法に関する全体的なガイダンスを確認します。たとえば、次の内容です。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、さまざまなファイル種別に関するインストラクションファイルへのリンク。

> [!TIP]
> インストラクションファイルから、他のインストラクションファイルやプロジェクト内のファイルを参照できます。パスはインストラクションファイルからの相対パスです。これにより、再利用や複雑なインストラクションの分割が容易になり、コード生成の参考となる例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) で残りのフィルタリングロジックを追加または改善する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページを更新して、フィルタリング UI を追加する。

さらに、コードベースにマージする前に、テストを実行して成功させる必要があります。Copilot Agent Mode に依頼して、これらの作業を実行しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、インストラクションファイルを指定します。

   ![UI インストラクションファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログ内の関連 issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、必要なファイルを見つける様子を確認します。データレイヤーのヘルパー、UI、テストを見つけ、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調査する様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行などを行うため、数分かかることがあります。作業中にコードエディターで **Keep** や **Undo** が表示される場合があります。完了するとすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に確認されたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテストの実行を確認するダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを行き来しながら、タスクを完了してエラーを検出しなくなるまで作業します。

   ![Copilot Agent Mode の完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を試します。
11. すべてが動作し、コードのレビューも完了したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる新機能を実装する方法。
- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、複数のファイルにまたがる変更を行う方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 – ページネーションを実装する

ゲームの一覧が増えるにつれて、ページネーションが必要になります。この演習で学んだことを使い、Copilot にサイトのページネーション実装を依頼します。次の点を検討してください。

- 既存のインストラクションファイルを使うなど、現在のベストプラクティスに従います。
- 利用者がページサイズを選択できるようにするか、固定にするかなど、ページネーションの実装方法を検討します。
- 希望どおりの実装を作成できるよう、必要なガイダンスを Copilot に提供します。
- Copilot にコンテキストを伝えて変更を依頼し、反復が必要な場合があります。Copilot と作業する際は通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
