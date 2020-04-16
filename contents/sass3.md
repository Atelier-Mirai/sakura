* [Web Design Leaves](https://www.webdesignleaves.com/pr/css/css_basic_08.html)
より引用。

### 数値の操作（四則演算）
Sass では数値型の値に計算の記号を使うことで計算をすることができます。単位を省略すると元の単位にあわせて計算されます。


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

* 加算： ``` +（プラス）```
* 減算： ``` -（ハイフン）```
* 乗算： ``` *（アスタリスク）```
* 除算： ``` /（スラッシュ）```
* 剰余： ``` %（パーセント）```

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
