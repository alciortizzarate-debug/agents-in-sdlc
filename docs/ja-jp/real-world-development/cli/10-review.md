---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、参考リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連の開発ワークフローを完了しました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されているフィルタリングの issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始めて、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタムインストラクションでエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テストカバレッジ、スキルの結果、ブラウザーでの根拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリングの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

ワークショップでは、更新した `main` から作成した別々のブランチで、2つの PR を行います。

1. **星評価**: 既存の `starRating` と未評価であることを示す状態をゲームカードに表示します。
2. **フィルタリングと品質向上のワークフロー**: フィルタリングを実装し、インストラクションを更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチで作業しました。ワークショップを簡潔にするため、これらの変更を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加え、PR を作成する前に一貫した評価を行いました。

追加するテストは、実際に存在する問題を補うものである必要があります。QA の実行結果から追加テストが不要と判断されることもあります。マージを許可する前に、コードと検証結果をレビューしてください。変更を加えた場合は、影響を受ける検証を再実行します。

## ベストプラクティス

Copilot に与えるコンテキストやツールが、作業結果に影響します。このワークショップでは、インストラクションの更新、スキルのカスタマイズ、QA プロファイルの作成、MCP サーバーの設定を行いました。これらのカスタマイズは会話を越えて再利用し、チームのニーズに合わせて調整できます。インストラクションは標準を設定し、スキルは繰り返し実行するタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更やツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築の前に方針を考えるには **Plan**、範囲の絞られた変更で常に確認しながら進めるには **Interactive**、範囲が明確なタスクを進めるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

ツールの基盤と同じくらい、コンテキストも重要です。構築したいもの、その理由、望む実装方法を明確に伝えることで、出力は大きく変わります。

## さらに試す

主要なワークフローはここまでです。次のような CLI の機能も試してみてください。

- `/review` で、コードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で、問題について話し合い、別の視点を得ます。
- `/fleet` で、独立したサブタスクを並行して調整します。
- `/worktree` で、別のタスクを分離します。
- `/delegate` で、Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。実際のコード、趣味のコード、長年作りたいと思いながら手を付けられなかった小さなアプリなどで活用してください。チームと知見を共有し、ほかのメンバーから学びましょう。そして、ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境での作業を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
