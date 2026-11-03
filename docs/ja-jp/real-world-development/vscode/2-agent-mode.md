---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数ファイルの変更やテスト実行などが必要です。開発者は必要なファイルを探し、変更し、テストを実行し、デバッグし、更新し忘れたファイルを見つけて再度修正する、といった作業を繰り返します。

このような作業に役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内で自律的に作業します。既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、問題が見つかると自動的に修正します。エージェントモードでサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で動作するため、すばやいフィードバックと対話が可能です。cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルをまとめて変更する方法
- Copilot エージェントモードで UI とデータレイヤーにまたがる機能を実装する方法
- コードベースにマージする前に、Copilot が作成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと販売元で絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。残りのデータレイヤー、UI、テストの作業を Copilot エージェントモードで完了します。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接検索します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作には標準 DOM API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクで Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば、`main` に戻っているはずです。
2. Codespace で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

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
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

サイトを起動したら、機能を確認します。主な内容は次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、販売元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加します。まずバックログを確認します。テンプレートからプロジェクトを作成すると、Issue のバックログも自動的に用意されます。GitHub Copilot に項目を表示してもらいましょう。

1. Chat ビューのエージェントドロップダウンから **Agent** を選びます。**Agent** は自律的に計画し、複数ファイルを変更し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルから **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高める意図がありますが、LLM は確率的に動作するので、異なる提案が表示される場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、利用者に最も役立つと考えた項目から優先順位を付けたことを確認できます。

## 指示ファイルを確認する

コード生成をエージェントに依頼する前に、作業のコンテキストを与える指示ファイルを確認します。Web サイトへの機能追加の方法を説明する[ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加する方法について、次のガイダンスを確認します。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - ファイル種別別の指示へのリンク (Astro、Tailwind CSS、Drizzle など)

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクトファイルを参照できます。パスは指示ファイルの場所からの相対パスです。再利用、複雑な指示の分割、Copilot に示す例の共有に役立ちます。

## フィルター機能を実装する

フィルター機能には、少なくとも次の3つの変更が必要です。

- データレイヤー (`src/lib/`) の残りのフィルタリングロジックを追加または調整する
- フィルターの動作をテストするテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

コードベースにマージする前に、テストを実行して成功することも必要です。Copilot エージェントモードに作業を依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選んで新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot はプロジェクトを調べ、機能に関係するファイルを探します。データレイヤーのヘルパー、UI、テストを見つけてから、ファイルを変更しテストを実行します。

   ![Copilot がプロジェクトファイルを調べる様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行します。タスクやコードベースによって数分かかる場合があります。作業中、エディターに **Keep** と **Undo** が表示される場合があります。Copilot が完了するとすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが検出されなくなるまで進めます。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードで Tailspin Toys の Web サイトに機能を追加する方法を確認しました。

- UI とデータレイヤーにまたがる機能を実装しました。
- プロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法を確認しました。
- コードベースにマージする前に、生成された変更とテストをレビューする方法を学びました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの追加を依頼します。

- 既存の指示ファイルを含むベストプラクティスに従います。
- ページサイズを利用者が選択するか、固定値にするかなど、ページネーションの方法を検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot にコンテキストを追加で伝えながら、変更を反復して改善します。これは Copilot と作業する通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
