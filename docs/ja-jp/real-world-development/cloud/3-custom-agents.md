---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、重点的な指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供することで、特定の種類の作業を効率よく進められます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなど、チームの専門知識を再利用できるエージェントにまとめられます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはユーザー共通の `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記載します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、ある程度共通点があります。どちらも主に Markdown ファイルで定義し、AI がタスクをどのように実行するかを伝えます。区別するには、**カスタムエージェントが作業者で、スキルがツール**と考えると分かりやすいでしょう。

カスタムエージェントには専用のコンテキストウィンドウがあり、作業の一環としてスキルやほかのエージェントを調整できます。このラボでは、アクセシビリティ用カスタムエージェントがガイドラインに照らしてサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフロースキルやテストを実行、管理するスキルなどを呼び出せます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの機能と同じく、環境やシナリオに合う方法が見つかるまで、試行と改善を重ねてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントについて、次の内容を確認します。

- カスタムエージェントの定義方法。
- カスタムエージェントにタスクを割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で読みづらいという利用者の声が寄せられました。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられる高コントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成しましょう。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに作成されています。Copilot をどのようにガイドするかを理解するため、内容を確認しましょう。

codespace に戻り、アクセシビリティ用カスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な役割。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴と間違い。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを管理する中心的な場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、方向を変えたり追加のガイダンスを与えたりできます。まず Copilot に高コントラストモードの作成を依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択し、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して空の Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. 使用可能なカスタムエージェントの一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントを割り当てる画面で、Accessibility agent が選択されている様子](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクに取り組み始めます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである GitHub Copilot の[カスタムエージェント][custom-agents]を扱いました。カスタムエージェントにチームの専門知識と標準をまとめ、特定の種類の作業を効率よく進められます。

次の内容を確認しました。

- カスタムエージェントの定義方法。
- カスタムエージェントにタスクを割り当てる方法。

Copilot が高コントラストモードを実装している間、次は Mission control から[エージェントセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
