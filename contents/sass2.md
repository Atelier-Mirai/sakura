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
  'tablet':  'screen and (min-width: 481px)',
  'desktop': 'screen and (min-width: 960px)',
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

@media screen and (min-width: 481px) {
  .foo {
    color: yellow;
  }
}

@media screen and (min-width: 960px) {
  .foo {
    color: red;
  }
}
```
