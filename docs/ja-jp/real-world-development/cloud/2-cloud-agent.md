---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: カスタム指示][previous-lesson]

多くの組織では、解決されていないセキュリティ課題、更新が必要なレガシーコード、手が回らずバックログに残った機能要望など、技術的負債に直面しています。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に行います。作業が終わると、人がレビューできるドラフト PR を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent では、次のことを確認します。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 範囲を明確にした Issue の重要性
- Copilot への Issue の割り当て

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを作った開発者はドキュメントを整備しておらず、ほとんど存在しません。まず、アプリケーションのエクスポート関数すべてに TSDoc の doc comment を追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、利用者が閲覧を続けられるようにします。洗練された実装はまだ必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロックされていますが、ほかに優先度の高い Issue があります。

こうした作業は優先順位が下がりやすく、Copilot cloud agent に適しています。非同期で作業を任せている間に別の作業に取り組み、後から成果をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者のようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると Copilot は進捗を追跡するドラフト PR を作成し、環境を準備して作業を始めます。処理中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業でも、詳しく調べると複雑な部分があります。[Copilot cloud agent のタスク範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、Copilot を導くカスタム指示を確認しました。まだ確認していない場合は、cloud agent に作業を割り当てる前に、このリポジトリの指示ファイルを読み、後で成果に現れる影響を確認できるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc の doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターで、指示が `**/*.test.ts` に適用されることを確認します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てます。生成されるプルリクエストに、これらの指示に基づく TSDoc コメント、TypeScript 規約、ファイルヘッダーがあるか確認してください。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent 用の開発環境をセットアップする

コードを作成する際は、通常、必要な開発ツールや依存関係を整える環境とセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同じです。cloud agent はソフトウェアエンジニアのように作業します。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]で、作業開始前の環境をカスタマイズできます。このラボでは、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、SQLite データベースのマイグレーションと初期データを利用できるよう、あらかじめ設定しています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各ステップ*に必要な最小限の権限を設定します。Copilot は自身の操作に独自のトークンを使用します。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリを clone する場合は `contents: read` 権限が必要です。セットアップ手順で clone しない場合は、完了後に Copilot が自動的に clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のデータを用意するため、ローカル SQLite データベースをマイグレーションして初期データを投入します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、次の点を確認してください。

- `copilot-setup-steps` という1つのジョブがあり、Copilot が PR 作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあり、リポジトリの **Actions** タブから手動で実行できます。Copilot の実行を待たずに、ワークフローの成功をテストできます。

## ドキュメントを追加する

ドキュメントが重要なのは誰もが理解していますが、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。これは対処されずに生産性を落とし、コードベースの保守や新しいメンバーの参加を難しくする技術的負債です。Copilot はドキュメントの作成を得意としているため、cloud agent に割り当てるのに適した Issue です。必要なドキュメントの作成をバックグラウンドで任せ、後の演習で成果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択します。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された、`[WIP]: Code lacks documentation` のようなタイトルのプルリクエストを開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot が PR に TODO リストを表示するまで、数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を構築し、プロジェクトを分析して、作業方法を検討しています。

13. TODO リストと、実行予定の作業を確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態を表示するには、ウィンドウを更新する必要がある場合があります。

16. セッションをスクロールし、Copilot がコードを調査して状態を把握し、計画を考え、コードを作成する様子を確認します。

この作業には数分かかります。Copilot cloud agent の主な目的は、非同期でタスクを実行し、開発者が別の作業に集中できるようにすることです。もう1つのタスクを割り当てて、アプリケーションへの機能追加にも取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の1つは、作業を分けられることです。デザインチーム向けに関連ゲームセクションを作る作業が短時間であっても、その間に別の作業に取り組めます。これも cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択します。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことが分かります。

![Issue に Copilot の作業中を示す目の絵文字が表示されている](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組んでいます。cloud agent はソフトウェアエンジニアと同じように動作するため、常に監視する必要はありません。作業が完了してからレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、Issue を Copilot に割り当て、非同期で実行できます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに利用できます。

この演習では、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確にした Issue の重要性を学びました。
- Copilot に Issue を割り当てました。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使う方法を確認します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
