# Markdown Viewer

Markdownエディタと印刷ツールを統合したシンプルなWebアプリケーションです。

## 機能

- **リアルタイムプレビュー**: 左側のエディタでMarkdownを入力すると、右側にリアルタイムでプレビューが表示されます
- **Mermaid図表サポート**: Mermaidコードブロック（\`\`\`mermaid ... \`\`\`）を図表としてレンダリングします
  - シーケンス図、フローチャート、ガントチャートなど、Mermaidでサポートされているすべての図表タイプに対応
  - 印刷時も図表が含まれます
- **印刷機能**: プレビュー部分のみを印刷できます
- **印刷カスタマイズ**:
  - ヘッダーに印刷時間を表示/非表示
  - ヘッダーにタイトルを表示/非表示
  - カスタム印刷タイトルを設定可能
  - フッターにページ番号（現在ページ/総ページ数）を自動表示

## 使い方

1. 左側の入力エリアにMarkdown形式のテキストを入力します。ローカルに保存した .md または .txt ファイルを左側の入力エリアにドラッグ＆ドロップすると、内容を読み込めます。
2. 右側のプレビューエリアでリアルタイムにプレビューを確認できます
3. 印刷する場合は、以下の設定を行えます：
   - 「ヘッダーに印刷時間を表示」チェックボックスで印刷時間の表示/非表示を切り替え
   - 「ヘッダーにタイトルを表示」チェックボックスでタイトルの表示/非表示を切り替え
   - 「印刷タイトル」フィールドに任意のタイトルを入力
4. 「印刷」ボタンをクリックして印刷ダイアログを開きます
5. 印刷ダイアログから印刷を行います

## 技術仕様

- **Markdownパーサー**: [Marked.js](https://marked.js.org/) (CDN経由)
- **図表ライブラリ**: [Mermaid.js](https://mermaid.js.org/) (CDN経由)
- **対応ブラウザ**: モダンブラウザ（Chrome、Firefox、Edge、Safariなど）
- **依存関係**: 外部ライブラリのみ（CDN経由）

## ファイル構成

```
MarkdownViewer/
├── index.html           # メインのHTMLファイル（単一ファイルで完結）
├── README.md            # このファイル
└── .nojekyll            # GitHub PagesでJekyllを無効化するファイル
```

## GitHub Pagesでの公開

このプロジェクトをGitHub Pagesで公開する場合：

1. リポジトリのSettings > Pagesで、ソースブランチを選択して公開を有効化します
2. `.nojekyll`ファイルが含まれているため、Jekyllは無効化され、`index.html`が直接表示されます
3. 公開後、`https://[ユーザー名].github.io/[リポジトリ名]/` でアクセスできます

**注意**: `.nojekyll`ファイルがない場合、GitHub PagesはJekyllを使用してREADME.mdを自動的にレンダリングするため、`index.html`ではなくREADME.mdが表示されることがあります。

## Renderでのホスティング

このプロジェクトを [Render](https://render.com/) の Static Site で公開する場合の手順と注意点です。

### 手順

1. [Render Dashboard](https://dashboard.render.com/) で **New > Static Site** を選択
2. GitHub のリポジトリを接続
3. デプロイするブランチを指定（例: `main`）
4. **Build Command**: **空のまま**（ビルド不要な静的HTMLのため）
5. **Publish Directory**: **`.`**（ルートディレクトリに `index.html` があるため）
6. **Create Static Site** で作成

### 注意点

- **ビルドコマンド**: 本システムはビルド不要のため、Build Command は**空欄**にしてください。何か入力すると「コマンドがない」等で失敗する場合があります。
- **Publish Directory**: 必ず **`.`** を指定してください。空や `public` などにすると `index.html` が配信されません。
- **依存関係の自動検出**: Render はデフォルトで npm 等の依存関係を検出しようとします。本プロジェクトは `package.json` がなく CDN のみのため問題になりませんが、ビルドがスキップされるか短時間で完了する想定です。
- **無料プラン**: 静的サイトは無料でデプロイできますが、ワークスペースの**月間送信帯域（Outbound Bandwidth）**の枠を消費します（Hobby で 100 GB/月など）。枠超過時は有料になります。
- **スリープ**: 静的サイトは Web Service と異なり**スリープしません**。常時 CDN から配信されるため、初回アクセス待ちは発生しません。
- **URL**: デプロイ後は `https://[サービス名].onrender.com` でアクセスできます。カスタムドメインも設定可能です（無料プランではワークスペースあたり 2 ドメインまで）。

### render.yaml で設定する場合（任意）

リポジトリに `render.yaml` を置くと、同じ設定をコードで管理できます。

```yaml
services:
  - type: web
    name: markdown-viewer
    runtime: static
    buildCommand: "true"   # ビルド不要のため no-op（空だと失敗する場合があるため）
    staticPublishPath: .
```

※ Static Site は `type: web` と `runtime: static` の組み合わせで定義します。Dashboard で「Static Site」を選んだ場合と同じです。

## 注意事項

- インターネット接続が必要です（Marked.jsとMermaid.jsをCDNから読み込むため）
- 印刷機能はブラウザの印刷機能を使用します
- テーブルの印刷時は自動的に改行されるように調整されます
- Mermaid図表は印刷時も正しく表示されます

## ライセンス

このプロジェクトは単一のHTMLファイルで構成されており、自由に使用・改変できます。
