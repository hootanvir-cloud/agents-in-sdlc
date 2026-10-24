---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot に知っておいてほしい背景情報がある場合は、それを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。重要な方法の1つが[指示ファイル][instruction-files]です。コード生成のガイダンスを伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに適用範囲を限定した指示ファイルを通して、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に適用される方法を確認します。
- 現在の指示を使って、フィルタリングに必要な最初のデータスライス (パブリッシャー用ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映される様子を確認します。
- 指示の更新とフィルタリング用データスライスを `main` にコミットし、次の演習で Copilot が更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部に従わないことがあります。Copilot の動作は確率的です。この演習では、出力を一字一句揃えるのではなく、指示を更新した後に動作がどのように変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データレイヤーには必ず単体テストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コード内のドキュメントには TSDoc コメントを使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業できるように必要な情報を与えます。

### カスタム指示

カスタム指示を使うと、Copilot がコーディングスタイルや要件を理解するためのコンテキストと設定を与えられます。好みのコーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル種類に適用する指示も作成できます。

指示ファイルには、次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot へのほとんどのチャットや CLI リクエストに関係するプロジェクト全体の情報を記載します。使用技術、構築しているものの概要、ベストプラクティス、共通のガイダンスなどを含められます。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル種類に適用する指示ファイルです。TypeScript や Astro など特定の言語や、UI コンポーネントまたは単体テストの作成といったタスクのガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準に準拠した指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。サンプルプロジェクトの例から、一般的なアプローチを確認できます。

- `copilot-instructions.md` には、構築しているものの説明、プロジェクトの構成、全体的なコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` ファイルには、ファイル種類 (単体テスト、Astro コンポーネント、データレイヤー) や特定のタスクに関する指示を記載します。
- 自然な言葉を使い、ガイダンスを明確にします。コードのよい例と避けるべき例を示します。

指示ファイルの作成方法も、AI の使い方と同じく1つに決まっているわけではありません。試行を重ねて、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示が見つかります。
>
> Copilot に指示ファイルの生成を依頼することもできます。各環境での手順は異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`)。該当する場合は、各環境向けのレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースが集まった [awesome-copilot][awesome-copilot] をご覧ください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれている指示ファイルを読んでみましょう。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭にある `applyTo` フィールドを確認します。これはリポジトリのルートからの glob で、適用対象のファイルを指定します。ここでは、`**/*.test.ts` などに一致する TypeScript テストファイルが対象です。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、既存のプロジェクトファイルへのリンクがあります。これにより、大きな指示セットを小さく再利用できるファイルに分け、コード生成時に参考となる例を示せます (パスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc コメントとファイル先頭のコメントヘッダーを追加します。

## 変更用のブランチを作成する

ここでは、変更を別ブランチで進めるベストプラクティスに従います。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な手順は該当箇所で説明します。

カスタム指示の効果を確認するため、最初に現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新した後に、同じプロンプトを再送信して違いを比べます。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを読み取れるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを作成する場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査し、コードを変更します。ヘルパーファイルとテストに変更が及ぶこともあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、パブリッシャーの型付き配列を返す型付き関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に従っています。
7. 提案されたコードに TSDoc コメントとファイル先頭のコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が高まるかどうかを確認するのが目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を与えるためのファイルです。コードの提案を改善するため、リポジトリの標準を明記しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがまだありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に、ファイルのコメントヘッダーと TSDoc コメントを追加する次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体のガイダンスには `.github/copilot-instructions.md` を、特定の言語、ファイル種類、タスクには `*.instructions.md` を使えます。コードコメントの規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使うコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも、リポジトリ内のほかのファイルと同じようにソース管理で扱います。指示の更新とブランチをコミットして、リポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドを実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新とヘルパーの基礎となる変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュできました。バックログのドキュメント標準に関する Issue と紐付けて、プルリクエストを作成します。手動で行うこともできますが、プロジェクトに接続済みの GitHub ツールを使い、Copilot に Issue を探して PR を作成し、マージしてもらいましょう。

> [!NOTE]
> この演習では、初めて Copilot に GitHub の操作を依頼します。プロジェクトのテンプレートでは、GitHub のツールがワークスペースにあらかじめ接続されています。セットアップは不要ですが、Copilot が初めてツールを使う際に VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習では、この接続のしくみ (Model Context Protocol) を学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成して、指示の更新とパブリッシャーヘルパーの追加の両方を説明するよう Copilot に依頼します。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に代わって GitHub の操作を行うために **Allow** を求められた場合は、コマンドを確認し、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカルのブランチを `main` に戻します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に代わって GitHub の操作やシェルコマンドを実行するために **Allow** を求められた場合は、コマンドを確認し、適切な場合に **Allow** を選択します。

これで GitHub Copilot を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の指示を使ったコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加しました。
- 同じプロンプトを再実行し、提案コードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーの基礎となる変更を `main` にコミットし、次の演習で使えるようにしました。

次は、[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体へ新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 用のよりよいカスタム指示を作成する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
