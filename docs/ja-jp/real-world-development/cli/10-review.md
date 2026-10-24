---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルタリングの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードでアプローチを決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの証拠を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge にフィルタリングの PR をマージする権限を与えました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## 提供したもの

このワークショップには、更新済みの `main` からそれぞれ別のブランチで作成する、2つの PR があります。

1. **星評価:** 既存の `starRating` と、評価がないことを明示する状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を1つの PR にまとめました。

## さまざまな方法での検証

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、カスタマイズした形式で報告しました。QA はこれらの結果に、要件とテストカバレッジのレビューを加えて PR の準備を整えました。

追加したテストで本当の不足を補う必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと証拠を確認してください。変更後は影響を受ける証拠を再確認します。

## ベストプラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに応じて調整してください。指示は標準を定め、スキルは繰り返し行うタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に進め方を検討するため、**Interactive** は範囲を絞った変更に取り組みながら確認を続けるため、**Autopilot** は範囲が明確なタスクに適しています。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

基盤と同じようにコンテキストも重要です。何を作るのか、なぜ必要か、どう作るのかを明確に説明すると、結果が大きく変わります。

## さらに試す

中心的なワークフローは完了しました。Copilot CLI の機能をさらに活用するには、次のコマンドを試してください。

- `/review`: コードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck`: 問題について相談し、別の視点を得ます。
- `/fleet`: 独立したサブタスクを並行して調整します。
- `/worktree`: 別のタスクを分離します。
- `/delegate`: タスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールも、使い続けることが上達への近道です。本番コード、趣味のコード、長年思い描いていた小さなアプリなどで使ってみましょう。学んだことをチームと共有し、ほかのメンバーの知見も学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も試すには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]をご覧ください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
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
