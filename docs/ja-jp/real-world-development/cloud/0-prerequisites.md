---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備を整えましょう。Tailspin Toys リポジトリの自分用コピーを作成し、cloud agent が使う指示ファイルを編集したり、エージェントが作成した内容をレビューしたりするための [codespace][codespaces] を起動します。

## ワークショップ用リポジトリをセットアップする

作成するコードを保存するため、[テンプレート][template-repository]からリポジトリのインスタンスを作成します。新しいインスタンスには演習に必要なファイルがすべて含まれます。演習を進める間、このリポジトリを使います。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボのリポジトリページで **このテンプレートを使用**を選択し、次に **新しいリポジトリを作成する**を選択して、自分用のコピーを作成します。

    ![「このテンプレートを使用」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。後の演習で参照します。

## Codespace を作成する

次に、codespace を使って演習に取り組みます。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーで直接コードを書き、実行し、デバッグできます。複数のプログラミング言語、拡張機能、ツールに対応した、機能豊富な IDE が用意されています。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する画面](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、すべてのサービスを手作業でインストールするよりはずっと早く完了します。その間に GitHub Copilot のほかの機能を見ておきましょう。次にそれらを使います。

> [!CAUTION]
> 後の演習でこの codespace に戻ります。今はブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できるように作られています。どちらも、スムーズに作業できるよう必要な前提条件が揃った環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、表示されたら **コンテナーで再度開く**を選択してください。codespace と同じ dev container が VS Code によって構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ワークショップ用リポジトリのコピーを作成できました。Copilot cloud agent と一緒に作業するための codespace も作成を始めました。

## 次のステップ

cloud agent が従うカスタム指示を追加しましょう。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
