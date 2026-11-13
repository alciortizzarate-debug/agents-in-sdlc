---
title: "レッスン 4 - カスタムインストラクションで Copilot をガイドする"
description: "リポジトリのインストラクションを確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際は、コンテキストが重要です。特定の方法でタスクを実行したい場合、その指針を Copilot が参照できるようにしましょう。[インストラクションファイル][instruction-files]には、求めるコードの内容だけでなく、コードの構成方法も記述できます。フィルタリングを実装したので、Copilot が参照したインストラクションを確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリのインストラクションとパス別のインストラクションファイルがエージェントに適用される仕組みを確認します。
- コーディング標準が守られるようにインストラクションファイルを更新します。
- インストラクションファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントには、コードの繰り返しではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数についても記述します。
- 再利用可能な Astro コンポーネントには `Props` コントラクトを記述し、関連コードが変更された際にコメントを最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

インストラクションファイルを活用して、Copilot がこれらの開発慣例に沿ってタスクを実行できるようにします。

## インストラクションファイル

カスタムインストラクションを使うと、コンテキストや設定を Copilot に提供し、コーディングスタイルや要件を理解しやすくできます。使用するコーディング規約やライブラリ、好みのコメントの書き方などを指定することで、より適切な提案やコードスニペットを得られます。リポジトリ全体に適用するインストラクションや、特定のファイル種別に適用するインストラクションを作成できます。

インストラクションファイルには、次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリへのすべてのリクエストで Copilot に送信される単一のインストラクションファイルです。大半のリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル種別、タスクに関するガイドラインを記載します。

> [!NOTE]
> その他のインストラクション形式とそのサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタムインストラクションのサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタムインストラクションファイルを確認する

スタータープロジェクトには、すでにインストラクションファイルが含まれています。変更を加える前に現在の内容を確認して、変更の影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。このインストラクションは、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関するインストラクションがあります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他のインストラクションファイルや既存のプロジェクトファイルへのリンクを確認します。これにより、大きなインストラクションを再利用できる小さなファイルに分け、参考にする例を Copilot に示せます。

## チームのガイダンスに合わせてインストラクションファイルを更新する

既存のファイルはよい出発点ですが、追加すべき項目があります。新しく生成する TypeScript に TSDoc コメントを記述するよう、中心となる `copilot-instructions.md` ファイルを更新しましょう。

1. `.github/copilot-instructions.md` で、35行目付近にある **Code formatting guidance** というセクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリのインストラクションを読み込みます。更新したガイダンスを適用するために、フィルタリングのコンテキストを保ったまま Copilot CLI を終了して会話を再開します。

1. フィルタリングの会話をまだ開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しく追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がインストラクションファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリのインストラクションとパス別のインストラクションファイルがエージェントに適用される仕組みを確認しました。
- コーディング標準が守られるようにインストラクションファイルを更新しました。
- インストラクションファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタムインストラクションを追加する][instruction-files]
- [カスタムインストラクションのサポート][custom-instructions-support]
- [効果的なカスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — インストラクションファイルやその他のリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
