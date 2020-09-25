# FLOU

Webの開発をやったことのある方なら誰しも、「CSSって結局どう書くのがベストなの？」という悩みを感じたことがあるでしょう。
一見簡単なCSSですが、一度書き始めるとそのあまりの自由さに、まるで大海原に放り出された赤子のような気分になってしまいますよね。
人生何事も、ある程度制約があったほうがやりやすいものです。
そんなわけで今日はCSSの設計について考えてみましょう。

[FLOCSSを扱いきれないあなたに贈る、スリムなCSS設計の話](https://webnaut.jp/technology/20170407-2421/)をもとに簡略化。

## F、L、O、Uの4つに分けよう

* すっきりした構文
  * 閉じタグの無い短い構文 (代わりにインデントを用いる)
  * 閉じタグを用いた HTML 形式の構文
  * 設定可能なショートカットタグ (デフォルトでは `#` は `<div id="...">` に, `.` は `<div class="...">` に)
* 安全性
  * デフォルトで自動 HTML エスケープ
* 柔軟な設定
* プラグインを用いた拡張性:
  * インクルード
* 高性能
  * ERB に匹敵するスピード

## リンク

* ホームページ: <http://slim-lang.com>

## イントロダクション

### Slim とは?

Slim は 高速, 軽量なテンプレートエンジンです。
Slim の核となる構文は1つの考えによって導かれています: "この動作を行うために最低限必要なものは何か。"

### なぜ Slim を使うのか?

* Slim によって メンテナンスが容易な限りなく最小限のテンプレートを作成でき, 正しい文法の HTML が書けることを保証します。
* Slim の構文は美しく, テンプレートを書くのがより楽しくなります。Slim は主要なフレームワークで互換性があるので, 簡単に始めることができます。
* Slim のアーキテクチャは非常に柔軟なので, 構文の拡張やプラグインを書くことができます。

___そう, Slim は速い!___ Slim は開発当初からパフォーマンスに注意して開発されてきました。
私たちの考えでは, あなたは Slim の機能と構文を使うべきです。Slim はあなたのアプリケーションのパフォーマンスに悪影響を与えないことを保証します。

### どうやって使い始めるの?

Slim を gem としてインストール:

~~~
gem install slim
~~~

あなたの Gemfile に `gem 'slim'` と書いてインクルードするか, ファイルに `require 'slim'` と書く必要があります。これだけです! 後は拡張子に .slim を使うだけで準備完了です。

### 構文例

Slim テンプレートがどのようなものか簡単な例を示します:

~~~ slim
doctype html
html
  head
    title Slim のファイル例
    meta name="keywords" content="template language"
    meta name="author" content=author
    link rel="icon" type="image/png" href=file_path("favicon.png")
    javascript:
      alert('Slim は javascript の埋め込みに対応しています!')

  body
    h1 マークアップ例

    #content
      p このマークアップ例は Slim の典型的なファイルがどのようなものか示します。

    == yield

    - if items.any?
      table#items
        - for item in items
          tr
            td.name = item.name
            td.price = item.price
    - else
      p アイテムが見つかりませんでした。いくつか目録を追加してください。
        ありがとう!

    div id="footer"
      == render 'footer'
      | Copyright &copy; #{@year} #{@author}
~~~

インデントについて, インデントの深さはあなたの好みで選択できます。マークアップを入れ子にするには最低1つのスペースによるインデントが必要なだけです。

## ラインインジケータ

### テキスト `|`

パイプを使うと, Slim はパイプよりも深くインデントされた全ての行をコピーします。

~~~ slim
body
  p
    |
      一行目
      二行目
      三行目
~~~

~~~ html
<body><p>一行目 二行目 三行目</p></body>
~~~

改行を入れたい場合、次のように書けます。

~~~ slim
body
  p
    | 一行目
    br
    | 二行目
    br
    | 三行目
~~~

~~~ html
<body><p>一行目<br>二行目<br>三行目</p></body>
~~~

### インライン html `<` (HTML 形式)

HTML タグを直接 Slim の中に書くことができます。Slim では, 閉じタグを使った HTML タグ形式や HTML と Slim を混ぜてテンプレートの中に書くことができます。

~~~ slim
<html>
  head
    title Example
  <body>
    - if articles.empty?
    - else
      table
        - articles.each do |a|
          <tr><td>#{a.name}</td><td>#{a.description}</td></tr>
  </body>
</html>
~~~

### 制御コード `-`

ダッシュは制御コードを意味します。制御コードの例としてループと条件文があります。`end` は `-` の後ろに置くことができません。ブロックはインデントによってのみ定義されます。
複数行にわたる Ruby のコードが必要な場合, 行末にバックスラッシュ `\` を追加します。

~~~ slim
body
  - if articles.empty?
    | 在庫なし
~~~

### 出力 `=`

イコールはバッファに追加する出力を生成する Ruby コードの呼び出しを Slim に命令します。Ruby のコードが複数行にわたる場合, 例のように行末にバックスラッシュを追加します。

~~~ slim
= javascript_include_tag \
   "jquery",
   "application"
~~~

行末・行頭にスペースを追加するために修飾子の `>` や `<` がサポートされています。

* `=>` は末尾のスペースを伴った出力をします。 末尾のスペースが追加されることを除いて, 単一の等合 (`=`) と同じです。
* `=<` は先頭のスペースを伴った出力をします。先頭のスペースが追加されることを除いて, 単一の等号 (`=`) と同じです。

### HTML エスケープを伴わない出力 `==`

単一のイコール (`=`) と同じですが, `escape_html` メソッドを経由しません。 末尾や先頭のスペースを追加するための修飾子 `>` と `<` はサポートされています。

* `==>` は HTML エスケープを行わずに, 末尾のスペースを伴った出力をします。末尾のスペースが追加されることを除いて, 二重等号 (`==`) と同じです。
* `==<` は HTML エスケープを行わずに, 先頭のスペースを伴った出力をします。先頭のスペースが追加されることを除いて, 二重等号 (`==`) と同じです。

### コードコメント `/`

コードコメントにはスラッシュを使います。スラッシュ以降は最終的なレンダリング結果に表示されません。コードコメントには `/` を, html コメントには `/!` を使います。

~~~ slim
body
  p
    / この行は表示されません。
      この行も表示されません。
    /! html コメントとして表示されます。
~~~

  構文解析結果は以下:

~~~ html
<body><p><!--html コメントとして表示されます。--></p></body>
~~~

### HTML コメント `/!`

html コメントにはスラッシュの直後にエクスクラメーションマークを使います (`<!-- ... -->`)。

## HTML タグ

### <!DOCTYPE> 宣言

doctype キーワードでは, とても簡単な方法で複雑な DOCTYPE を生成できます。

~~~ slim
doctype html
~~~

~~~ html
<!DOCTYPE html>
~~~

### 行頭・行末にスペースを追加する (`<`, `>`)

a タグの後に > を追加することで末尾にスペースを追加するよう Slim に強制することができます。

~~~ slim
a> href='url1' リンク1
a> href='url2' リンク2
~~~

< を追加することで先頭にスペースを追加できます。

~~~ slim
a< href='url1' リンク1
a< href='url2' リンク2
~~~

これらを組み合わせて使うこともできます。

~~~ slim
a<> href='url1' リンク1
~~~

### インラインタグ

タグをよりコンパクトにインラインにしたくなることがあるかもしれません。

~~~ slim
ul
  li.first: a href="/a" A リンク
  li: a href="/b" B リンク
~~~

可読性のために, 属性を囲むことができるのを忘れないでください。

~~~ slim
ul
  li.first: a[href="/a"] A リンク
  li: a[href="/b"] B リンク
~~~

### テキストコンテンツ

タグと同じ行で開始するか、入れ子にするか、どちらかを選択できます。

~~~ slim
body
  h1 id="headline" 私のサイトへようこそ。
~~~

~~~ slim
body
  h1 id="headline"
    | 私のサイトへようこそ。
~~~

### 動的コンテンツ (`=` と `==`)

同じ行で呼び出すか、入れ子にすることができます。
Rubyコードにより、page_headline を定義している場合に使います。

~~~ slim
body
  h1 id="headline" = page_headline
~~~

~~~ slim
body
  h1 id="headline"
    = page_headline
~~~

### 属性

タグの後に直接属性を書きます。通常の属性記述にはダブルクォート `"` か シングルクォート `'` を使わなければなりません (引用符で囲まれた属性)。

~~~ slim
a href="http://slim-lang.com" title='Slim のホームページ' Slim のホームページへ
~~~

引用符で囲まれたテキストを属性として使えます。

#### 属性の囲み

区切り文字が構文を読みやすくするのであれば,
`{...}`, `(...)`, `[...]` で属性を囲むことができます。

~~~ slim
body
  h1(id="logo") = page_logo
  h2[id="tagline" class="small tagline"] = page_tagline
~~~

属性を囲んだ場合, 属性を複数行にわたって書くことができます:

~~~ slim
h2[id="tagline"
   class="small tagline"] = page_tagline
~~~

#### 引用符で囲まれた属性

例:

~~~ slim
a href="http://slim-lang.com" title='Slim のホームページ' Slim のホームページへ
~~~

引用符で囲まれたテキストを属性として使えます:

~~~ slim
a href="http://#{url}" #{url} へ
~~~

#### Ruby コードを用いた属性

`=` の後に直接 Ruby コードを書きます。コードにスペースが含まれる場合,
`(...)` の括弧でコードを囲まなければなりません。ハッシュを `{...}` に, 配列を `[...]` に書くこともできます。

~~~ slim
body
  table
    - for user in users
      td id="user_#{user.id}" class=user.role
        a href=user_action(user, :edit) Edit #{user.name}
        a href=(path_to_user user) = user.name
~~~

#### 真偽値属性

属性値の `true`, `false` や `nil` は真偽値として
評価されます。属性を括弧で囲む場合, 属性値の指定を省略することができます。

~~~ slim
input type="text" disabled="disabled"
input type="text" disabled=true
input(type="text" disabled)

input type="text"
input type="text" disabled=false
input type="text" disabled=nil
~~~

### ショートカット

#### ID ショートカット `#` と class ショートカット `.`

`id` と `class` の属性を次のショートカットで指定できます。

~~~ slim
body
  h1#headline
    = page_headline
  h2#tagline.small.tagline
    = page_tagline
  .content
    = show_content
~~~

これは次に同じです

~~~ slim
body
  h1 id="headline"
    = page_headline
  h2 id="tagline" class="small tagline"
    = page_tagline
  div class="content"
    = show_content
~~~

#### タグショートカット

`:shortcut` オプションを設定することで独自のタグショートカットを定義できます。Rails アプリケーションでは, `config/initializers/slim.rb` のようなイニシャライザに定義します。Middleman アプリでは, ```config.rb``` の中に以下を記述します。

~~~ ruby
Slim::Engine.set_options shortcut: {'c' => {tag: 'container'}, '#' => {attr: 'id'}, '.' => {attr: 'class'} }
~~~

Slim コードの中でこの様に使用できます。

~~~ slim
c.content テキスト
~~~

レンダリング結果

~~~ html
<container class="content">テキスト</container>
~~~

#### 属性のショートカット

カスタムショートカットを定義することができます (id の`#` , class の `.` のように)。

例として, type 属性付きの input 要素のショートカット `&` を追加します。

~~~ ruby
Slim::Engine.set_options shortcut: {'&' => {tag: 'input', attr: 'type'}, '#' => {attr: 'id'}, '.' => {attr: 'class'}}
~~~

Slim コードの中でこの様に使用できます。

~~~ slim
&text name="user"
&password name="pw"
&submit
~~~

レンダリング結果

~~~ html
<input type="text" name="user" />
<input type="password" name="pw" />
<input type="submit" />
~~~

別の例として, role 属性のショートカット `@` を追加します。

~~~ ruby
Slim::Engine.set_options shortcut: {'@' => 'role', '#' => 'id', '.' => 'class'}
~~~

Slim コードの中でこの様に使用できます。

~~~ slim
.person@admin = person.name
~~~

レンダリング結果

~~~ html
<div class="person" role="admin">Daniel</div>
~~~

## テキストの展開

Ruby の標準的な展開方法を使用します。テキストはデフォルトで html エスケープされます。

~~~ slim
body
  h1 ようこそ #{current_user.name} ショーへ。
~~~

## 埋め込みエンジン

slim中に、cssなども記述できます。

例:

~~~ slim
    css:
      body: {
        background: yellow;
      }

    scss class="myClass":
      $color: #f00;
      body { color: $color; }

    markdown:
      #Header
        #{"Markdown"} からこんにちは!
        2行目!
~~~

レンダリング結果：

~~~ html
<style type="text/css">body{background: yellow}</style>
<style class="myClass" type="text/css">body{color:red}</style>
<h1 id="header">Header</h1>
<p>Markdown からこんにちは! 2行目!</p>
~~~

対応エンジン:

| フィルタ    | 必要な gems                  | 種類                | 説明                                                        |
|:------------|:-----------------------------|:--------------------|:------------------------------------------------------------|
| ruby:       | なし                         | ショートカット      | Ruby コードを埋め込むショートカット                         |
| javascript: | なし                         | ショートカット      | javascript コードを埋め込み、script タグで囲む              |
| css:        | なし                         | ショートカット      | css コードを埋め込み、style タグで囲む                      |
| scss:       | sass                         | コンパイル時        | scss コードを埋め込み、style タグで囲む                     |
| markdown:   | redcarpet/rdiscount/kramdown | コンパイル時 + 展開 | Markdown をコンパイルし、テキスト中の # \{variables} を展開 |

## Slim の設定

### デフォルトオプション

通常、slim で生成される html ファイルは、空白等を取り除き、コンパクトに圧縮されています。
これにより、Webサイトに公開した際の速度改善等を望むことができます。
そして、デバッグ時には、これを無効化することができます。
``` Middleman ```アプリケーションでは、``` config.rb ```中に、以下のように記述します。

~~~ ruby
# デバック用に html をきれいにインデントし属性をソートしない
Slim::Engine.set_options pretty: true, sort_attrs: false
~~~

### 構文ハイライト

様々なテキストエディタのためのプラグインがあります。:

* [Atom用language-slimパッケージ](https://github.com/slim-template/language-slim)

### テンプレート変換

#### htmlファイルからslimファイルへ変換

```
$ html2slim index.html index.html.slim
```

#### slimファイルからhtmlファイルへ変換

```
$ slimrb -p index.html.slim > index.html
```
