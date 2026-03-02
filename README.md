# 障害年金相談ランディング - プロジェクト構成

## プロジェクト概要

このプロジェクトは、障害年金相談サービスのランディングページです。BEM (Block Element Modifier) CSS方法論とSCSSを使用して構築されています。

## ファイル構成

```
New/
├── index.html                 # メインHTMLファイル
├── css/
│   └── main.css              # コンパイル済みCSS（本番用）
├── scss/
│   ├── main.scss             # SCSS メインファイル
│   ├── components/           # コンポーネント固有のスタイル
│   │   ├── _header.scss      # ヘッダーコンポーネント
│   │   ├── _nav.scss         # ナビゲーションコンポーネント
│   │   ├── _hero.scss        # ヒーローセクションコンポーネント
│   │   ├── _button.scss      # ボタンコンポーネント
│   │   └── _section.scss     # セクション/メインコンテンツコンポーネント
│   └── utilities/            # グローバルユーティリティ
│       ├── _variables.scss   # カラー、フォント、サイズ変数
│       ├── _mixins.scss      # 再利用可能なMixin（メディアクエリなど）
│       └── _reset.scss       # リセット＆ベーススタイル
├── assets/                   # 画像やメディアファイル
└── main.css                  # 古いCSS（削除予定）

```

## BEM命名規則

このプロジェクトでは、BEM (Block Element Modifier) CSS方法論を採用しています。

### 基本構造

```
.block { }           /* メインコンポーネント */
.block__element { }  /* ブロック内の要素 */
.block--modifier { } /* バリエーション */
```

### 実装例

#### ヘッダーコンポーネント
```html
<header class="header">
  <div class="header__content">
    <div class="header__brand">...</div>
    <div class="header__actions">
      <a class="button button--primary">...</a>
    </div>
  </div>
</header>
```

#### ナビゲーションコンポーネント
```html
<nav class="nav">
  <ul class="nav__list">
    <li class="nav__item nav__item--active">ホーム</li>
    <li class="nav__item">障害年金について</li>
  </ul>
</nav>
```

#### セクションコンポーネント
```html
<section class="section">
  <h3 class="section__title section__title--primary-blue">タイトル</h3>
  <ul class="section__list">
    <li class="section__item">リストアイテム</li>
  </ul>
</section>
```

## SCSSの構造

### variables (_variables.scss)
- カラー定義
- フォントファミリー
- フォントサイズスケール
- スペーシングスケール
- ブレークポイント

### mixins (_mixins.scss)
- レスポンシブデザイン用のメディアクエリヘルパー
- よく使用されるFlexboxパターン
- テキストユーティリティ

### reset (_reset.scss)
- ブラウザのデフォルトスタイルをリセット
- ベーススタイルを設定

## カラーパレット

```
$primary-green:  #327C49   /* メインCTA */
$primary-blue:   #327C7D   /* サブカラー */
$accent:         #EA916A   /* アクセントカラー */
$bg-beige:       #EBE1D2   /* 背景色 */
$light-beige:    #FBF0DF   /* ライトベージュ */
$brown:          #8b5e44   /* ナビゲーション背景 */
$dark-brown:     #6f3f2b   /* テキスト色 */
```

## レスポンシブデザイン

プロジェクトはモバイルファーストアプローチを採用しており、以下のブレークポイントを使用しています：

- `sm` (640px)
- `md` (768px)
- `lg` (1024px)
- `xl` (1280px)

### Mixinの使用方法

```scss
@include respond-to('md') {
  /* md以上の幅でのスタイル */
}
```

## コンポーネント一覧

### Header (.header)
- `header__content` - コンテナ
- `header__brand` - ブランド名
- `header__brand-title` - ブランドタイトル
- `header__actions` - CTA ボタン領域

### Navigation (.nav)
- `nav__list` - ナビゲーションリスト
- `nav__item` - ナビゲーションアイテム
- `nav__item--active` - アクティブ状態

### Hero (.hero)
- `hero__overlay` - 背景オーバーレイ
- `hero__content` - メインコンテンツエリア
- `hero__text` - テキスト領域
- `hero__title` - メインタイトル
- `hero__subtitle` - サブタイトル
- `hero__box` - CTA ボックス
- `hero__box-text` - ボックス内のテキスト
- `hero__box-phone` - 電話番号表示

### Button (.button)
- `button--primary` - プライマリボタン

### Section (.section)
- `section__title` - セクションタイトル
- `section__title--primary-blue` - ブルータイトル
- `section__subtitle` - サブタイトル
- `section__content` - 白背景コンテンツエリア
- `section__list` - リスト
- `section__list--decimal` - 番号付きリスト
- `section__item` - リストアイテム
- `section__text` - テキスト

### Main (.main)
- `main__sections` - セクションコンテナ

## 開発ガイドライン

### 新規コンポーネント追加時

1. `scss/components/` に新しい `_component-name.scss` ファイルを作成
2. BEM命名規則に従ってクラスを定義
3. `scss/main.scss` にインポートを追加
4. HTMLのコンポーネント名とクラス名を一致させる

### 色やサイズの変更

すべての値は `scss/utilities/_variables.scss` に定義されています。プロジェクト全体で値を変更する場合は、ここで定義を更新してください。

## SCSSのコンパイル

現在、`css/main.css` はSCSSから手動でコンパイルされています。本番環境では、以下のツールを使用してコンパイルすることをお勧めします：

- Sass CLI
- Node-sass
- Dart Sass
- VS Code拡張機能 (Live Sass Compiler など)

## ブラウザ互換性

このプロジェクトは以下のブラウザをサポートしています：
- Chrome (最新2バージョン)
- Firefox (最新2バージョン)
- Safari (最新2バージョン)
- Edge (最新2バージョン)

## 注意事項

- ❌ Tailwind CSS は使用されていません
- ✅ BEM方法論を厳密に守っています
- ✅ すべてのスタイルはSCSS/CSSで管理されています
- ✅ モバイルファースト設計です
