# Tailwind CSS から BEM + SCSS への移行ガイド

## 変更内容の概要

このドキュメントは、Tailwind CSSからBEM + SCSSへの移行で行った主な変更をまとめています。

## 主な変更点

### 1. ファイル構造の整理

**Before (Tailwind CDN):**
```
New/
├── index.html (Tailwind CDN + 大量のユーティリティクラス)
├── main.css (最小限)
└── assets/
```

**After (BEM + SCSS):**
```
New/
├── index.html (BEM クラス)
├── css/
│   └── main.css (コンパイル済みCSS)
├── scss/
│   ├── main.scss
│   ├── components/
│   │   ├── _header.scss
│   │   ├── _nav.scss
│   │   ├── _hero.scss
│   │   ├── _button.scss
│   │   └── _section.scss
│   └── utilities/
│       ├── _variables.scss
│       ├── _mixins.scss
│       └── _reset.scss
└── assets/
```

### 2. HTMLの変更

#### Header セクション

**Before (Tailwind):**
```html
<header class="w-full">
  <div class="w-full bg-[#FBF0DF] px-[34px] py-[8px]">
    <div class="mx-auto flex items-center justify-between">
      <div class="text-[32px] text-[#6f3f2b] leading-tight">...</div>
      <div class="flex flex-col">
        <a class="inline-block bg-[#327C49] text-white text-[22px] ... border-[5px] border-white">...</a>
      </div>
    </div>
  </div>
</header>
```

**After (BEM):**
```html
<header class="header">
  <div class="header__content">
    <div class="header__brand">...</div>
    <div class="header__actions">
      <a href="#" class="button button--primary">...</a>
    </div>
  </div>
</header>
```

#### Navigation セクション

**Before (Tailwind):**
```html
<nav class="w-full bg-[#8b5e44]">
  <ul class="flex">
    <li class="w-1/5 text-center py-4 text-white border-r border-white">...</li>
  </ul>
</nav>
```

**After (BEM):**
```html
<nav class="nav">
  <ul class="nav__list">
    <li class="nav__item">...</li>
  </ul>
</nav>
```

#### Hero セクション

**Before (Tailwind):**
```html
<section class="relative bg-cover bg-center" style="height:480px;">
  <div class="absolute inset-0 bg-black bg-opacity-25"></div>
  <div class="relative z-10 container mx-auto px-4 h-full flex flex-col md:flex-row justify-center items-center text-white">
    <div class="max-w-xl text-center md:text-left">
      <h2 class="text-3xl md:text-4xl font-bold">...</h2>
    </div>
    <div class="mt-8 md:mt-0 md:ml-12 bg-[var(--primary-green)] p-6 rounded-md max-w-xs text-center">
      ...
    </div>
  </div>
</section>
```

**After (BEM):**
```html
<section class="hero" style="background-image: url('assets/hero-bg.webp');">
  <div class="hero__overlay"></div>
  <div class="hero__content">
    <div class="hero__text">
      <h2 class="hero__title">...</h2>
      <p class="hero__subtitle">...</p>
    </div>
    <div class="hero__box">
      <p class="hero__box-text">...</p>
    </div>
  </div>
</section>
```

#### セクション/メインコンテンツ

**Before (Tailwind):**
```html
<main class="flex-grow">
  <div class="container mx-auto px-4 py-12 space-y-16">
    <section>
      <h3 class="text-xl font-semibold text-[var(--primary-blue)]">...</h3>
      <ul class="list-disc list-inside mt-4 text-sm space-y-1">
        <li>...</li>
      </ul>
    </section>
    <section class="bg-white p-6 rounded shadow">
      <h4 class="text-lg font-bold">...</h4>
      <p class="mt-2 text-sm">...</p>
    </section>
  </div>
</main>
```

**After (BEM):**
```html
<main class="main">
  <div class="main__sections">
    <section class="section">
      <h3 class="section__title section__title--primary-blue">...</h3>
      <ul class="section__list">
        <li class="section__item">...</li>
      </ul>
    </section>
    
    <section class="section">
      <div class="section__content">
        <h4 class="section__subtitle">...</h4>
        <p class="section__text">...</p>
      </div>
    </section>
  </div>
</main>
```

### 3. CSSの構造化

#### 変数の一元管理

すべてのカラー、フォント、サイズは `_variables.scss` で定義：

```scss
// Colors
$primary-green: #327C49;
$primary-blue: #327C7D;
$accent: #EA916A;

// Typography
$font-size-sm: 14px;
$font-size-base: 16px;
$font-size-lg: 18px;

// Spacing
$spacing-xs: 4px;
$spacing-sm: 8px;
$spacing-md: 12px;
```

#### Mixinの活用

よく使われるパターンをMixinで再利用可能に：

```scss
@mixin respond-to($breakpoint) {
  @if $breakpoint == 'md' {
    @media (min-width: $breakpoint-md) {
      @content;
    }
  }
}

@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}
```

#### コンポーネント別ファイル

各コンポーネントは専用のSCSSファイルで管理：

```scss
// _header.scss
.header {
  background-color: $light-beige;
  padding: $spacing-sm $spacing-3xl;

  &__content {
    @include flex-between;
    max-width: 1200px;
  }

  &__brand {
    font-size: $font-size-3xl;
    
    &-title {
      font-size: $font-size-2xl;
    }
  }
}
```

## BEM 命名ルール

### Block（ブロック）
独立したコンポーネント：`.header`, `.nav`, `.hero`, `.button`, `.section`

### Element（要素）
ブロック内の子要素：`.header__brand`, `.nav__item`, `.section__title`

### Modifier（修飾子）
バリエーション：`.button--primary`, `.nav__item--active`, `.section__title--primary-blue`

## パフォーマンスの違い

### Tailwind CDN
- ファイルサイズ：CDN経由で数MB（未最適化）
- ロード時間：CDN遅延の可能性
- キャッシング：難しい

### BEM + SCSS
- ファイルサイズ：コンパイル済みCSS（数KB）
- ロード時間：高速
- キャッシング：最適化しやすい
- 保守性：クラス名が明確で理解しやすい

## 開発での利点

✅ **保守性向上**
- クラス名が自己説明的
- コンポーネント構造が明確

✅ **拡張性向上**
- 新しいコンポーネントを追加しやすい
- 既存コンポーネントに影響しない

✅ **チーム開発対応**
- 命名規則が統一されている
- コードレビューが容易

✅ **パフォーマンス**
- 最小化されたCSS
- 不要なCSSがない

✅ **デザイン一貫性**
- 変数で色・サイズを統一管理
- 変更が一元管理される

## 今後の改善案

1. **SCSSのコンパイル自動化**
   - Webpack, Gulp, Parcel などのビルドツール導入

2. **CSS最小化**
   - CSSMinifier でファイルサイズ削減

3. **ポストプロセッシング**
   - PostCSS + Autoprefixer でベンダープレフィックス自動付与

4. **テスト**
   - CSSlinting (StyleLint)
   - ビジュアルテスト

## 参考資料

- [BEM CSS 公式](http://getbem.com/)
- [SCSS ドキュメント](https://sass-lang.com/documentation)
- [CSS Methodologies](https://www.smashingmagazine.com/2018/05/guide-css-layout/)
