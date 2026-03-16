# AGENTS.md

このファイルは、AIコーディングエージェントがこのリポジトリで作業する際のガイドラインです。

## プロジェクト概要

1U猫キー屋の企業Webサイト。Hugo静的サイトジェネレーター（v0.147.9）とPaperModテーマを使用した日本語サイトです。JekyllからHugoへの移植が完了し、現在はHugoベースで運用中。

**実行環境**: Windows環境  
**言語**: 日本語（ユーザとの対話も日本語で行う）  
**本番URL**: https://ymkn.github.io/1u-nekokey/

## ビルド・テストコマンド

### 開発サーバー起動
```bash
hugo server -D
```
ローカル開発サーバーを起動（ドラフト記事も表示）。デフォルトでhttp://localhost:1313/で確認可能。

### 本番ビルド
```bash
hugo --minify
```
本番環境用の最適化されたサイトを`public/`ディレクトリに生成。

### 本番ビルド（完全版）
```bash
hugo --gc --minify
```
ガベージコレクション付き本番ビルド。GitHub Actionsで使用されるコマンド。

### サブモジュール更新（テーマ更新）
```bash
git submodule update --remote --merge themes/PaperMod
```

### 新しいコンテンツ作成
```bash
# 新しいニュース記事
hugo new content news/記事名.md

# 新しい製品ページ
hugo new content products/製品名.md

# 新しい固定ページ
hugo new content ページ名.md
```

### テスト・検証
Hugo自体にユニットテストはありませんが、以下のコマンドでビルド検証を行います：
```bash
# ビルドエラーチェック
hugo --minify

# 開発サーバーでプレビュー確認
hugo server -D
```

## プロジェクト構造

```
1u-nekokey/
├── hugo.toml              # サイト設定ファイル（GitHub Pages用設定済み）
├── content/               # サイトコンテンツ（Markdown）
│   ├── news/             # ニュース記事
│   ├── products/         # 製品情報ページ
│   ├── about.md          # Aboutページ
│   └── support.md        # サポートポリシー
├── layouts/               # カスタムレイアウト・テンプレート
│   ├── index.html        # トップページレイアウト
│   ├── products/list.html # 製品一覧ページ
│   ├── partials/         # 部分テンプレート
│   └── shortcodes/       # カスタムショートコード
│       ├── gallery.html      # PhotoSwipe 5対応ギャラリー
│       ├── feature_row.html  # 製品特徴カード
│       ├── button.html       # 統一デザインボタン
│       └── products_grid.html # 製品一覧グリッド
├── assets/css/extended/   # カスタムCSS
│   └── custom.css        # サイト全体のカスタムスタイル
├── static/                # 静的ファイル（画像、favicon等）
│   └── images/           # 画像ファイル
├── themes/PaperMod/       # PaperModテーマ（gitサブモジュール）
└── .github/workflows/     # GitHub Actions設定
    └── deploy.yml        # 自動デプロイ設定
```

## コーディング規約

### Hugoテンプレート（.html）

#### インデント・フォーマット
- スペース2個でインデント
- Go template構文は `{{ }}` で記述
- 閉じタグは適切にインデント

#### 変数命名
- キャメルケース使用: `$gallery`, `$featureRow`, `$layout`
- パラメータアクセス: `.Page.Params.変数名` または `.Params.変数名`
- Hugo変数: `.Site`, `.RelPermalink`, `.Title`, `.Date` など

#### コメント
```html
<!-- コメントは日本語でわかりやすく -->
{{ /* Go templateコメントも使用可 */ }}
```

#### エラーハンドリング
```html
{{ if $variable }}
  <!-- 変数が存在する場合の処理 -->
{{ else }}
  <!-- 変数が存在しない場合の処理またはコメント -->
{{ end }}
```

### Markdown（.md）

#### Front Matter形式
YAML形式（`---`で囲む）を使用：
```yaml
---
title: "ページタイトル"
description: "SEO用の説明文（必須）"
date: YYYY-MM-DD
draft: false
image: /1u-nekokey/images/path/to/image.jpg  # 製品ページの場合
---
```

#### 画像パス
すべての画像パスはbaseURLを含める：
```markdown
![代替テキスト](/1u-nekokey/images/製品名/image.jpg)
```

#### ショートコード使用例
```markdown
<!-- ギャラリー（3列表示） -->
{{< gallery layout="third" >}}

<!-- 特徴カード -->
{{< feature_row >}}

<!-- ボタン -->
{{< button url="https://example.com" text="ボタンテキスト" >}}
```

### CSS（custom.css）

#### スタイル記述
- セレクタは具体的に記述（PaperModテーマとの競合回避）
- `!important` は必要な場合のみ使用（テーマ上書き時）
- カラーコードは小文字で統一: `#ead6bb`, `#e3344a`

#### ブランドカラー定義
```css
:root {
    --theme: #ead6bb;          /* 背景色 */
    --entry: #ead6bb;
}

/* リンク色 */
a { color: #e3344a !important; }

/* ホバー色 */
a:hover { color: #c02d3f !important; }
```

#### レスポンシブ対応
```css
@media (max-width: 992px) { /* タブレット */ }
@media (max-width: 768px) { /* 小型タブレット */ }
@media (max-width: 576px) { /* スマートフォン */ }
@media (max-width: 480px) { /* 小型スマートフォン */ }
```

## 製品ページデザインルール

### 必須Front Matter
```yaml
title: "製品名"
description: "製品の簡潔な説明（SEO重要）"
date: YYYY-MM-DD
image: /1u-nekokey/images/products/製品名/main.jpg
gallery:
  - url: /1u-nekokey/images/products/製品名/image-01.jpg
    image_path: /1u-nekokey/images/products/製品名/image-01-th.jpg
feature_row:
  - image_path: /1u-nekokey/images/products/製品名/feature-01.png
    title: "特徴タイトル"
    excerpt: "特徴の説明文"
    url: https://example.com  # オプション
    btn_label: "ボタンラベル"  # オプション
```

### コンテンツ構成順序
1. メイン画像
2. ギャラリー: `{{< gallery layout="third" >}}`
3. 特徴セクション: `{{< feature_row >}}`
4. 仕様表（Markdown table）
5. 購入ボタン: `{{< button url="..." text="..." >}}`

### デザイン仕様
- コンテンツ幅: 850px（最大）
- フォント: Barlow + Noto Sans JP
- h2見出し: 1.8rem、#0e2431色、下線#bbab96
- 表のボーダー: 水平線のみ #bbab96色、0.9remフォントサイズ
- ボタン背景: #e3344a

## Git・デプロイ

### GitHub Actions
- `main` ブランチへのpushで自動デプロイ
- Hugo v0.147.9（extended版）を使用
- ビルド成果物は `public/` ディレクトリ
- GitHub Pagesへ自動デプロイ

### コミットメッセージ
日本語で簡潔に記述。例:
```
製品ページにHenge40を追加
```
```
ギャラリーショートコードのPhotoSwipe対応を改善
```

## 重要な注意事項

1. **日本語優先**: すべてのコンテンツとコミュニケーションは日本語で行う
2. **画像パス**: 必ずbaseURL (`/1u-nekokey/`) を含める
3. **SEO対策**: すべてのページに `description` を設定
4. **テーマ更新**: PaperModはgitサブモジュールなので、更新時は `git submodule update` を使用
5. **ビルド確認**: コンテンツ変更後は必ず `hugo server -D` でプレビュー確認
6. **レスポンシブ**: すべての新機能はモバイル対応必須
7. **Front Matter必須**: `title`, `description`, `date` は必ず設定

## トラブルシューティング

### ビルドエラー
```bash
# キャッシュクリア
hugo --gc

# 詳細ログ出力
hugo --verbose
```

### テーマが見つからない
```bash
# サブモジュール初期化
git submodule update --init --recursive
```

### 画像が表示されない
- パスに `/1u-nekokey/` プレフィックスを確認
- ファイル名の大文字小文字を確認（Windowsは区別しないがLinuxは区別する）
