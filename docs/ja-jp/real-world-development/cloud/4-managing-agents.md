---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザー体験の改善と機能追加を目的とする3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクの進捗を監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェントページ][agents-page]では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べ、進行中のセッションに追加の指示を与えられます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加の機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているところなので、ライトモードも含めるのが自然です。エージェントがハイコントラストとライトモードの両方を追加するよう、作業中の指示を更新します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードのドキュメントを更新する
   - ゲーム変更用の API を生成する
   - Web サイトにハイコントラストモードを追加する
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。たとえば、コードのチェックアウト、作業環境の作成、MCP サーバーのセットアップ、タスクを完了するための作業などです。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。追加の監視に使える PR と変更ファイルが表示されます。

## cloud agent の作業に指示を追加する

実行中のタスクを確認したので、ハイコントラストモードに取り組んでいる Copilot にライトモードも追加するよう依頼します。

1. ハイコントラストモードを追加するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

   ![ミッションコントロール上のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![「Steer active session while Copilot is working」ダイアログが表示された cloud agent タスク](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に加えたことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順が終わると、新しい依頼を作業の流れに取り込みます。

これには数分かかります。その間に、ここまでに学んだことを振り返ってください。

## まとめと次のステップ

Copilot agents ページを使って、GitHub Copilot cloud agent のタスクを監視し、指示を追加する方法を確認しました。ここでは、進行中と完了済みのタスクを追跡し、作業内容を調べ、進行中のタスクに追加の指示を与えて範囲を調整できます。

このレッスンでは、次のことを行いました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに指示を追加し、機能の拡張を依頼しました。

Copilot がアクセシビリティ機能を完成させている間に、[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
