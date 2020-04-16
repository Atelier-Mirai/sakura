# データベース

## [データベース](https://ja.wikipedia.org/wiki/データベース)
データベース（英: database, DB）とは、検索や蓄積が容易にできるよう整理された情報の集まり。 通常はコンピュータによって実現されたものを指すが、紙の住所録などをデータベースと呼ぶ場合もある。 狭義には、データベース管理システム (Database Management System, DBMS) またはそれが扱う対象のことをいう。(Wikipediaより)

## [CRUD](https://ja.wikipedia.org/wiki/CRUD)
CRUD（クラッド）とは、ほとんど全てのコンピュータソフトウェアが持つ永続性[1]の4つの基本機能のイニシャルを並べた用語。その4つとは、Create（生成）、Read（読み取り）、Update（更新）、Delete（削除）である。ユーザインタフェースが備えるべき機能（情報の参照/検索/更新）を指す用語としても使われる。(Wikipediaより)

## [ACID](https://ja.wikipedia.org/wiki/ACID_(コンピュータ科学))
ACIDとは、信頼性のあるトランザクションシステムの持つべき性質として1970年代後半にジム・グレイが定義した概念で、これ以上分解してはならないという意味の原子性（英: atomicity、不可分性）、一貫性（英: consistency）、独立性（英: isolation）、および永続性（英: durability）は、トランザクション処理の信頼性を保証するために求められる性質であるとする考え方である。(Wikipediaより)

## SQLite
様々なデータベースがあります。
ここでは、初心者向けに学習しやすいSQLiteをご紹介します。

[SQLite入門](http://www.dbonline.jp/sqlite/)   (http://www.dbonline.jp/sqlite/) で学習されて下さい。

## SQL文 サンプル

```
select * from stock;
select name from stock;
select * from stock order by quantity;
select * from stock order by quantity desc;
select * from stock where quantity > 100;
select * from stock where id >= 300 and quantity > 0;
select * from stock where id >= 300 and quantity > 0 order by quantity desc;
select * from stock where name like "%卵%";
select * from stock where name like "卵%";
select * from stock where name like "%卵";
select * from stock;

-- insert into stock values(400, "キャベツ", 123);
-- insert into stock values(401, "白菜", 234);
select * from stock;

-- insert into stock(name, quantity) values("玉ねぎ", 345);

select * from stock where id > 400;

-- INSERT INTO stock DEFAULT VALUES;

select * from stock;

--UPDATE stock SET quantity = 50 WHERE id = 400;
-- select * from stock where id = 400;

-- UPDATE stock SET name = "愛知県産キャベツ" WHERE id = 400;
-- select * from stock where id = 400;

--UPDATE stock SET quantity = 100 WHERE id = 400;
--UPDATE stock SET quantity = 1000;

--DELETE FROM stock WHERE id=300;
DELETE FROM stock;
select * from stock;

insert into invoice(company, tel) values("田中産業", "03-xxxx-xxxx");
insert into invoice(company, tel) values("丸八青果", "06-xxxx-xxxx");
insert into invoice(company, tel) values("伊藤農園", "052-xxxx-xxxx");

select * from invoice;

```

## C言語でのデータベース操作例

[include](lecture_extra/sqlite3_sample.c)
