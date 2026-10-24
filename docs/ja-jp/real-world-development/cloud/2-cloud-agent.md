---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

技術的負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどが考えられます。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行します。エージェントが作業を完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent について、次の内容を確認します。

- コードを生成する環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確に定めた Issue が重要である理由。
- Copilot に Issue を割り当てる。

## シナリオ

Tailspin Toys は、いくつかの技術的負債に対処したいと考えています。サイトの初期開発を担当した委託先は、ドキュメントを不十分な状態で残しました。実際には、ドキュメントがほとんどありません。まず、アプリケーション内のすべてのエクスポート関数に TSDoc コメントを追加します。

また、デザインチームはゲームを見つけやすくする改善を予定しています。ユーザーが引き続きゲームを閲覧できるよう、各ゲーム詳細ページに同じカテゴリーに属するほかのタイトルを関連ゲームとして表示したいと考えています。まだ洗練された実装は必要なく、UX の受け入れテストに使えるものがあれば十分です。これは現在ブロッカーですが、より優先度の高い Issue がほかにあります。

いずれも優先度が下がりやすいタスクですが、Copilot cloud agent に割り当てるのに適しています。cloud agent に非同期で作業させている間、開発者は別の作業に集中し、後から結果をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を記録するドラフトのプルリクエストを作成し、環境をセットアップして、タスクに取りかかります。実行中または完了後に Copilot のセッションを確認できます。解決案のレビュー準備が整うと、プルリクエストで通知されます。

## 範囲を明確にした指示が重要な理由

GitHub Copilot は魔法ではなく、考える工程を省略できるわけではありません。一見単純に見える作業でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼する作業の範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試しながら調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot へのガイダンス方法を学びました。すでにカスタム指示の演習に取り組んだ場合は、`.github/copilot-instructions.md` がどのように使われるか確認しました。まだの場合は、この機会に内容を見ておきましょう。Copilot cloud agent に作業を依頼する前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認すると、後でその影響を見つけやすくなります。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクションを確認し、特に TypeScript 規約と TSDoc コメントに関する要件を読みます。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter があり、これらの指示が `**/*.test.ts` ファイルに適用されることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、作成されたプルリクエストで TSDoc コメント、TypeScript 規約、コメントヘッダーを確認できます。後の演習で PR をレビューする際にも、改めて説明します。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、作業には専用の環境とセットアップが必要です。Copilot にタスクを割り当てる場合も同じです。Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。.github/workflows/copilot-setup-steps.yml ファイルで設定する[セットアップ用ワークフロー][setup-workflow]を作成し、作業の前に実行できます。必要な開発ツールや依存関係を利用できるようになります。ワークショップの流れと学習目的のため、あらかじめ設定済みです。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一の Astro アプリで使用するマイグレーション済み・シード済み SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできるようにします
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各ステップ*に必要な最低限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
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

      # ビルドとテストに必要なデータを用意するため、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

通常の GitHub ワークフローと同じですが、いくつか特長があります。

- `copilot-setup-steps` というジョブが1つあります。このジョブは、Copilot がプルリクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の実行を待たずにワークフローの成功を確認する際に便利です。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、古くなった情報が放置されていたり、情報が不足していたりするプロジェクトは少なくありません。このような技術的負債を解消せずにいると生産性が下がり、コードベースの保守や新しい開発者の参加が難しくなります。Copilot はドキュメント作成を得意とするため、cloud agent に任せるのに適した Issue です。バックグラウンドで必要なドキュメントを生成してもらい、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択し、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して空の Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成しているはずです。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を作成して (前述のワークフローを実行し)、プロジェクトを分析し、問題に取り組む最適な方法を決めています。

13. リストと、Copilot が実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されるまで、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot の作業を確認します。コードの調査や現在の状態の把握、計画を検討するための一時停止、コードの作成などが行われます。

作業には数分かかることがあります。Copilot cloud agent の主な利点は、タスクを非同期で実行し、その間に別の作業ができることです。この機能を活用し、もう1つのタスクを cloud agent に割り当ててから、アプリケーションの機能追加に取り組みましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点の1つは、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業に時間がかからなくても、その間にほかのタスクを進められます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択し、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
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

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに目の絵文字 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が Issue に取り組んでいることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を扱いました。Issue を Copilot に割り当て、非同期に作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

次のことを確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確に定めた Issue が重要である理由。
- Copilot に Issue を割り当てる。

cloud agent がバックグラウンドで作業している間、カスタムエージェントの作成と使用に取り組みましょう。[Copilot cloud agent は MCP サーバーも使用できます][cloud-agent-mcp]。また、前のモジュールで学んだカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

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
