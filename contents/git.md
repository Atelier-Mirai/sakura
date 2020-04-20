# Git(ギット)

## Gitとは?
分散型バージョン管理システム。
ソースコードの管理や、チームでの共同開発ができる。

### 利点
[はじめてのGit forデザイナー＆コーダー](https://www.slideshare.net/saekoyamamoto/gitfor)
  - Gitってなに？ プログラマではないけれど、Git導入するメリットは？ いわゆるデザイナーやコーダー向けの、「Gitとは？」「Gitの構造とは？」…のやさしい説明スライドです。

Gitを導入する利点がとてもよく分かる。5分くらいで気軽に読めるので、必読!!

### 概要
* Git を用いたバージョン管理のすすめ
https://www.jstage.jst.go.jp/article/isciesci/61/10/61_394/_pdf
  - 研究者向けに書かれたエッセイ。Gitのエッセンスを6ページに凝縮しており、概要を掴むのに最適。「3. Git によるバージョン管理」、「4. 複数人での Git」を読むと全貌が掴める。

### 基本的な流れ

1. ファイルやディレクトリの状態を記録するためのデータベースをつくる。これをリポジトリ（貯蔵所、倉庫、宝庫)といい、今までに開発してきた全ての歴史が保存されていく場所。
2. ワーキングディレクトリ
  - ローカルコンピュータ(手元のコンピュータ)で、開発中のディレクトリのこと。
3. ステージングエリア
  - どのファイルをリポジトリへ保存するのかを管理する領域。
  - (準備ができたものを、リポジトリに登録(公開)するイメージ)
4. ローカルリポジトリ
  - 手元のコンピュータのリポジトリ。今までの自分の開発した履歴が保存されている。
5. リモートリポジトリ
  - クラウド上にある共同開発用のリポジトリ。

1.は一番最初に一度行う。  
2.で今まで通り開発を行い、区切りのいいところまで開発したら、  
3.のステージングエリア(準備領域)にファイルを登録して、  
4.のローカルリポジトリにコミット(保存)する。  
そして、2.に戻って、開発を続ける。  
5.のリモートリポジトリに、自分の開発履歴(ローカルリポジトリ)を公開する操作をpush(押し上げる)という。皆で共用したいものができたときにすると良い。(一人で開発しているときには、自分用のバックアップ(控え)になる。)
逆に、リモートリポジトリを、ローカルリポジトリに落とす操作をfetch(取ってくる)という。

### 操作環境

CUI: ターミナルからコマンドを入力して行う方法。基本。
GUI: グラフィカルに図示される。いろいろなソフトがあるが、SourceTree がおすすめ。

## 参考

### 書籍
* わかばちゃんと学ぶGit
  - 漫画や可愛いイラストが豊富で、分かりやすい。

* よくわかる入門Git
  - 本書は、Gitの基本的な使い方から、チーム開発で使うための機能「ブランチ」、そして高度なGitコマンドまでを解説した入門書です。さらにGitのブランチモデルである「Git flow」と「GitHub-flow」の二つも紹介。チーム開発の基本スキルが身につきます!

### Web
* [はじめてのGit forデザイナー＆コーダー](https://www.slideshare.net/saekoyamamoto/gitfor)
  - Gitってなに？ プログラマではないけれど、Git導入するメリットは？ いわゆるデザイナーやコーダー向けの、「Gitとは？」「Gitの構造とは？」…のやさしい説明スライドです。Gitを導入する利点がとてもよく分かる。5分くらいで気軽に読めるので、必読!!

* [Git を用いたバージョン管理のすすめ](https://www.jstage.jst.go.jp/article/isciesci/61/10/61_394/_pdf)
  - 研究者向けに書かれたエッセイ。Gitのエッセンスを6ページに凝縮しており、概要を掴むのに最適。「3. Git によるバージョン管理」、「4. 複数人での Git」を読むと全貌が掴める。

* [git - 簡単ガイド 猫でもわかるGit 最初の一歩](http://rogerdudler.github.io/git-guide/index.ja.html)
  - 一ページに纏めたGitの導入ページ。簡潔に纏まっています。

* [GIT チートシート](https://github.github.com/training-kit/downloads/ja/github-git-cheat-sheet.pdf)
  - Git を使えるようになったら見ておきたい。良く使うコマンドを2ページに纏めてある。

* [計算機科学実験及演習3 ハードウェア「Gitの使い方」](http://www.lab3.kuis.kyoto-u.ac.jp/~takase/le3a/2020HW3-git.pdf)
  - 京都大学情報学科計算機科学コース で学ぶ学生向けのGitの100枚(A4版なら25枚)のスライド。Git / GitHubについて、概要や使用例を紹介している。

* [Pro Git](https://git-scm.com/book/ja/v2)
  - 500ページにわたりGitを詳細に解説。
  - 二章だけでも読むとよい。80ページで、Gitの主な使い方が学べるチュートリアル形式になっており、手を動かしながら、基本的な使い方を習得できる。


## 環境設定

### macOS用パッケージマネージャ Homebrew をインストールする。
```
$ /bin/bash -c "$(curl -fsSL
  https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

### 高機能ターミナルソフト iTerm2 をインストールする。
```
$ brew cask install iterm2
```

### Git をインストールする。
```
$ brew install git
```

## Gitの初期設定

### バージョン確認
``` bash
$ git version
```

### コミット操作に付加する名前を設定する
```bash
git config --global user.name  "Yamada Taro"
```

### コミット操作に付加するメールアドレスを設定する
``` bash
git config --global user.email "taro@example.com"
```

### コマンドラインの出力を色をつけ見やすくする。
``` bash
git config --global color.ui auto
```

### git commit で Atom を使うようにする。
```
$ git config --global core.editor "atom --wait"
```

### git l でログを簡潔表示できるようにする。
``` bash
$ git config --global alias.l "log --date=short --pretty=format:'%C(yellow)%h %C(reset)%cd %C(red)%d %C(reset)%s'"
```

``` alias(別名) ``` と呼ばれる git の機能です。  

``` bash
$ git config --global -e
```
で、設定内容をエディタで編集できる。
いろいろな方が便利な設定を公開しているので、gitに慣れてきたら自分好みに使いやすくカスタマイズするのも良い。


### バージョン管理システムからの追跡を除外する。
``` .gitignore ``` ファイルに除外したいファイルやディレクトリのパターンを記述する。

** .gitignore の例 **

``` text
.DS_Store
build/
*.log
```

## 基本操作

### リポジトリの作成
ローカルリポジトリを新規作成する。
``` bash
$ git init [project-name]
```

既存のリモートリポジトリをダウンロードする。
``` bash
$ git clone [url]
```

### ファイルの変更管理
```mermaid
graph LR
  作業ディレクトリ-->|add|ステージ
  ステージ-->|commit|リポジトリ
```
<figcaption class="left" style="margin-top: 0;"> 図: Gitの三つの領域 </figcaption>

#### 新規または変更のあるファイルを表示する
``` bash
$ git status
```

#### ファイルの変更内容を確認する
``` bash
$ git diff
```

#### ファイルをステージに追加する。(Git の管理対象にする。)
``` bash
$ git add <ファイル名>
```

#### 全てのファイルをステージに追加する。(Git の管理対象にする。)
``` bash
$ git add .
```

#### Git の管理対象から、除外する。
誤ってステージングした際には、次のコマンドで取り消せる。
``` bash
$ git restore --staged <ファイル名>
```

#### 変更をコミットする
エディタで、変更内容を編集して、コミットする。
``` bash
$ git commit
```

コマンドラインで、変更内容を入力して、コミットする。
``` bash
$ git commit -m "メッセージ"
```

#### ステージングとコミットを一緒に行う
ステージングして、コミットしてと二回コマンドを入力するのは大変である。
次のコマンドにより、纏めて行うことができる。
``` bash
$ git commit -am "メッセージ"
```

#### コミットメッセージを修正する
コミットした後に、タイプミスなどに気付き、コミットメッセージを修正したいときは、次のコマンドを実行する。
``` bash
$ git commit --amend
```
エディタが立ち上がるので、メッセージを適宜編集し、``` Command + S```, ``` Command + W``` で、修正が完了する。

### 変更履歴を確認する
コミットした履歴を参照するには、git log コマンドを用いる。

#### 標準的な形式で、履歴を確認する。
``` bash
$ git log
```

#### 一行で簡潔に表示する。
``` bash
git log --date=short --pretty=format:'%C(yellow)%h %C(reset)%cd %C(red)%d %C(reset)%s'
```
毎回、入力すると大変なので、次のように ``` alias(別名) ``` を 定義すると良い。
``` bash
$ git config --global alias.l
"log --date=short --pretty=format:'%C(yellow)%h %C(reset)%cd %C(red)%d %C(reset)%s'"
```

次回以降、以下のコマンドで表示できる。
``` bash
$ git l
```

#### ファイルの変更差分を表示する。
``` bash
git log -p <ファイル名>
```

#### 二つのコミット間での相違を確認する
``` bash
git diff <コミットA> <コミットB> <ファイル名>
```

#### 表示するコミット数を制限する
``` bash
git log -n<コミット数>
```

### ファイルの移動、削除の管理

#### 作業ディレクトリからファイルを削除し、削除をステージする。
``` bash
$ git rm <ファイル名>
$ git rm -r <ディレクトリ名>
```
#### バージョン管理からファイルを削除する。ローカルのファイルは保持する。
(パスワードファイルなどを間違えてコミットしてしまった場合に使うと良い。)

``` bash
$ git rm --cached <ファイル名>
```

#### ファイル名を変更し、コミットする。
``` bash
$ git mv <旧ファイル名> <新ファイル名>
```

### ファイルの変更を取り消す。
#### ローカルで編集後、まだコミットしていないファイルの変更を取り消す。
``` bash
$ git restore <ファイル名>
```

#### コミットしたことのあるファイルを、特定のコミットの状態にする。
``` bash
$ git restore --source <コミット> <ファイル名>
```



### リモートリポジトリとの同期

#### リモートリポジトリ(GitHub)を新規登録する
```bash
git remote add origin https:github.com/username/repository.git
```
* originというショートカットでURLのリモートリポジトリを登録する。
* 今後は `origin` という名前でgithubリポジトリにアップしたり取得したりできる。
* gitではメインのリモートリポジトリの事を `origin` と呼んでいる。

#### リモートリポジトリに、ローカルリポジトリを送信する

``` bash
$ git push <リモート名><ブランチ名>
$ git push origin master
```

毎回、``` git push origin master ``` と入力するのは大変である。
``` bash
$ git push -u origin master
```
を行うと、次回以降、
``` bash
$ git push
```
で、リモートリポジトリに、ローカルリポジトリを送信できる。

#### リモートリポジトリから履歴を取得、作業ディレクトリに取り込む(fetch & merge)
``` bash
$ git fetch <リモート名>
$ git fetch origin/master
```
リモートリポジトリのマスターブランチの履歴をダウンロードした。
これを、リモートリポジトリに統合(merge)するには、次のコマンドを実行する。

``` bash
$ git merge origin/master
```
これで、現在のブランチに、リモートリポジトリが統合される。

#### リモートリポジトリから履歴を取得、作業ディレクトリに取り込む(pull)
``` bash
$ git pull
```
リモートリポジトリから履歴を取得、変更を統合する。
(リモートリポジトリの履歴に置き換わる)


**補足**

ブランチの中身を全て確認する（-aはallの略）
現在いるブランチに「＊」がつく
git branch -a

ブランチを切り替えてフェッチした内容を確認する
git checkout remotes/origin/master

元のマスターブランチに切り替える
git checkout master


git pullは下記コマンドと同じ事をしている
git fetch origin master
git merge origin/master

git pullはリモートリポジトリからローカルリポジトリのワークツリーに反映させるのを一度にやってしまう。便利な反面注意点がある。

プルを実行する際の注意点
プルしてきたブランチは現在いるブランチに統合される為、思わぬブランチに統合されファイルがぐちゃぐちゃになってしまう危険性がある。
そのため慣れないうちはフェッチを使った方が安全。

### ブランチ
並行して複数の機能を開発するためにあるのがブランチ

#### ブランチを新規作成する
```bash
$ git branch <ブランチ名>
$ git branch feature
```

#### ブランチの一覧を表示する
```bash
$ git branch
```
-aはallの略でリモート追跡ブランチも表示される
$ git branch -a

#### ブランチを切り替える
```bash
$ git switch <既存ブランチ名>
$ git checkout feature
```

#### ブランチを新規作成して切り替える
```
git switch -c <新ブランチ名>
```

#### ブランチを変更・削除する
```bash
# 自分が作業しているブランチの名前を変更する
git branch -m <ブランチ名>
git branch -m new_branch

# ブランチを削除する
# masterにマージされていない変更が残っている場合はエラーが出る
git branch -d <ブランチ名>
git branch -d feature

# ブランチを強制削除する
# masterにマージされていない変更が残っていても強制削除される
git branch -D <branch名>
```

### マージ
マージとは他の人の変更内容を取り込む作業のこと

#### Auto Merge:基本的なマージ
`masterブランチ` の内容が `developブランチ` を分岐した時より進んでしまっている場合、両方の変更を取り込んだマージコミットが作成される。
マージコミットは２つのperentをもつ。

```bash
# 指定したブランチが作業中のブランチにマージされる
git merge <ブランチ名>
git merge <リモート名/ブランチ名>
git merge origin/master
```

#### コンフリクト
`masterブランチ`と`developブランチ`で
同じファイルの同じ行が編集されていた場合、
マージした際に `コンフリクト` が起きる。

###### コンフリクトが起きないようにする為には
・複数人で同じファイルを変更しない
・pullやmergeをする際に変更中の状態をなくしておく（commitやstashをしておく）

## プルリクエスト
自分の変更したコードをリポジトリに取り込んでもらえるよう依頼する機能

```bash
# プルリクエストの順序

1. ブランチを切る
2. ファイルを編集する
3. ローカルにadd,commitする
4. githubにプッシュする
5. githubで「Pull request」タブの「New pull request」ボタンを選択。
6.「base」と「compare」を選択する。
7.「Create pull request」を選択。
8.タイトルとコメントを入力。
9.「Create pull request」を押す。
10.右側の「reviewers」からレビューしてもらいたい人を選択し通知を送る。

# レビュワーの作業順序

1. githubで「Pull request」タブのレビューするコードを選択。
2. 「File changed」からコードを確認する。
3. コードの修正依頼をする場合は修正するコードをホバーして「➕」を押す。
4. コメントを入力して「Add single comment」を押す
5. コードレビューがリクエストした人に送信される。


# プルリクエストした内容を承認する場合

1.githubで「Pull request」タブの「Review changes」を押す。
2.「Approve」を選択し「Submit review」を押す。


# 承認されたリクエストをマージする方法

1. githubで「Pull request」タブの中の「conversation」タブで「Merge pull request」を選択する。
2.マージメッセージを確認し「Comfirm merge」を押す。
3.「Delete branch」ボタンを押してプルリクエストブランチを削除する。


# マージした内容をローカルにも取り込みプリクエストブランチを削除する。

git checkout master
git pull origin master
git branch -d pull_request

```

#### プルにはマージ型とリベース型がある
```bash
# マージ型のプル（通常のプル fetch → mergeと同じ挙動）
git pull <リモート名> <ブランチ名>
git pull origin master

## タグ
コミットを参照しやすくするために、分かりやすく名前を付けるのがタグ。
よくリリースポイントに使います。

#### タグの一覧を表示する
```bash
git tag

# パターンを指定してタグを表示
git tag -l "201705"
20170501_01
20170502_01
20170502_02
```
#### タグには２つの種類がある
・注釈付き版（annotated）
・軽量版（lightweight）

#### 注釈付き版のタグの作成
```bash
git tag -a <タグ名> -m "<メッセージ>"
git tag -a 20200203_01 -m "version 20200203_01"
```

#### 軽量版のタグの作成
```bash
git tag <タグ名>
git tag 20200203_01
```

#### 昔のコミットにタグを付ける
```bash
git tag -a <タグ名> <コミットID> -m "<メッセージ>"
git tag -a 20200203_01 hfk9dsh -m "version 20200203_01"

git tag <タグ名> <コミットID>
git tag 20200203_01 hfk9dsh
```

#### タグのデータを表示する
```bash
git show <タグ名>
git show 20200203_01
```

#### タグをリモートリポジトリに送信する方法
```bash
git push <リモート名> <タグ名>
git push origin 20200203_01

# タグを一斉に送信する
git push origin --tags
```

#### githubでタグを確認する
Codeタブ → Releaseタブ →　Tags


git switch

# ブランチを切り替える
```
$ git switch <branch>
```

# ブランチを新規作成して切り替える
```
$ git switch -c <branch>
```

git restore

# ファイルの変更を取り消す
git restore <filename>

# 特定ファイルを特定のコミットの状態にする
git restore --source <commit> <filename>

## その他

#### コマンドにエイリアスをつける
```bash
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.br branch
git config --global alias.co
```
* エイリアスをつける事で設定が楽になる
* git config は設定を変更するコマンド
* `--global` をつけるとPC全体の設定を変更するコマンドになる
* `--global` を付けないと特定のプロジェクトにしか反映されない。
* 今回の省略コマンドは全てのプロジェクトで使用するので--grobalをつける

#### Gitで管理したくないファイルを外す
* パスワード情報などが記載されているファイル
* windwsやmacで自動生成されるファイル
* キャッシュ
* チーム開発に必要ないファイル

##### .gitignoreファイルに指定する
```bash

"4つの指定方法"

# 指定したファイルを除外
index.html

#ルートディレクトリを指定
/root.html

# ディレクトリ以下を除外
dir/

# /以外の文字列にマッチ「*」
/*/*.css
```
