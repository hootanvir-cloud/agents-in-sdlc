---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot に知っておいてほしい背景情報がある場合は、それを参照できるようにします。[指示ファイル][instruction-files]を使うと、何をしてほしいかだけでなく、どのように作業してほしいかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに適用範囲を限定した指示ファイルを通して、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に適用される方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、ここではプロンプトの変更前後の比較を行いません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムで横並びに比較しにくいためです。この環境の後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

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

このリポジトリに含まれている指示ファイルを読んでみましょう。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。GitHub の Web UI または起動済みの codespace で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。Copilot とのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの glob で、適用対象のファイルを指定します。ここでは `**/*.test.ts` などに一致する TypeScript テストファイルが対象です。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや既存のプロジェクトファイルへのリンクがあります。これにより、大きな指示セットを小さく再利用可能なファイルに分割し、コード生成時に参考となる例を示せます (パスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc コメントとファイル先頭のコメントヘッダーを追加します。

## リポジトリ全体に適用する標準を追加する

次に行う編集は1つだけです。コード内のドキュメントには TSDoc コメントとファイル先頭のコメントヘッダーを使うという、プロジェクト全体のルールを追加します。後の演習で cloud agent が Issue に取り組む際、この指示を読み込みます。

編集の前にブランチを作成します。cloud agent は Issue の対象ブランチにある指示を読み込みますが、ここでは変更を明確にコミットしておきます。

1. codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を与えるためのファイルです。コードの提案を改善するため、リポジトリの標準を明記しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがまだありません。そのため、生成コードにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に、ファイルのコメントヘッダーと TSDoc コメントを追加する次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体のガイダンスには `.github/copilot-instructions.md` を、特定の言語、ファイル種類、タスクには `*.instructions.md` を使えます。コードコメントの規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は、Issue が対象とするブランチの指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、cloud agent は `main` からブランチを作成します。指示を読み込ませるには、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue を割り当てるとき、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業しても構いません。その場合、ブランチの作成は省略して変更を `main` に直接コミットできます。ここでは実際のプロジェクトでの進め方を再現するため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加しました。この環境の残りの演習で cloud agent が作成するプルリクエストを確認すると、標準が適用されているか分かります。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 用のよりよいカスタム指示を作成する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
