# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

このプロジェクトは「1U猫キー屋」の企業Webサイトです。Hugo静的サイトジェネレーターとPaperModテーマを使用しています。

**プロジェクト状況**: JekyllからHugoへの移植が完了。今後はHugoベースで運用・更新を行います。

## 重要な開発コマンド

### 開発サーバー起動
```bash
hugo server -D
```
ローカル開発サーバーを起動（ドラフト記事も表示）

### 本番ビルド
```bash
hugo --minify
```
本番環境用の最適化されたサイトを`public/`ディレクトリに生成

### 新しいコンテンツ作成
```bash
# 新しいブログ記事
hugo new content posts/記事名.md

# 新しい製品ページ
hugo new content products/製品名.md

# 新しい固定ページ
hugo new content ページ名.md
```

### テーマの更新
```bash
git submodule update --remote --merge themes/PaperMod
```

## プロジェクト構造

- `hugo.toml` - サイト設定ファイル（GitHub Pages用に設定済み）
- `content/` - サイトコンテンツ
  - `news/` - ニュース記事
  - `products/` - 製品情報ページ
  - `support.md` - サポートポリシー
- `themes/PaperMod/` - PaperModテーマ（gitサブモジュール）
- `static/` - 静的ファイル（画像、CSS等）
- `layouts/` - カスタムレイアウト（必要に応じて）

## 設定情報

- **サイト名**: 1U猫キー屋
- **テーマ**: PaperMod
- **言語**: 日本語
- **本番URL**: https://ymkn.github.io/1u-nekokey/
- **GitHub Pages**: 対応済み

## 開発時の注意点

- ユーザとの対話は日本語で行うこと
- 実行環境はWindows環境
- コンテンツ作成時は日本語のメタデータを適切に設定
- 画像ファイルは`static/images/`に配置
- SEO対策のためメタディスクリプションを必ず設定

## 製品ページデザインルール

### 基本レイアウト
- **コンテンツ幅**: 800px
- **フォント**: Barlow + Noto Sans JP
- **背景色**: #ead6bb

### 見出しスタイル
- **h2見出し**: 1.8rem、#0e2431色、下線#bbab96、上マージン4rem、下マージン1.5rem

### コンテンツ構成順序
1. **メイン画像**: 製品の代表画像
2. **ギャラリー**: `{{< gallery layout="third" >}}` - PhotoSwipe対応3列表示
3. **特徴セクション**: `{{< feature_row >}}` - 3列カード、画像中央・テキスト左揃え
4. **仕様表**: Markdown表、水平線のみ#bbab96色、文字サイズ0.9rem
5. **購入ボタン**: `{{< button url="URL" text="テキスト" >}}` - #e3344a背景

### 必要なFront Matter
```yaml
title: "製品名"
description: "製品説明"
date: YYYY-MM-DD
image: /1u-nekokey/images/products/製品名/main.jpg
gallery: [url, image_path配列]
feature_row: [image_path, title, excerpt, url, btn_label配列]
```

## 実装済み機能

### カスタムショートコード
- `{{< gallery >}}` - PhotoSwipe 5対応ギャラリー（layout="third"で3列表示）
- `{{< feature_row >}}` - 製品特徴カード（3列レスポンシブ）
- `{{< button >}}` - 統一デザインボタン
- `{{< products_grid >}}` - 製品一覧グリッド

### カスタムレイアウト
- `layouts/index.html` - トップページ（最新ニュース3件 + 製品一覧）
- `layouts/products/list.html` - 製品ページ一覧（重複コンテンツ回避）

### スタイリング
- ブランドカラー: 背景#ead6bb、リンク#e3344a
- Google Fonts: Barlow + Noto Sans JP
- レスポンシブデザイン対応
- PhotoSwipe 5統合

### 移植完了コンテンツ
- News記事（キーボードマーケット出展情報など）
- Products（各種キーボード製品ページ）
- Support（サポートポリシー）

## Jekyll→Hugo変換ルール（参考）
- Jekyll `{% include gallery %}` → Hugo `{{< gallery >}}`
- Jekyll `{% include feature_row %}` → Hugo `{{< feature_row >}}`
- 通常リンク → `{{< button >}}`ショートコード
- Markdown表にヘッダー行と区切り行を追加
- Front Matter: `layout`削除、`description`追加