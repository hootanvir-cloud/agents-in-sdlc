---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的とする複数のタスクに GitHub Copilot と取り組みました。コードへのドキュメント追加、デザインチームが改善案を検討するための関連ゲーム機能、高コントラストとライトモードの切り替えを含むアクセシビリティ機能を Copilot に依頼しました。コードの変更を確認し、必要であれば Copilot にフィードバックを送って改善してもらいましょう。

### シナリオ

何度も確認してきたとおり、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成コードをレビューし、通常の DevOps プロセスを進める必要があります。チーム全体のレビューを始める前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監視されていない状態でタスクを実行するため、安全性を保つためにいくつかの制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセスは作業に使うブランチ**のみ**に限定されます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時的な環境を作成します。
- GitHub Actions ワークフローは、人間が承認するまで実行されません。
- MCP サーバーを含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず GitHub Copilot cloud agent が作成した、コードへのドキュメント追加に関する最初のプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認する際、GitHub Copilot がファイアウォールによってブロックされたという警告が表示されることがあります。Copilot は外部 MCP サーバーへの接続など、外部リソースへのアクセスが既定で制限されるため、これは**想定された動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択し、プルリクエストの一覧を開きます。
3. **Add missing documentation** またはそれに近いタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれています。その場合は、作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだ内容を振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントやその他のドキュメントを含め、更新されたコードを確認します。具体的な変更内容は異なる場合があります。

   変更を確認する際は、TSDoc コメント、TypeScript 規約、コメントヘッダーを探します。これらは演習 2 の最初に確認したカスタム指示ファイルに基づいています。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。順調に進めば、単一の Astro アプリに対するプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と協力する方法は、一方的に作業を任せるだけではありません。チームメンバーと同じように、プルリクエストやコードのインラインコメントで Copilot をタグ付けできます。Copilot はコメントを確認し、対応するための別セッションを開始します。結果は確率的に変わるため、依頼文を決め打ちにはできません。たとえば、次のような更新を依頼できます。

- 各コードファイルの先頭に、役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記のアイデアなどを使い、生成されたドキュメントへの変更をコメントで依頼します。ほかのユーザーと同様に **@copilot** をタグ付けします。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択するとプルリクエストに戻れます。

   ![プルリクエストに戻る画面](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが追加されています。
5. **Files changed** タブを選択して変更をレビューします。

満足できるまで反復して改善できます。問題がなければ、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** またはそれに近いタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があることを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが追加されます。順調に進めば、単一の Astro アプリに対するプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動テストすることもできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前回と同じように Copilot cloud agent に更新を依頼して、反復的に改善できます。たとえば、表示する関連ゲームの数を変更したり、コメントヘッダーや TSDoc コメントを追加したりできます (この Issue はカスタム指示を更新**する前に**割り当てました)。前回と同様に **Conversation** タブからコメントを追加すると、Copilot はそれを確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 の Mission control で追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** またはそれに近いタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれています。その場合は、作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モードを切り替えるためのトグル UI コンポーネント。
   - ユーザー設定を保存するローカルストレージの使用。
   - 高コントラストモードとライトモード用の CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。順調に進めば、単一の Astro アプリに対するプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動テストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    続いてアプリケーションを起動し、ブラウザーで高コントラストとライトモードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントにより、アクセシビリティのベストプラクティスに従って機能を実装できました。懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** をタグ付けし、更新を依頼します。

## 任意の演習: 作業をさらに委任する

Cloud agent は、実際のバックログ項目を任せて別の作業に集中できるときに効果を発揮します。リポジトリに Issue をいくつか登録し、Copilot に割り当ててみましょう。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者の関心を登録するフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent の環境での演習を完了しました。次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、後で cloud agent の出力にどう反映されるかを把握しました。
- **Issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期実行を確認しました。
- **カスタムエージェントを作成して使用し**、アクセシビリティのために高コントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページを Mission control として使い**、アクセシビリティのセッションを監視して方向付けました。
- **cloud agent のプルリクエストをレビューして反復改善し**、`@copilot` をタグ付けして変更を依頼し、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent の環境での演習を完了しました。さらに学びたい場合は、ほかの環境で関連するシナリオに取り組めます。

- 🖥️ **[VS Code の環境](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI の環境](../../cli/)** — ターミナルから Plan と Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、`/delegate` による cloud agent への委任を使い、レビュー済みの2つの変更を提供します。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテストカバレッジに関する Issue を cloud agent に割り当てる。
- ほかの分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を構成する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
