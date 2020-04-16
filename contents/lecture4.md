# ファイルの取り扱い・文字の検索

ファイルの各種モード

|モード        |文字列|補足説明|
|:-------------|:-:|:----------|
|読み取りモード|```"r"```|ファイルが存在しなければエラー|
|書き込みモード|```"w"```|ファイルが存在しなければ新規作成。ファイルが存在すれば上書き|
|追記モード    |```"a"```|ファイルが存在しなければ新規作成。ファイルが存在すれば追記|


## 宣言など
[import:1-11, file_handling.c](lecture_4/file_handling.c)

## ファイルの読み込み
[import:12-55, file_handling.c](lecture_4/file_handling.c)

## ファイルの書き込み
[import:57-93, file_handling.c](lecture_4/file_handling.c)


## 得点ファイルを読み込んで、合計点・平均点を求める
[include](lecture_4/file_score.c)

## 文字の検索

詳細なアルゴリズム解説付
[include](lecture_4/string_find.c)

関数化
[include](lecture_4/string_find2.c)

## 文字の置換
[include](lecture_4/string_replace.c)

## ファイルでの文字の検索
[include](lecture_4/file_find.c)
