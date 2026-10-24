---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、重点的な指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供することで、特定の種類の作業を効率よく進められます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなど、チームの専門知識を再利用できるエージェントにまとめられます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはユーザー共通の `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記載します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、ある程度共通点があります。どちらも主に Markdown ファイルで定義し、AI がタスクをどのように実行するかを伝えます。区別するには、**カスタムエージェントが作業者で、スキルがツール**と考えると分かりやすいでしょう。

カスタムエージェントには専用のコンテキストウィンドウがあり、作業の一環としてスキルやほかのエージェントを調整できます。この演習では、アクセシビリティ用カスタムエージェントがガイドラインに照らしてサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフロースキルやテストを実行、管理するスキルなどを呼び出せます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの機能と同じく、環境やシナリオに合う方法が見つかるまで、試行と改善を重ねてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントについて次のことを学びます。

- カスタムエージェントの定義方法。
- Copilot Chat でアクセシビリティエージェントを使い、高コントラストモードを実装する方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で読みづらいという利用者の声が寄せられました。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられる高コントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成しましょう。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに作成されています。Copilot をどのようにガイドするかを理解するため、内容を確認しましょう。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します。演習 3 のフィルタリング PR とは別のブランチで進めます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な役割。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴と間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` で定義されたすべてのカスタムエージェントが Copilot Chat ビュー下部のエージェントドロップダウンに表示されます。カスタムエージェントを選ぶと、その指示とツールの範囲内でチャットセッションを進められます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

1. Chat ビューのエージェントドロップダウンで **Agent** を選択します。すでに選択されている場合はそのまま進みます。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントドロップダウンを選択します。現在のエージェントが表示されます。既定では **default** です。
3. 使用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調査し、変更を提案して、プロジェクトファイルに適用します。編集内容は、ファイルパスと差分とともにチャット内に表示されます。
6. 変更が反映されるたびに **Files changed** の表示が更新され、このエージェントが変更したファイルの一覧を確認できます。

> [!NOTE]
> この処理には数分かかることがあります。Copilot はリポジトリに実際の変更を加えます。Astro コンポーネント、CSS、関連テストなど、既存ファイルを編集する場合があります。

次の演習では、作業の進行中に確認して、必要に応じて方向付けします。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである GitHub Copilot の[カスタムエージェント][custom-agents]を扱いました。カスタムエージェントにチームの専門知識と標準をまとめ、特定の種類の作業を効率よく進められます。

次のことを学びました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は、[エージェントの作業を監視して方向付けます][next-lesson]。変更を進行中に確認し、同じセッションでライトモードの切り替えも追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
