# My Tools — 公開ツールカタログサイト

AIと一緒に開発した公開ツール・ユーティリティ・開発支援ツールを一覧できる静的カタログサイトです。  
Zero-Dependency（外部ライブラリ不要・ビルド不要・HTML/CSS/Vanilla JS）で構築されており、GitHub Pages上でそのまま高速に動作します。

---

## 🎯 コンセプト

```text
                 ┌─────────────┐
                 │ GitHub Pages│
                 │   ツール一覧  │ （目録・カタログ）
                 └──────┬──────┘
                        │
             ┌──────────┴──────────┐
             ↓                     ↓
       ┌──────────┐          ┌──────────┐
       │  GitHub  │          │   note   │
       │ ソースコード│          │ 開発ストーリー│ （実物・解説）
       └──────────┘          └──────────┘
```

- **GitHub**: 実物（ソースコード、Issue、リポジトリ）
- **note**: 物語（開発経緯、解決した面倒、使い方）
- **GitHub Pages**: 目録（現在公開されているツールの検索・一覧入口）

---

## 📁 ディレクトリ構成

```text
tool-catalog-system/
│
├── index.html                  # メインページ（セマンティック構造・OGP・CSP対応）
├── css/
│   └── style.css               # ミニマル・ダークUI（レスポンシブ対応）
├── js/
│   └── app.js                  # tools.json動的描画・リアルタイム検索・カテゴリ絞り込み
├── data/
│   └── tools.json              # ツールマスターデータ（1件追加するだけで一覧反映）
├── scripts/
│   ├── config.json             # 同期スクリプト設定ファイル（マガジンID・除外ワード等）
│   ├── config.example.json     # 設定テンプレート（流用時のお手本）
│   ├── sync_tools.py           # noteマガジンから最新記事を自動同期するスクリプト
│   └── update_seo.py           # JSON-LD構造化データおよびサイトマップ自動生成
├── サイト確認.bat               # 【Windows用】ローカルプレビュー起動バッチ
├── 記事同期.bat                 # 【Windows用】ワンクリック同期＆GitHub自動プッシュ
├── robots.txt                  # 検索エンジンクローラー制御
├── sitemap.xml                 # サイトマップ
├── favicon.ico                 # サイトアイコン
├── .nojekyll                   # GitHub Pages Jekyll無効化フラグ
├── LICENSE                     # MIT License
└── README.md                   # 本ドキュメント
```

> **Note**: `記事同期.bat` および `サイト確認.bat` は Windows 環境向けの自動化スクリプトです。Mac / Linux 環境の場合は、ターミナルから `python scripts/sync_tools.py` や `python -m http.server 8085` を直接実行してください。

---

## 🛠️ 新しいツールの追加方法

### 方法1: `data/tools.json` を直接編集（推奨・最も確実）

`data/tools.json` の配列に新しいツールのオブジェクトを追加します：

```json
{
  "id": "my-new-tool",
  "name": "ツール名",
  "subtitle": "短いキャッチコピー",
  "category": "AI開発", // "AI開発" | "開発支援" | "ファイル・PC" | "その他"
  "description": "何のツールかを1文で説明",
  "why": "どんな面倒・課題を解決するために作ったか",
  "features": [
    "主な特徴1",
    "主な特徴2"
  ],
  "github_url": "https://github.com/your-username/...",
  "web_url": "https://your-username.github.io/.../", // なければ空文字 ""
  "note_url": "https://note.com/your-note/n/...",
  "eyecatch": "サムネイル画像URL",
  "tags": ["AI開発", "Webツール"],
  "release_date": "2026-10-25",
  "status": "公開中"
}
```

追加後、`python scripts/update_seo.py` を実行すると、`index.html` 内の構造化データ（JSON-LD）と `sitemap.xml` が自動更新されます。

### 方法2: 同期スクリプトを実行（noteマガジン連携）

`scripts/config.json` にご自身の note マガジンIDや GitHub ユーザー名を設定した後、以下のコマンドを実行します：

```bash
python scripts/sync_tools.py
```
Windows の場合は `記事同期.bat` をダブルクリックするだけで、同期から GitHub Pages へのデプロイ（git commit & push）まで一括実行されます。

---

## ⚙️ 独自環境へのカスタマイズ

他者が本リポジトリを流用して独自のツールカタログを作成する場合、`scripts/config.json` を編集してください（雛形として `scripts/config.example.json` を同梱しています）：

```json
{
  "magazine_id": "your_note_magazine_id",
  "github_username": "your_github_username",
  "note_username": "your_note_username",
  "catalog_repo_name": "your-catalog-repo",
  "exclude_keywords_title": [
    "メンバーシップ限定",
    "会員限定",
    "アーカイブ"
  ],
  "exclude_keywords_body": [
    "メンバーシップ限定記事"
  ]
}
```

---

## ⚠️ 既知の制限事項

- **カテゴリの追加・変更**:
  - 現在のバージョンでは、新しいカテゴリを追加または名称変更する際、`index.html`（タブボタン・カウンタID）、`js/app.js`（集計・クラス判定ロジック）、`css/style.css`（バッジカラー）の手動修正が必要です。
- **今後のリファクタリング予定**:
  - 重複しているSVGアイコンマークアップの共通コンポーネント化
  - マジックナンバーおよびカテゴリ文字列の定数（Enum）集約
  - `app.js` の責務別モジュール分割

---

## 🚀 拡張性について（将来の可能性）

本システムは現在、作者自身のツールカタログとして最適化されています。

今後、利用者からの要望に応じて以下のような拡張を行い、**「あらゆる個人開発者が自身のツールカタログとして即座に流用できる汎用テンプレート」**へと進化させていく予定です：

1. **カテゴリの完全動的生成**: `tools.json` に存在するカテゴリを自動集計し、タブやバッジを自動生成する機能
2. **多言語（日英）対応**: UIラベルおよびツール説明のバイリンガル切り替え
3. **タグ絞り込みの高度化**: 複数タグによるクロス検索・AND/OR検索

### 相談・機能要望の受付
カタログテンプレートの機能拡張に関するご要望や、AIを活用した個人開発・Webツール構築のご相談は、下記フォームよりお気軽にお寄せください：

👉 **[Zero Dev 相談・要望フォーム](https://zero-dev.jp/consult/)**

---

## 🌐 GitHub Pages への公開手順

本リポジトリをご自身の GitHub Pages で公開する手順：

1. **GitHub に新しい公開リポジトリを作成**（例: `my-tools` または `tool-catalog`）
2. **ファイルをプッシュ**
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Tool Catalog System"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```
3. **GitHub リポジトリ設定で GitHub Pages を有効化**
   - リポジトリの **Settings** > **Pages** を開く
   - **Source** を `Deploy from a branch` に設定
   - **Branch** を `main` / `/(root)` を選択して **Save**
4. 数分後、`https://<your-username>.github.io/<your-repo>/` でカタログサイトが公開されます。

---

## 📄 ライセンス

MIT License です。詳細は [LICENSE](LICENSE) を参照してください。

© 2026 Zero Dev (tk030)
