---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここ数回の演習では、ユーザー体験の改善と機能追加を目的とする3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。GitHub.com の[エージェントページ][agents-page]もその1つです。この Mission control では、オープンなプルリクエスト (PR) があるエージェントのタスクをすべて確認できます。実行した操作を調べたり、進行中のセッションに方向付けを行ったりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- セッションの途中で追加機能を依頼し、作業の方向を調整します。

## シナリオ

高コントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加するとよいと気付きました。サイトのスタイルと切り替え機能の更新をすでに進めているため、同じ作業に含めるのが適切です。エージェントにライトモードも追加するよう依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状況を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインにタスクの一覧が表示されます。Copilot に割り当てた次のようなタスクがあることを確認します。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトに高コントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が表示されます。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するための各種操作を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更ファイルを表示して、作業をさらに監視できます。

## Cloud agent に方向を指示する

実行中のタスクを確認できたので、高コントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. 高コントラストモードの追加に関するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control に表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分間セッションを確認します。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のセッションに指示するダイアログを表示したエージェントページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業の流れに組み込むことを確認します。

## Copilot に作業を続けてもらう

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進行中だった手順が完了した後、新しい依頼を作業に取り入れます。

今回も数分かかるため、これまでに学んだことを振り返る時間にしましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、方向付けする中心的な場所である Copilot agents ページを扱いました。Mission control で進行中や完了済みのタスクを追跡し、作業をレビューし、進行中のタスクに追加の指示を送って範囲を調整できます。

次の内容を確認しました。

- Mission control とエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼し、作業を方向付けました。

Copilot がアクセシビリティ機能の実装を終えたら、次は[作成されたプルリクエストを反復して改善します][next-lesson]。

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
