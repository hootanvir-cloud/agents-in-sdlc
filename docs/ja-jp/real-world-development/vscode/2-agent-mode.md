---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの小さな更新でも、複数のファイルを変更し、テストを実行する必要があることがよくあります。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、デバッグし、変更漏れのファイルを見つけて再度更新する、といった作業を繰り返します。作業は尽きません。

そこで役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に動作します。開発者と同じように既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる体験を提供します。エージェントモードは IDE 内で作業するため、すばやくフィードバックを得てエージェントとやり取りできます。Cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを介してやり取りします。

この演習では、次のことを学びます。

- Copilot エージェントモードがプロジェクトを調査し、関連ファイルを特定して、複数の変更を連携して行う方法。
- GitHub Copilot エージェントモードで UI とデータレイヤーにまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリーや発行元で絞り込めるようにします。前の演習ではパブリッシャーヘルパーを追加しました。この演習では、Copilot エージェントモードを使って、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。Astro アプリは単一のアプリケーションで、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を介してローカルの SQLite データベースに直接問い合わせます。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace では次の手順で実行できます。

1. codespace に戻ります。前の演習後は `main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザーで開くダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> codespace では、ターミナルに表示された localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。codespace へのプライベートトンネルを通じて Web サーバーに接続しています。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧とタイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリーなど、詳細情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されます。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** は自律的に計画し、複数のファイルに変更を加え、ターミナルコマンドを実行して、ツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルから **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。コードの提案が比較的揃い、スムーズに体験できることを期待しています。ただし、LLM は確率的に動作するため、提案内容が手順と異なることがあります。これは正常であり、想定される動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログを表示するよう依頼します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、ユーザーにとって最も役立つと思われる項目から順に優先順位を付けていることも確認できます。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える指示ファイルを確認しましょう。Web サイトに機能を追加する方法が記載された [ユーザーインターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティに関する原則。
   - ファイルの種類ごとにまとめた指示ファイルへのリンク。Astro、Tailwind CSS、Drizzle などがあります。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。これにより指示を再利用でき、複雑な指示を小さな単位に分割し、コード生成時に参考となる例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の3つを更新する必要があります。

- データレイヤー (`src/lib/`) に残りのフィルタリングロジックを追加または調整します。
- フィルタリングの動作をテストするためのテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot エージェントモードにこれらの作業を任せましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 指示ファイルを指定します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が引き続き選択されていることを確認します。
5. バックログの関連 Issue に基づき、機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを探します。データレイヤーのヘルパー、UI、テストを見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって、数分かかることがあります。処理中は、コードエディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業中に選択する必要はありません。完了すると、すべての変更について **Keep** または **Undo** を選択できます。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat がテストの実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot は、タスクが完了してエラーがなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの会話が完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは必ずレビューしてください。

10. Web サイトを実行中のブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードで Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーにまたがる機能を実装する方法。
- Copilot エージェントモードがプロジェクトを調査し、関連ファイルを特定して、複数の変更を連携して行う方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習: ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだ方法を使って、Copilot にページネーションを実装してもらいましょう。次の点を考慮します。

- 既存の指示ファイルを使うなど、ベストプラクティスに従います。
- ユーザーが1ページあたりの件数を選択できるようにするか、固定値にするかなど、ページネーションの仕様を検討します。
- 希望する実装に必要なガイダンスを Copilot に伝えるプロンプトを作成します。
- 必要に応じて GitHub Copilot にコンテキストを提供し、変更を依頼しながら繰り返し改善します。Copilot との作業では一般的な進め方です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
