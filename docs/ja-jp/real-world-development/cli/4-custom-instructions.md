---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際は、コンテキストが重要です。タスクを特定の方法で進めたい場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も説明できます。フィルタリングを実装したので、Copilot が使用した指示を確認し、ドキュメント標準を追加してコードに適用しましょう。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに適用範囲を限定した指示ファイルが、エージェントにどのように適用されるかを確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントでは、コードの内容を繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入できる場合は、その引数も記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードの変更時にはコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、Copilot がコーディングスタイルや要件を理解するためのコンテキストと設定を与えられます。ライブラリの指定や、記述するコメントの好みなどを含めることもできます。リポジトリ全体に適用する指示も、特定のファイル種類に適用する指示も作成できます。

指示ファイルには、次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関連するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル種類、タスクに適用するガイドラインです。

> [!NOTE]
> ほかの指示形式とサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、あらかじめ指示ファイルが含まれています。変更を加える前に内容を確認し、変更後の影響を比べましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示が含まれています。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭にある `applyTo` フィールドを確認します。このフィールドで、指示を適用するファイルを指定する glob を設定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分割し、参考例へのリンクを記載できます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、追加すべき内容があります。新しい TypeScript に TSDoc コメントを追加するよう、メインの `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` を開き、35行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、フィルタリングの会話を終了して再開します。機能のコンテキストはそのままに、新しい指示を読み込めます。

1. フィルタリングの会話が開いたままの場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しく追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がコンテキストを読み込む方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパスに適用範囲を限定した指示ファイルが、エージェントにどのように適用されるかを確認しました。
- コーディング標準に従うよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な [quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
