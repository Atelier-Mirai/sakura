# Middleman

静的サイトを構築する使いやすいフレームワーク
Middleman はモダンな web 開発のショートカットやツールを 採用した静的サイトジェネレータです。

https://middlemanapp.com/jp/

## インストール
```
$ gem install middleman
```

インストール後、新しいコマンドと、3つの便利な機能が追加される。
```
$ middleman init     # 新たに Middleman を使ったサイトを作成する。
$ middleman server   # コーディング中に、ブラウザで結果を確認する。
$ middleman build    # 公開用のサイトデータを出力する。
```

## 新しいサイトの作成
開発を始めるに Middleman が動作するプロジェクトフォルダを作る必要があります。
* 既存フォルダを、Middlemanで開発できるようにする場合
  ```
  $ middleman init
  ```
* 新規に、Middlemanで開発するフォルダを作成する場合
  ```
  $ middleman init my_new_site
  ```

これにより、いくつかのディレクトリとファイルが自動生成される。

source ディレクトリ: webサイト構築用に、slim, sass 等のソースファイルを置くディレクトリ
config.rb: middleman の設定を行うためのファイル
Gemfile: Middlemanが必要とする、gem(=便利なプログラム)を記述したファイル

## ディレクトリ構造

```
my_middleman_site/
  +-- .gitignore               # git の対象にしたくないファイルを記述する
  +-- Gemfile                  # Middlemanが必要とするgemを記述する
  +-- Gemfile.lock             
  +-- config.rb                # Middleman の各種設定用ファイル
  +-- build/                   # 静的サイトのファイルがコンパイルされ出力されるディレクトリ
  +-- data/                    # データファイルを置くと、テンプレート内(=slim)で利用できる
  +-- helper/                  # よくあるHTMLの作業を簡単にためのプログラムを自作できる
  +-- source/                  # web サイトのソースファイルを置くディレクトリ
      +-- images/              # 画像ファイル
      +-- index.html.slim      # slimで記述。middlemanがコンパイルし、index.htmlになる
      +-- javascripts/         # サイトで必要なjavascript用のディレクトリ
      ¦   +-- site.js          
      +-- layouts/             # レイアウトファイル(後述)を置くディレクトリ
      ¦   +-- layout.slim
      +-- stylesheets          # スタイルシートを置くディレクトリ
          +-- site.css.scss
```

それぞれのファイルは、次のように記述します。

** Gemfile **
```
source 'https://rubygems.org'

# 静的サイトジェネレータ Middleman
gem 'middleman'
# ベンダープリフィックス 自動付与する
gem 'middleman-autoprefixer'
# ファイル更新の際、ブラウザを再読み込みする
gem 'middleman-livereload'
# テンプレートエンジンはSlimを使用する
gem 'slim'
# イメージ圧縮を行う
gem "middleman-imageoptim"
# HTML圧縮を行う
gem "middleman-minify-html"
```

** config.rb **
```
# 自動再読み込み
activate :livereload

# ベンダープリフィックス付与
activate :autoprefixer do |prefix|
  prefix.browsers = "last 2 versions"
end

# レイアウト
set :layout, 'site'
page 'index.html', layout: 'top'
page 'no_layout.html', layout: false

# ビルド時の設定
configure :build do
  # HTML 圧縮
  activate :minify_html
  # CSS 圧縮
  activate :minify_css
  # # JavaScript 圧縮
  activate :minify_javascript
  # # イメージ 圧縮
  activate :imageoptim
  # アセットファイルの URL にハッシュを追加
  # activate :asset_hash
end

# Slim の設定
set :slim, {
  # デバック用に html をきれいにインデントし属性をソートしない
  # pretty: true, sort_attrs: false,

  # 属性のショートカット
  # Slim コード中、「&text name="user"」と書くと、
  # <input type="text" name="user"> とレンダリングされる。
  shortcut: {'&' => {tag: 'input', attr: 'type'}, '#' => {attr: 'id'}, '.' => {attr: 'class'}}
}
```

** source/layouts/layout_sample.slim **
```
doctype html
html
  head
    meta charset="utf-8"
    meta name="viewport" content="width=device-width, initial-scale=1"
    title
      = current_page.data.title || "株式会社さくら商会"

    / ファビコンの指定
    = favicon_tag 'favicon.ico'
    link rel="apple-touch-icon" sizes="180x180" href="/images/apple-touch-icon-180x180.png"

    / 検索エンジン用にサイトの紹介文章
    meta name="description" content="お花見ならさくら商会。屋台の出店からライトアップ、場所とリまでお任せください。"

    / fontawesome
    link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.13.0/css/all.css"

    / jQuery
    script src="https://code.jquery.com/jquery-3.5.0.min.js"

    / IE Buster
    script src="https://cdn.jsdelivr.net/npm/ie-buster@1.1.0/dist/ie-buster.min.js"

    / stylesheet の読み込み (style.cssを読み込む)
    = stylesheet_link_tag "style"

    / JavaScript (site.jsを読み込む)
    = javascript_include_tag "site"

  body.site
    / 部分ファイル _header.slim を読み込む
    = partial 'header'
    / この = yield が、index.html.slim等、各々の内容に置き換えられる
    = yield
    / 部分ファイル _footer.slim を読み込む
    = partial 'footer'
```

## 開発サイクル

### middleman server
コマンドラインから, プロジェクトフォルダの中でプレビューサーバを 起動してください。
```
$ cd my_project
$ middleman server
```

このコマンドはローカルの Web サーバを起動します: http://localhost:4567/
source フォルダでファイルを作成編集し, プレビュー Web サーバ上で 反映された変更を確認することができます。
コマンドラインから Ctrl + C を使って プレビューサーバを停止できます。

### LiveReload
Middleman にはサイト内のファイルを編集するたびにブラウザを自動的にリロードする 拡張がついています。
まず Gemfile に middleman-livereload を追記してください。

** Gemfile **
```
gem 'middleman-livereload'
```

続いて config.rb を開いて次の行を 追加してください。

** config.rb **
```
activate :livereload
```

これであなたのブラウザはページ内容に変更があると自動的にリロードされます。

## ビルド & デプロイ

### ``` middleman build ``` でサイトをビルド
静的サイトのコードを出力する準備ができたら、サイトをビルドする必要があります。
コマンドラインを使い, プロジェクトフォルダの中から middleman build を実行してください。

```
$ cd my_project
$ middleman build
```

サイトをビルドすることで, 必要なものはすべて build ディレクトリに 用意されます。
適宜、公開してください。
(ソースコード管理にGitを、Webサイトの公開にNetlifyの使用がおすすめです。)

### アセットハッシュの付与
プロダクション環境では一般的にアセットファイル名にハッシュ文字列を付与します。

** config.rb **
``` ruby
configure :build do
  # アセットファイルの URL にハッシュを追加
  activate :asset_hash
```

ハッシュ文字列を付与しないときには、
```
<link href="/stylesheets/style.css" rel="stylesheet" />
```
と出力されます。

ハッシュ文字列を付与すると、
```
<link href="/stylesheets/style-795ee195.css" rel="stylesheet" />
```
と出力されます。
styleのあとに付与される文字列は、スタイルシートが更新される都度、異なる文字列となるため、(ブラウザのキャッシュ機能による)「新しいスタイルシートをWebサイトにアップしても、見た目が変わらない」という課題を解消することができます。

## Frontmatter
Frontmatterは、本の前付けの意味です。
Frontmatter は YAMLフォーマット(形式)でテンプレート上部に記述することができる、ページ固有の変数です。

** source/contact.html.slim **
```
---
title: "お問い合わせ"
my_list:
  - one
  - two
  - three
---

h1 リスト
ol
  - current_page.data.my_list.each do |f|
    li = f
```

** source/layouts/layout.slim **
``` slim
doctype html
html
  head
    meta charset="utf-8"
    title
      = current_page.data.title || "会社名"
    = stylesheet_link_tag "site"
    = javascript_include_tag "site"
  body
    = yield
```

** でき上がったcontact.html **
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>お問い合わせ</title>
  </head>
  <body>
    <h1>リスト</h1>
    <ol>
      <li>one</li>
      <li>two</li>
      <li>three</li>
    </ol>
  </body>
</html>
```

## テンプレート言語

Middleman は HTML の開発を簡単にするためにたくさんのテンプレート言語へのアクセスを提供します。テンプレート言語はページ内で変数やループを使えるようにするシンプルなものから、ページをHTMLに変換するまったく異なったフォーマットを提供するものにまで及びます。 Middleman は Slim, Sass のサポートを搭載しています。

Middleman で使うテンプレートはそのファイル名にテンプレート言語の拡張子を 含みます。slim で書かれたシンプルな index ページはファイル名の index.html と、slim の拡張子を含む index.html.slim という名前になります。
sourceディレクトリの直下に、index.html.slimを置き、ビルドすると、
buildディレクトリに、index.htmlが出力されます。

** index.html.slim の例 **
```
h1 ようこそ
ul
  - 5.times do |number|
    li
      | カウント:
      = number
```

** ビルド後のindex.html **
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>お問い合わせ</title>
  </head>
  <body>
    <h1>ようこそ</h1>
    <ul>
      <li> カウント: 0 </li>
      <li> カウント: 1 </li>
      <li> カウント: 2 </li>
      <li> カウント: 3 </li>
      <li> カウント: 4 </li>
    </ul>
  </body>
</html>
```

## ヘルパーメソッド
テンプレートヘルパはよくある HTML の作業を簡単にするため、テンプレートの中で使用できるメソッドです。
基本的なメソッドのほとんどは Ruby on Rails のビューヘルパを利用したことのある人にはお馴染みのものです。

### リンクヘルパ
基本的な使い方は、link_toメソッドに引数として、「リンク名」と「リンク先URL」を与えます。

```
= link_to 'わたくしのサイト', 'http://mysite.com'
```

link_to はより複雑な内容のリンクを生成できるように、ブロックをとることもできます。

```
= link_to 'http://mysite.com' do
  = image_tag 'mylogo.png', alt: 'ロゴマーク'
  p
    | わたくしの会社へようこそ
```

```
<a href="http://mysite.com">
  <img src="/images/mylogo.png" alt="ロゴマーク">
  <p>
    わたくしの会社へようこそ
  </p>
</a>
```

### イメージヘルパ
source/images/ディレクトリ内の画像を表示させたい場合には、
イメージヘルパを使って、次のように書くこともできます。

```
= image_tag 'mylogo.png', alt: 'ロゴマーク'
```

```
<img src="/images/mylogo.png" alt="ロゴマーク">
```

### カスタム定義ヘルパ
Middleman によって提供されるヘルパに加え、コントローラやビューの中からアクセスできる独自のヘルパを追加することができます。
良く使うhtml部品生成用のヘルパを創っておくと便利です。

* カスタム定義ヘルパの例：ファイル名を与えるとチラシ用のhtmlを出力してくれる

** helpers/custom_helper.rb **
```
def chirashi_tag(title: , filename: )
  "<div class='col-xs-12 col-md-4'>" +
    "<div class='card leaflet'>" +
      "<a href=\"/pdf/#{filename}.pdf\">" +
        "<img src=\"/pdf/thumbnail/#{filename}.jpg\" class='card-img-top w-100' alt=\"\">" +
      "</a>" +
      "<div class='card-footer'>" +
          "#{title}" +
      "</div>" +
    "</div>" +
  "</div>"
end
```

* 実際に、index.html.slimの中で使ってみる

** source/index.html.slim **
```
= chirashi_tag title: "春の大売り出し", filename: "chirashi"
```

* ビルドして出力された html

** build/index.html **
```
<div class="col-xs-12 col-md-4">
  <div class="card leaflet">
    <a href="/pdf/chirashi.pdf">
      <img src="/pdf/thumbnail/chirashi.jpg" class="card-img-top w-100" alt="">
    </a>
    <div class="card-footer">春の大売り出し</div>
  </div>
</div>
```

## レイアウト
レイアウト機能はテンプレート間で共有する, 個別ページを囲むための共通 HTML の 使用を可能にします。 "layout" は "header" や "footer" 両方を含むことで個別ページのコンテンツを 囲みます。

** レイアウトの例 **

** source/layouts/layout.slim **
```
doctype html
html
  head
    title "株式会社さくら商会"
  body
    / この = yield の部分が、各slimで置き換えられる。
    = yield  
```

slim で、各ページに固有の内容を記述します。
** source/index.html.slim **
```
h1 ようこそ、さくら商会へ
```

** source/layouts/about.html.slim **
```
h1 わたくしたちの会社について
```

ビルドすると、buildディレクトリ以下に、index.htmlやabout.htmlが出力されます。

** build/index.html **
``` html
<!DOCTYPE html>
<html>
  <head>
    <title>株式会社さくら商会</title>
  </head>
  <body>
    <h1>ようこそ、さくら商会へ</h1>
  </body>
</html>
```

** build/about.html **
``` html
<!DOCTYPE html>
<html>
  <head>
    <title>株式会社さくら商会</title>
  </head>
  <body>
    <h1>わたくしたちの会社について</h1>
  </body>
</html>
```

全てのページに共通する部分を、レイアウトファイルに纏め、
各ページに固有の内容を、個別ページに記述することで、開発効率が上がります。
レイアウトファイルは、ファイル名が、source/layouts/layout.slim であるのに対し、
個別ページは、ファイル名が、source/index.html.slim や、source/about.html.slimと、拡張子が異なることに注意してください。

### カスタムレイアウト
デフォルトでは, Middleman はそのサイトのすべてのページに同じレイアウト(layout.slim)を適用します。

複数のレイアウトを使い, どのページがどのレイアウトを使うのか指定したい場合があります。例えば, トップページと、その傘下にある下層ページのような場合です。

** source/layouts/top.slim **
```
doctype html
html
  head
    title "株式会社さくら商会"
  body
    h1 トップページです。
    p 我が社のヒーローイメージです。
    = image_tag 'hero.jpg', alt: 'ヒーローイメージ'
    = yield  
```

** source/layouts/site.slim **
```
doctype html
html
  head
    title "株式会社さくら商会"
  body
    = yield  
```

トップページである、``` index.html ``` には、``` top.slim ``` というレイアウトファイルを適用し、
下層ページである、それ以外のページには、``` site.slim ``` という下層ページ用のレイアウトファイルを使うようにするためには、``` config.rb ``` に次のように記述します。

** config.rb **
``` ruby
# レイアウトの指定
page 'index.html', layout: 'top'
set :layout, 'site'
```

### 完全なレイアウト無効化
いくつかの場合では, まったくレイアウトを使いたくない場合があります。
``` config.rb ``` で次のように書くと、レイアウトを無効化できます。

** config.rb **
```
# 全てのページで、レイアウトを適用したくない場合
set :layout, false
```

** config.rb **
```
# 特定のページ(no_layout.html)にはレイアウトを適用したくない場合
page 'no_layout.html', layout: false
```

## パーシャル（部分・断片ファイル）
パーシャルはコンテンツの重複を避けるためにページ全体にわたってそのコンテンツを共有する方法です。 パーシャルはページテンプレートとレイアウトで使うことができます。

パーシャルのファイル名は ``` _ (アンダースコア) ```から始まります。例として source フォルダに置かれる _footer.slim と 名付けられた footer パーシャルを示します。

** source/_footer.slim **
```
footer
  | Copyright 株式会社さくら商会
```

次に, "partial" メソッドを使ってデフォルトのレイアウトにパーシャルを配置します。

** source/layouts/layout.slim **
```
doctype html
html
  head
    title "株式会社さくら商会"
  body
    = yield  
    / この = partial "footer" の部分が、``` _footer.slim ``` で置き換えられる。
    = partial "footer"
```

ビルドすると、buildディレクトリ以下に、各ページ が出力されます。

** build/index.html **
``` html
<!DOCTYPE html>
<html>
  <head>
    <title>株式会社さくら商会</title>
  </head>
  <body>
    <footer>Copyright 株式会社さくら商会</footer>
  </body>
</html>
```

パーシャルを使い始めると, 変数を渡すことで異なった呼び出しを行いたくなるかもしれません。次の方法で対応出来ます。

** source/\_cat_introduction.slim 猫の自己紹介用のパーシャル **
```
h2 = "#{name}"
= image_tag "#{image_filename}"
p = "#{introduction}"
```

** source/index.html.slim 呼び出し元の猫の紹介ページ **
```
h1 猫の紹介
= partial 'cat_introduction',
           locals: { name: 'タマ',
                     image_filename: 'tama.jpg',
                     introduction: 'タマです。白黒の可愛い毛並みの猫です。'}
= partial 'cat_introduction',
           locals: { name: 'ミケ',
                     image_filename: 'mike.jpg',
                     introduction: 'ミケです。茶色の可愛い毛並みの猫です。'}
```

** build/index.html 出力されたhtmlファイル **
``` html
<h1>猫の紹介</h1>

<h2>
  タマ
</h2>
<img src="/images/tama.jpg" alt="Tama">
<p>
  タマです。白黒の可愛い毛並みの猫です。
</p>
<h2>
  ミケ
</h2>
<img src="/images/mike.jpg" alt="Mike">
<p>
  ミケです。茶色の可愛い毛並みの猫です。
</p>
```

## データファイル
ページのコンテンツデータをレンダリングから抜き出すと便利な場合があります。(例：猫の紹介データ)
データファイルは data フォルダの中に YAML(ヤムル)形式のデータとして作ることができ, テンプレートの中でこの情報を使うことができます。 data フォルダは, source フォルダと同じように, プロジェクトのルートに置かれます。

** ディレクトリ構造(再掲) **
``` text
my_middleman_site/
  +-- .gitignore               # git の対象にしたくないファイルを記述する
  +-- Gemfile                  # Middlemanが必要とするgemを記述する
  +-- Gemfile.lock             
  +-- config.rb                # Middleman の各種設定用ファイル
  +-- build/                   # 静的サイトのファイルがコンパイルされ出力されるディレクトリ
  +-- data/                    # データファイルを置くと、テンプレート内(=slim)で利用できる
  +-- helper/                  # よくあるHTMLの作業を簡単にためのプログラムを自作できる
  +-- source/                  # web サイトのソースファイルを置くディレクトリ
      +-- images/              # 画像ファイル
      +-- index.html.slim      # slimで記述。middlemanがコンパイルし、index.htmlになる
      +-- javascripts/         # サイトで必要なjavascript用のディレクトリ
      ¦   +-- site.js          
      +-- layouts/             # レイアウトファイル(後述)を置くディレクトリ
      ¦   +-- layout.slim
      +-- stylesheets          # スタイルシートを置くディレクトリ
          +-- site.css.scss
```

それでは、ページのコンテンツデータ（ここでは、猫の紹介データ）を、YAML形式のデータファイルに抜き出してみましょう。
data ディレクトリの中に、cats.ymlという名前で作成します。

** data/cats.yml **
``` yml
- name: タマ
  image_filename: tama.jpg
  introduction: タマです。白黒の可愛い毛並みの猫です。
- name: ミケ
  image_filename: mike.jpg
  introduction: ミケです。茶色の可愛い毛並みの猫です。
```
``` name: ``` の後には、「半角スペース」を入れて、「タマ」と書きます。
``` image_filename: ``` の前には、「半角スペース２つ」が必要です。

一般的に、ymlファイルは上のように書きますが、下のように書くこともできます。

** data/cats.yml **
``` yml
[ { name: タマ,
    image_filename: tama.jpg,
    introduction: タマです。白黒の可愛い毛並みの猫です。},
  { name: ミケ,
    image_filename: mike.jpg,
    introduction: ミケです。茶色の可愛い毛並みの猫です。}]
```

```
[
  {一匹目の猫のデータ},
  {二匹目の猫のデータ}
]
```
の形式になっていることが読み取れるでしょうか。
( [] は、配列といい、個々のデータ(要素)を纏めたものです。
  {} は、「ハッシュ(連想配列、辞書)」といい、「name: タマ」のように、「鍵: 値」の形になっていることが特徴です。)

用意したデータファイルは、次のように使います。

** source/index.html.slim **
``` slim
- data.cats.each do |cat|
  li = cat.name
  li = cat.image_filename
  li = cat.introduction
```

解説です。
slim では、テンプレート中に、Rubyコードを書くことができます。
Ruby コードを書くときには、``` - (ハイフン) ```で始めます。
``` data.cats ``` は、Middleman で、データファイルを読み込むための書き方です。このように書くことで、Middlemanは、dataディレクトリにあるcats.ymlから情報を読み込み、Ruby コードで扱えるようにします。
``` .each ``` は、Ruby で、配列の各々の要素について処理するよう、指示するコードです。
``` do ``` は、コードブロックと呼ばれ、以下に続くRubyコードを実行するよう指示する構文です。
``` |cat| ``` には、猫一匹分のデータが入っています。
``` cat.name ``` は、取り出した猫一匹のnameです。(タマやミケなど、個々の猫の名前になります)
``` li = ``` ですが、li は、``` <li>タグ ``` です。``` = ``` を書くことで、Ruby でいろいろ処理を行った結果を、liタグの内容として出力できるので、``` <li>タマ</li> ``` のようになります。

纏めると、ビルドして、buildディレクトリに出力されるhtmlは次のようになります。

** build/index.html **
``` html
<li>ミケです。茶色の可愛い毛並みの猫です。</li>
<li>tama.jpg</li>
<li>タマです。白黒の可愛い毛並みの猫です。</li>
<li>ミケ</li>
<li>mike.jpg</li>
<li>ミケです。茶色の可愛い毛並みの猫です。</li>
```

cats.yml という、猫の紹介データを用いて、htmlを作成できました。
たくさんのデータがあるときに、同じようなhtmlを繰り返し書かずに済むので、とっても楽です。

## ファイルサイズ最適化

### CSS と JavaScript の圧縮
Middleman は CSS のミニファイや JavaScript の圧縮処理を行うので, ファイル最適化 について心配することはありません。ほとんどのライブラリはデプロイを行うユーザの ためにミニファイや圧縮されたバージョンを用意していますが, そのファイルは 読めず編集できなかったりします。Middleman はプロジェクトの中にコメント付きの オリジナルファイルを取っておくので, 必要に応じて読んだり編集することができます。 そして, プロジェクトのビルド時には Middleman は最適化処理を行います。

config.rb で, サイトのビルド時に minify_css 機能と minify_javascript 機能を 有効化します。

** config.rb **
``` ruby
configure :build do
  activate :minify_css
  activate :minify_javascript
end
```

### 画像圧縮
ビルド時に画像も圧縮したい場合, ``` middleman-imageoptim ``` を試してみましょう。

** config.rb **
``` ruby
activate :imageoptim
```

### HTML 圧縮
Middleman は HTML 出力を圧縮する公式の拡張機能を提供しています。 Gemfile に middleman-minify-html を追加します:

** Gemfile **
``` ruby
gem "middleman-minify-html"
```

``` bundle install ``` を実行し, ``` config.rb ``` を開いて次を追加します。

** config.rb **
``` ruby
activate :minify_html
```

ソースを確認すると HTML が圧縮されていることがわかります。
