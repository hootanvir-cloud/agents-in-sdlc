---
title: "演習 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、用意されたフィルタリング Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースの探索、コード生成、コマンド実行、外部ツールとの連携をすべてコマンド ラインから行えます。タスクを任せたり、変更を依頼したりしながら、集中を保って作業できます。最初のステップは、想像どおりツールをインストールすることです。幸い、すでによく知っているツールを使って実行できます。

この演習では、次のことを学びます。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーを使ってフィルタリング Issue を見つける。
- インストールを確認する。

## シナリオ

チームでは、増え続けるバックログに対応するために AI agent を使い始めています。Copilot CLI はその機能をターミナルに持ち込みます。ターミナルは、多くの開発者が日常的に作業する場所です。この演習では、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Codespace でターミナルを開く

Copilot CLI をインストールする前に、codespace でターミナル ウィンドウを開く必要があります。

1. まだ開いていない場合は、codespace に戻ります。
2. <kbd>Ctrl</kbd>+<kbd>\`</kbd> を押してターミナル ウィンドウを開きます。
3. VS Code ウィンドウの下部にターミナル パネルが表示されます。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-npm]、[WinGet][install-winget]、[Homebrew][install-homebrew] でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、この演習では npm を使って Copilot CLI をインストールします。

1. ターミナルで、Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   Node.js 24 以上（例: `v24.x.x`）が表示されるはずです。

2. npm を使って codespace に Copilot CLI をグローバル インストールします。

   ```bash
   npm install -g @github/copilot
   ```

3. バージョンを確認してインストールを検証します。

   ```bash
   copilot --version
   ```

   バージョン番号（例: `v1.0.XX`）が表示されるはずです。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 現在ログインしていない場合は、認証を求めるプロンプトが表示されます。Copilot CLI は device code を表示し、URL にアクセスするよう案内します。
3. 画面の指示に従います。
   - 提示された URL をブラウザーで開く
   - 求められたら device code を入力する
   - Copilot CLI が GitHub アカウントにアクセスできるよう承認する
4. 認証が完了すると、質問やコマンドを受け付ける Copilot CLI のプロンプトが表示されます。

> [!NOTE]
> Codespace では、GitHub のセッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## CLI を使い始める

通常のシェルプロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI を起動した後は、自然言語の入力がエージェントに送られ、slash command で会話を操作します。

初めて Copilot CLI のプロンプトが表示されたので、このワークショップのリポジトリを信頼済みにし、Copilot CLI が正しくインストールされ、接続されていることを確認しましょう。

1. Copilot CLI にこのフォルダー内のファイルを信頼するか確認されたら、次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

2. パスが Tailspin Toys リポジトリであることを確認し、次のいずれかを選択します。
   - **Yes, proceed**: このセッション中のみ信頼します。
   - **Yes, and remember this folder for future sessions**: 今後のセッションでもこのフォルダーを信頼します。
   - **No, exit (Esc)**: ファイルへのアクセスを許可せず終了します。

   このワークショップでは、このリポジトリで継続して作業するため、**Yes, and remember this folder for future sessions** を選択します。

3. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
4. `/help` を入力して利用可能なコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
5. Copilot に簡単な質問をして、正しく動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

6. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
7. Copilot にフィルタリング Issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

8. URL を開いて Issue を読みます。最初の変更を行う演習の後で使用します。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。worktree は自動作成されません。変更ごとに機能ブランチを作成します。

## ワークショップ用ショートカットを使う

Copilot CLI は通常、許可範囲外のツールを使う前に確認します。このワークショップでは Codespace 内の承認プロンプトを省くため、利用者が承認したショートカット `--yolo` を使用します。

> [!CAUTION]
> `--yolo` は完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの隔離環境でのみ使用し、日常の開発用の既定値として alias に設定しないでください。[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` により、後のレッスンで Issue や pull request を操作するための読み書き可能な GitHub MCP ツールを有効にします。Codespace はローカルコンピューターへのアクセスを制限しますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話が機能していることを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで指示を変更したりエージェントを追加したりした後、`copilot --resume` を使うと、同じ機能の会話とブランチに戻れます。

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI のインストールと認証が完了しました。次のことを学びました。

- npm を使って Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- Copilot CLI が作業できるようにディレクトリを信頼する。
- 組み込みの GitHub MCP サーバーを使ってフィルタリング Issue を見つける。
- インストールが正しく動作していることを確認する。

Copilot CLI をインストールできたので、次はプロジェクトに小さな変更を加えます。[レッスン 2: 星評価を追加する][next-lesson]に進んでください。

## リソース

- [GitHub Copilot CLI のインストール][install-copilot-cli]
- [Copilot CLI について][about-copilot-cli]
- [Copilot CLI を使う][using-copilot-cli]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-copilot-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[install-npm]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli#installing-with-npm-all-platforms
[install-winget]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli#installing-with-winget-windows
[install-homebrew]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli#installing-with-homebrew-macos-and-linux
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[using-copilot-cli]: https://docs.github.com/copilot/how-tos/use-copilot-agents/use-copilot-cli
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
