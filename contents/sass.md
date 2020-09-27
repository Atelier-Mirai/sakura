# css と sass の比較
Sass: Syntactically Awesome Style Sheets の略です。
公式ガイド： https://sass-lang.com/guide

Sassには、sass形式とscss形式があります。
従来のcssと記法が同じであるため、scss形式が広く使われています。
(sass記法は、```{}```や、```;```を取り除き、よりすっきりとさせています。)

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

``` scss ```では、「変数」を用いることができます。

``` $font-stack ```や、``` $primary-color ```が、変数です。スタイルを変更したくなった際には、変数の値を変更するだけでよいため、管理が楽です。

## 入れ子(ネスト)にできます。

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

htmlの構造そのままに記述していけるので、楽です。

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

``` & ``` で繋いで、纏めて書けます。

## スタイルファイルを分けて管理できます。

一つの大きなスタイルシートを管理するのは大変なので、
このため、機能ごとに個々のスタイルシートに分け、管理できます。

CSS設計思想は各種あるが、簡素で分かり易く使えるFLOU(フロウ)がお薦めです。

### 基本原則
FLOUは次の4つの層で構成されます。

```
1. Foundation - reset / normalize / base...
2. Layout     - justify-left / center / right...
3. Object     - header/main/sidebar/footer...
4. Utility    - margin / padding ...
```

次は Sassを採用した場合の例です。

``` text
├── foundation
│   └── _reset.scss
├── layout
│   ├── _footer.scss
│   ├── _header.scss
│   ├── _main.scss
│   └── _sidebar.scss
└── utility
     ├── _margin.scss
     ├── _padding.scss
     ├── _size.scss
     └── _text.scss
```

モジュール単位でファイルを分割することによって、ページ単位またはプロジェクト単位でのモジュールの追加・削除の管理が容易になっています。

これらを統括するための`style.css.scss`のようなファイルからは次のように参照します。

```scss
/*=============================================================================
  style.css.scss サイト全体で使うスタイルシート
=============================================================================*/

/*-----------------------------------------------------------------------------
  utility
-----------------------------------------------------------------------------*/
@import "utilities/utility";

/*-----------------------------------------------------------------------------
  foundation
-----------------------------------------------------------------------------*/
@import "foundation/foundation";

/*-----------------------------------------------------------------------------
  layout
-----------------------------------------------------------------------------*/
@import "layouts/layout";

/*-----------------------------------------------------------------------------
  object
-----------------------------------------------------------------------------*/
@import "objects/object";
```

### ミックスイン @mixin

CSS では、一度定義した CSS の再利用は難しいですが、Sass ではミックスインを使うことで CSS の再利用が可能になります。
* [Web Design Leaves](https://www.webdesignleaves.com/pr/css/css_basic_08.html)
より引用。

ミックスインを使うとプロパティやセレクタをまとめてワンセットにしておいて、それらを読み込むことができます。
ミックスインは @mixin ディレクティブ(指示命令)を用いて定義し、@include ディレクティブで定義したミックスインを呼び出します。
ミックスインでは引数(ひきすう)を取ることができるので、より使い回しが柔軟にできます。
ミックスインは、@mixin の後に半角スペースを置き、任意の名前で定義します。

``` scss
@mixin grayBox {  // ミックスインの定義
  margin: 20px 0;
  padding: 10px;
  border: 1px solid #999;
  background-color: #eee;
  color: #333;
}

.foo {
  @include grayBox;  // 定義したミックスインの呼び出し
}
```

``` css
.foo {
  margin: 20px 0;
  padding: 10px;
  border: 1px solid #999;
  background-color: #EEE;
  color: #333;
}
```

#### 引数を使ったミックスイン

ミックスインは引数を取ることができます。引数はミックスイン名の後に括弧を書き、その括弧の中に記述します。引数が複数ある場合は、カンマで区切って記述します。

``` scss
@mixin my-border($color, $width, $radius) {
  border: {
    color: $color;
    width: $width;
    radius: $radius;
  }
}

p {
  @include my-border(blue, 1px, 3px);
}
```

``` css
p {
  border-color: blue;
  border-width: 1px;
  border-radius: 3px;
}
```

#### 引数に初期値を設定

引数の初期値を設定しておくと、初期値と同じ値を使用する場合は、引数を省略することができます。よく使う値があれば、初期値を設定しておくと便利です。

引数の初期値を設定するには、変数  と同じ書式（名前は「$」から始め、「:」で区切って値を指定）で記述します。

呼び出す際は、引数が初期値と同じ場合は、（）や値は省略することができます。初期値と値が異なる場合だけ、引数の値を指定します。

``` scss
@mixin kadomaru($radius: 5px) {
  border-radius: $radius;
}

.foo {
  @include kadomaru;
}

.bar {
  @include kadomaru();
}

.baz {
  @include kadomaru(10px);
}
```

``` css
.foo {
  border-radius: 5px;
}
.bar {
  border-radius: 5px;
}
.baz {
  border-radius: 10px;
}
```

### メディアクエリへの活用
https://www.tam-tam.co.jp/tipsnote/html_css/post10708.html より引用

レスポンシブWebデザインではメディアクエリ（media queries）を書くことが多くなります。通常のCSSではブレイクポイントを変更したくなったときに、すでに書いてしまった箇所を直していくのはとても大変です。
Sass（scss記法）を使えば、変数や@mixinを使うことで1箇所で管理することが容易になります。

``` scss
$breakpoints: (
  'tablet':  '(min-width: 481px)',
  'desktop': '(min-width: 960px)',
) !default;

@mixin mq($breakpoint: tablet) {
  @media #{map-get($breakpoints, $breakpoint)} {
    @content;
  }
}
```

@mixinを呼び出すときは以下のようになります。

``` scss
.foo {
  color: blue;
  @include mq() { // 引数を省略（初期値はtabletの481px）
    color: yellow;
  }
  @include mq(desktop) { // 引数を個別に指定
    color: red;
  }
}
```

このように出力（コンパイル）されます。

``` css
.foo {
  color: blue;
}

@media (min-width: 481px) {
  .foo {
    color: yellow;
  }
}

@media (min-width: 960px) {
  .foo {
    color: red;
  }
}
```
* [Web Design Leaves](https://www.webdesignleaves.com/pr/css/css_basic_08.html)
より引用。

### 数値の操作（四則演算）
Sass では数値型の値に計算の記号を使うことで計算をすることができます。<br>
単位を省略すると元の単位にあわせて計算されます。


``` scss
.thumb {
  width: 200px - (5 * 2) - 2;
  padding: 5px + 3px;
  border: 1px * 2 solid #ccc;
}
```

``` css
.thumb {
  width: 188px;
  padding: 8px;
  border: 2px solid #ccc;
}
```

計算で使える記号には、以下のようなものがあります。

``` text
* 加算： +（プラス)
* 減算： -（ハイフン）
* 乗算： *（アスタリスク）
* 除算： /（スラッシュ）
* 剰余： %（パーセント）
```

実際には、以下のように変数を使って計算することが多いと思います。

``` scss
$main_width: 600px;
$border_width: 2px;

.foo {
  $padding: 5px;
  width: $main_width - $padding * 2 - $border_width *2;
}
```

``` css
.foo {
  width: 586px;
}
```

### 色関連の関数

| 関数名                           | 機能                | 実行例                                                |
|:---------------------------------|:--------------------|:------------------------------------------------------|
| rgba($color, $alpha)             | RGB値に透明度を指定 | rgba(blue, 0.2) => rgba(0, 0, 255, 0.2)               |
| lighten($color, $amount)         | 明るい色を作成      | lighten(#800, 20%) => #e00                            |
| darken($color, $amount)          | 暗い色を作成        | darken(hsl(25, 100%, 80%), 30%) => hsl(25, 100%, 50%) |
| mix($color1, $color2, [$weight]) | 中間色を作成        | mix(#f00, #00f, 25%) => #3f00bf                       |
| saturate($color, $amount)        | 彩度の高い色を作成  | saturate(#855, 20%) => #9e3f3f                        |
| desaturate($color, $amount)      | 彩度の低い色を作成  | desaturate(#855, 20%) => #726b6b                      |

### オリジナル関数の定義

@function を使って、自分で独自の function（関数）を定義することができます。以下が構文です。

``` scss
@function 関数名($引数) {
  // 処理の記述（必要であれば）
  @return 戻り値;
}
```

@function で関数の宣言をします。独自の関数名を指定して、引数を設定し、@return を使って戻り値を返します。引数は必須ではありません。

以下はサイズを変更する独自関数の例です。

``` scss
@function _changeSize($value, $size) {
  @return $value * $size;
}

.foo {
  width: _changeSize(200px, 1/3) ;
}

.bar {
  height: _changeSize(50px, 1.6);
}
```

``` css
.foo {
  width: 66.66667px;
}

.bar {
  height: 80px;
}
```

### コメント

独自に定義した関数の場合、その関数がどのようなものなのかをコメントとして残しておくと便利です。引数の型や単位が必要か等を記述するようにすると良いと思います。

``` scss
// @function _linearGradient　グラデーションの値を返す
// @param    {Color} $baseColor
// @param    {Number} percentage 0-100% default: 20%
// @return   {String} linear-gradient value
@function _linearGradient($baseColor, $amount: 20%) {
  @return "linear-gradient(" + $baseColor + "," + darken($baseColor, $amount) + ")";
}

.foo {
  background-image :_linearGradient(#cccccc);
}
```

上記の例では、第2引数に初期値を設定しています。

``` css
.foo {
  background-image: "linear-gradient(#cccccc, #999999)";
}
```
