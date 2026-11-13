---
title: "演習 1 - カスタムインストラクション (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにしましょう。Copilot に特定のコンテキストを伝える方法はいくつかあります。中でも重要なのは[インストラクションファイル][instruction-files]です。コード生成に関するガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタムインストラクションとパス別のインストラクションファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在のインストラクションを使って、フィルタリングに必要な最初のデータスライス (パブリッシャーヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準を取り入れる様子を確認します。
- 次の演習で Copilot が更新したガイダンスを利用できるように、インストラクションとフィルタリングの基盤を `main` にコミットします。

> [!CAUTION]
> 生成コードが、設定した標準の一部に従わないことがあります。Copilot の動作は確率的です。この演習では、指示の更新後に動作がどのように**変化するか**を確認します。生成結果が一字一句同じになることを期待するものではありません。

## インストラクションファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc doc comment を使って、コードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

インストラクションファイルを使って、Copilot がこれらの開発慣例に沿ってタスクを実行できるようにします。

### カスタムインストラクション

カスタムインストラクションを使うと、コンテキストや設定を Copilot に提供し、コーディングスタイルや要件を理解しやすくできます。使用するコーディング規約やライブラリ、好みのコメントの書き方などを指定することで、より適切な提案やコードスニペットを得られます。リポジトリ全体に適用するインストラクションや、特定のファイル種別に適用するインストラクションを作成できます。

インストラクションファイルには、次の種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一のインストラクションファイルです。プロジェクト全体の情報を記載します。たとえば、Copilot に送る多くのチャットや CLI リクエストに関係する技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなどです。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に使うインストラクションファイルです。TypeScript や Astro などの言語別ガイドラインや、UI コンポーネントまたはユニットテストの作成などのタスク別ガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、インストラクションファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルのインストラクションと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> また Copilot は、AGENTS.md や CLAUDE.md など、別の規約に基づくインストラクションファイルも[サポートしています][custom-instructions-support]。

### インストラクションファイル管理のベストプラクティス

インストラクションファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには代表的な構成例があります。基本的には、次のようにします。

- `copilot-instructions.md` には、構築するものの説明、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに関するインストラクションを記載します。
- 自然な言葉を使い、明確なガイダンスと例を示します。コードのよい例と、避けるべき例を記載します。

インストラクションファイルの作成方法は1つではありません。AI の使い方と同様に、プロジェクトに合った方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトには、充実したインストラクションファイルが必要です。プロジェクトに含まれるファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに対応するファイルがあります。
>
> Copilot にインストラクションファイルを作成してもらうこともできます。各環境で手順は異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init`)。このワークショップでは、関連するレッスンでその方法を説明します。
>
> テンプレートや出発点を探すには、インストラクションファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタムインストラクションファイルを確認する

このリポジトリに用意されているインストラクションファイルを読みます。中心となる `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** のガイダンスがあります。ここに記載された内容は、Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関するインストラクションがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フロントマターは、インストラクションを適用するファイルを指定します。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成する際のインストラクションを確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などのインストラクションファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きなインストラクションを再利用できる小さなファイルに分け、Copilot にコード生成時の参考例を示す方法です (パスはリポジトリのルートではなく、インストラクションファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc doc comment とファイル先頭のコメントを追加するルールを設定します。

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## インストラクションを更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタムインストラクションの効果を確認するため、まず現在のインストラクションのままプロンプトを送信します。その後インストラクションを更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディターのタブをすべて閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. Copilot がヘルパーの配置先を把握できるよう、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べて、ヘルパーファイルとテストを含むコードを更新します。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤーの慣例に基づいています。
7. 提案されたコードには、TSDoc doc comment とファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的であり、指示がなくても doc comment が追加される場合があります。その場合も問題ありません。インストラクションを更新した後の**一貫性**の向上を確認することが目的です。

## リポジトリ全体に適用する標準を追加する

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供するファイルです。コードの提案を改善するために、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーに doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。これにより、Copilot にファイル先頭のコメントと TSDoc doc comment を追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したとおり、リポジトリレベル (`.github/copilot-instructions.md`) に全体的なガイダンスを記載したり、特定の言語、ファイル種別、タスク用に `*.instructions.md` を作成したりできます。今回追加した doc comment のルールのような、プロジェクト全体に適用する標準はリポジトリレベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してバッファーをクリアします。
2. `src/lib/publishers.ts` をもう一度選択し、Copilot が正しいファイルを認識するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を新しいプロジェクト標準に従わせ、次の演習で基盤として利用する実際のコードに適用できました。

## インストラクションをコミットしてブランチをプッシュする

インストラクションファイルも、リポジトリ内のほかの資産と同じようにバージョン管理します。変更をコミットして、リポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、インストラクションとヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルでインストラクションの更新と、ヘルパーの基盤に関して Copilot が変更したすべてのファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (たとえばテスト) が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue と関連付けてプルリクエストを作成します。手動で作成することもできますが、GitHub ツールがプロジェクトに接続されているため、Copilot に依頼できます。Copilot に issue を検索して、issue をクローズする PR を作成し、マージしてもらいましょう。

> [!NOTE]
> この演習では初めて Copilot に GitHub での操作を依頼します。プロジェクトテンプレートでは、GitHub ツールをすでにワークスペースに接続しているため、セットアップは不要です。Copilot が初めて GitHub ツールを使う際に、VS Code から GitHub へのサインインを求められることがあります。案内に従って認証してください。この接続 (Model Context Protocol) の仕組みは、後の演習で学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. インストラクションファイルの更新に関する issue を検索し、現在のブランチから PR を作成して、インストラクションの更新とパブリッシャーヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が情報を収集して操作を行う際に、確認を求められたら、実行するコマンドを確認して **Allow** を選択します。
5. PR が作成されたら、Copilot に PR をマージしてローカルのブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作とシェルコマンドを許可するよう求められたら、コマンドを確認して **Allow** を選択します。

これで GitHub Copilot の助けを借りて、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot がインストラクションファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存のインストラクションを使ってコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 次の演習で使えるよう、インストラクションの更新とヘルパーの基盤を `main` にコミットしました。

次は [agent mode][next-lesson] を使い、Copilot がコードベース全体に新機能を追加する際に、インストラクションを活用します。

## リソース

- [GitHub Copilot のカスタマイズに使うインストラクションファイル][instruction-files]
- [効果的なカスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタムインストラクションを改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタムインストラクション][personal-instructions]
- [Awesome Copilot — インストラクションファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
