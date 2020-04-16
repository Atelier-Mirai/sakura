# css と sass の比較
Sass: Syntactically Awesome Style Sheets の略。
公式ガイド： https://sass-lang.com/guide

Sassには、sass形式とscss形式がある。
従来のcssと記法が同じであるため、scss形式が広く使われている。
sass記法は、```{}```や、```;```を取り除き、よりすっきりとさせている。

## Sassの特徴
* 一行コメント ``` // ``` が使える。
* 変数が使える。
* 入れ子(ネスト)ができる。
* スタイルファイルを分けて管理できる。
* ミックスイン（部品の再利用）ができる。
  - メディアクエリの記述が楽

* 演算ができる。
  - 幅や高さなどの四則演算。
  - 色の演算。
* オリジナル関数の定義

## 一行コメントを使う
``` sass ``` では、一行コメント ``` // ``` が使える。
複数行選択し、``` Command + / ``` で、全てコメントにできる。
css に コンパイルされると、コメントは残らない。
(コンパイルされたcssにコメントを残したいのであれば、``` /* */ ``` を使う。)

## 変数を使う
SCSS
```scss
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

CSS

```css
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
```

``` scss ```では、「変数」を用いることができる。

``` $font-stack ```や、``` $primary-color ```が、変数である。スタイルを変更したくなった際には、変数の値を変更するだけでよいため、管理が楽である。

## 入れ子(ネスト)にできる。

SCSS
```scss
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

CSS
```css
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

htmlの構造そのままに記述していけるので、楽である。

### 入れ子(ネスト) その2
SCSS
```scss
a {
  color: pink;
  &:hover {
    color: red;
  }
  &.active {
    color: blue;
  }
}
```

CSS

```css
a {
  color: pink;
}
a:hover {
  color: red;
}
a.active {
  color: blue;
}
```

``` & ``` で繋いで、纏めて書ける。

## スタイルファイルを分けて管理できる。

一つの大きなスタイルシートを管理するのは大変である。
このため、機能ごとに個々のスタイルシートに分け、管理する。

CSS の 設計思想は各種あるが、FLOCSS(フロックス)が良いと思う。
https://github.com/hiloki/flocss

### 基本原則
FLOCSSは次の3つのレイヤーと、**Object**レイヤーの子レイヤーで構成されます。

1. Foundation - reset/normalize/base...
2. Layout - header/main/sidebar/footer...
3. Object
   1. Component - grid/button/form/media...
   2. Project - articles/ranking/promo...
   3. Utility - clearfix/display/margin...

次の例は、Sassを採用した場合の例です。

``` text
├── foundation
│   ├── _base.scss
│   └── _reset.scss
├── layout
│   ├── _footer.scss
│   ├── _header.scss
│   ├── _main.scss
│   └── _sidebar.scss
└── object
    ├── component
    │   ├── _button.scss
    │   ├── _dialog.scss
    │   ├── _grid.scss
    │   └── _media.scss
    ├── project
    │   ├── _articles.scss
    │   ├── _comments.scss
    │   ├── _gallery.scss
    │   └── _profile.scss
    └── utility
        ├── _align.scss
        ├── _clearfix.scss
        ├── _margin.scss
        ├── _position.scss
        ├── _size.scss
        └── _text.scss
```

モジュール単位でファイルを分割することによって、ページ単位またはプロジェクト単位でのモジュールの追加・削除の管理が容易になります。

これらを統括するための`app.scss`のようなファイルからは次のように参照します。

```scss
// ==========================================================================
// Foundation
// ==========================================================================

@import "foundation/_reset";
@import "foundation/_base";

// ==========================================================================
// Layout
// ==========================================================================

@import "layout/_footer";
@import "layout/_header";
@import "layout/_main";
@import "layout/_sidebar";

// ==========================================================================
// Object
// ==========================================================================

// -----------------------------------------------------------------
// Component
// -----------------------------------------------------------------

@import "object/component/_button";
@import "object/component/_dialog";
@import "object/component/_grid";
@import "object/component/_media";

// -----------------------------------------------------------------
// Project
// -----------------------------------------------------------------

@import "object/project/_articles";
@import "object/project/_comments";
@import "object/project/_gallery";
@import "object/project/_profile";

// -----------------------------------------------------------------
// Utility
// -----------------------------------------------------------------

@import "object/utility/_align";
@import "object/utility/_clearfix";
@import "object/utility/_margin";
@import "object/utility/_position";
@import "object/utility/_size";
@import "object/utility/_text";
```
