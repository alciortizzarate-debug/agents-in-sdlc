---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリ内の `.github/agents` フォルダーに Markdown ファイルを作成し、エージェントを定義します。ファイルには、対象の作業を効果的に実行するための重点的な指示、ベストプラクティス、コーディングパターン、分野固有の知識を記述できます。チームの専門知識を再利用できるエージェントとして形式化できます。たとえば、[WCAG][wcag] への準拠を徹底するアクセシビリティエージェント、安全なコーディング規約に従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、インストラクションを定義する Markdown のプロンプトを記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が一部重なるところがあります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。わかりやすく言えば、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキル (ほかのエージェントを含む) を調整できます。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに照らしてサイトをレビューし、更新します。その作業の一部として、プルリクエストワークフローのスキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントを作成する方法は1つではありません。ほかの AI の活用方法と同じように、環境やシナリオに合わせてテストを繰り返してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次の内容を確認します。

- カスタムエージェントを定義する方法。
- カスタムエージェントにタスクを割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングプラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダークテーマではテキストと背景色のコントラストが不十分で、読みにくいというフィードバックが寄せられました。このアクセシビリティの課題に対応するため、デザインチームは利用者が切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、この機能をできるだけ早く実装します。カスタムエージェントを使って機能を生成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはあらかじめ作成されています。内容を確認して、Copilot をどうガイドするかを理解しましょう。

Codespace に戻り、次の手順でアクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントでは、`name` と `description` を含むフロントマターが必須です。

3. その後のセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主要な責任。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべき失敗。

## issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを扱う場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、作業中のセッションを誘導したり、追加の指示を与えたりできます。まず、ハイコントラストモードの作成タスクを Copilot に割り当てましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

   ![cloud agent の割り当て画面で、カスタムエージェントと Accessibility が選択されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の専門的な AI アシスタントである[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識と標準を再利用できる形式にまとめ、Copilot が特定のタスクや分野の作業をより効果的に行えるようにします。

次の内容を確認しました。

- カスタムエージェントを定義する方法。
- カスタムエージェントにタスクを割り当てる方法。

Copilot がハイコントラストモードを実装している間に、[Mission control からエージェントセッションを監視して誘導する方法][next-lesson]を確認しましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを利用する準備][org-custom-agents]
- [Enterprise でカスタムエージェントを利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
