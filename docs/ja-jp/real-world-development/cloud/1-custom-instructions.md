---
title: "演習 1 - カスタムインストラクション (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにしましょう。[インストラクションファイル][instruction-files]を使うと、Copilot に作業内容だけでなく方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタムインストラクションとパス別のインストラクションファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの実行前と実行後を比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、インストラクションの影響をリアルタイムに並べて示すのは困難です。後の演習で cloud agent が作成したプルリクエストをレビューするときに、インストラクションファイルの影響を確認します。

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

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、コード内に TSDoc doc comment とファイル先頭のコメントを記述するよう、プロジェクト全体に適用するルールを追加することだけです。以降の演習で issue を割り当てると、cloud agent がこのルールを適用します。

編集の前に、作業用のブランチを作成します (cloud agent は issue の対象ブランチにあるインストラクションを読み込みますが、変更をきれいにコミットしておきます)。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供するファイルです。リポジトリのコーディング標準に必要なガイダンスを追加します。

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

## インストラクションの更新をコミット、プッシュしてマージする

cloud agent は、issue が対象とするブランチのインストラクションファイルを読み込みます。次の演習で issue を Copilot に割り当てると、`main` からブランチが作成されます。cloud agent が更新したインストラクションを使えるように、変更を `main` にマージする必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成して、マージします。次の演習で cloud agent が issue に取り組むときに、更新したインストラクションが適用されます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略して、`main` に直接コミットしてもかまいません。実際のプロジェクトで一般的な方法を体験できるよう、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot がインストラクションファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。残りのハーネスで cloud agent が作成したプルリクエストをレビューするときに、この標準が使われていることを確認できます。

次は、Copilot cloud agent に[最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使うインストラクションファイル][instruction-files]
- [効果的なカスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタムインストラクションを改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — インストラクションファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
