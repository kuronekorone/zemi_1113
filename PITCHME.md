## Node.js 超入門

### ECL 輪読発表
#### 石川優輝

---

## OUTLINE

- ### 6-1 バリデーション
- ### 6-2 Bookshelfデータベース
- ### 7-1 DB版ミニ伝言板

---

## 6-1 バリデーション

+++

### バリデーションとはなんぞや？
- ざっくり言うと入力チェックしてくれる偉いやつ
- フォームがサーバーへ送信された時内容をチェックし、正常ならそのまま保存、問題ならエラーを知らせる |
- と、言うことでいつも通りインストール  |

+++
### 6-1より抜粋
```html
    <input type="text" name="name"value="<%= form.name %>">
    <input type="text" name="mail"value="<%= form.mail %>">
    <input type="text" name="age"value="<%= form.age %>">
    <input type="submit" value="作成">
```
#### フォームに用意しているinputタグに
#### value属性を追加
#### フォームに再入力してくれる |
 
+++

```css:6-2
            .error {
             color: red;
            }
```
#### エラーメッセージのstylesheet

+++

```js:6-3
const{check, validationResult} = require('express-validator');
```
#### モジュールをロードしてvalidatorを追加

+++

```js:6-4 GET
router.get('/add', (req, res, next) => {
    var data = {
        title: 'Hello/Add',
        content: '新しいレコードを入力：',
        form: {name:'', mail:'', age:0}
    }
    res.render('hello/add', data);
});
```
#### 初期状態の定義をするGET

+++

```js
router.post('/add', (req, res, next) => {
    req.check('name','NAME は必ず入力して下さい。').notEmpty();
    req.check('mail','MAIL はメールアドレスを記入して下さい。').isEmail();
    req.check('age', 'AGE は年齢（整数）を入力下さい。').isInt();
```
#### reqのcheckメソッドを呼び出して
#### 具体的な内容を設定
##### notEmpty:空入力を禁止する
##### isEmail:メールアドレスのみ受け付け
##### isInt:整数のみ受け付け

+++

```js
    req.getValidationResult().then((result) => {
        if (!result.isEmpty()) {
            var re = '<ul class="error">';
            var result_arr = result.array();
            for(var n in result_arr) {
                re += '<li>' + result_arr[n].msg + '</li>'
            }
            re += '</ul>';
            var data = {
                title: 'Hello/Add',
                content: re,
                form: req.body
            }
            res.render('hello/add', data);
        } 
```
@[1](バリデーションの結果を受け取るメソッド)
@[2](空かどうかの識別)
@[3,4,5,6,7,8,9,10,11,12,13,14](resultからバリデーションの結果情報を配列として取り出す)
+++

```js
        else {
            var nm = req.body.name;
            var ml = req.body.mail;
            var ag = req.body.age;
            var data = {'name':nm, 'mail':ml, 'age':ag};
            
            var connection = mysql.createConnection(mysql_setting);
            connection.connect();
            connection.query('insert into mydata set ?', data, 
                    function (error, results, fields) {
                res.redirect('/hello');
            });
            connection.end();
```
#### 空でない時の処理

+++

### チェック対象を指定するメソッドについて
* check(項目名,エラーメッセージ)
* assert(項目名,エラーメッセージ)
* validate(項目名,エラーメッセージ)
 * checkと全く同じ働きをする |
* checkBody(項目名,エラーメッセージ)
 * bodyから探してチェック |
* checkParams(項目名,エラーメッセージ)
 * パラメータの値から探してチェック |
* checkQuery(項目名,エラーメッセージ)
 * クエリーテキストの中から探してチェック |

+++

### まとめ

- 入力された値をチェックする機能
    - バリデーション |
- <input>タグにvalue属性を追加する
    -  フォームに再入力するため |
- isEmail
    - メールアドレスのみ受け付け |
- isInt
    - 整数のみ受け付け |
- bodyから指定の名前のデータを探してチェック
    - checkBody |

---

## 6-2 Bookshelfで
 ##     データベースを使いこなそう

+++

## 前章ではMySQLを使って簡単に基本操作が作成できましたね！！！

+++

## どこが？？？？？

+++

### そんなあなたに！
### Bookshelf

+++
### Bookshelfとは

* 複数のSQLデータベースに対応している
* 複数のデータベースを変更するのに初期設定の値１つだけでいい
 * 普通はデータベースの変更時、周りの処理をすべて書く必要がある

+++

```html
<% var obj = content[i].attributes; %>
```
#### attributeプロパティのBaseModelオブジェクトを取り出す

+++

```js
var knex = require('knex')({
    dialect: 'mysql',
    connection: {
                host     : 'localhost',
                user     : 'root',
                password : '',
                database : 'my-nodeapp-db',
                charset  : 'utf8'
      }
});
```
@[1](requireしたオブジェクトの関数を実行)

+++

```js
var Bookshelf = require('bookshelf')(knex);
```
#### Bookshelfをロードし、関数を実行

+++

```js
var MyData = Bookshelf.Model.extend({
    tableName: 'mydata'
});
```
#### Bookshelfのメソッドを使用し、テーブルにアクセスするオブジェクトを作成



```js:6-6
new MyData().fetchAll().then((collection) => {
        var data = {
                    title: 'Hello!',
                    content: collection.toArray()
                };
                res.render('hello/index', data);
   })
   .catch((err) => {
        res.status(500).json({error: true, data: {message: err.message}});
   });
 ```
@[1,8](MyDataから全レコードを取り出す)
@[9](エラー情報をまとめた引数)

+++

#### 保存の処理
```js:6-7
router.post('/add', (req, res, next) => {
    var response = res;
    new MyData(req.body).save().then((model) => {
        response.redirect('/hello');
    });
});
```
@[3](MyDataオブジェクトに設定する値をreq.bodyに指定すると、送信されたフォームの値がそのまま設定できる)

+++

#### 検索の処理
```js:6-9
router.get('/find', (req, res, next) => {
    var data = {
        title: '/Hello/Find',
        content: '検索IDを入力：',
        form: {fstr:''},
        mydata: null
    };
    res.render('hello/find', data);
});
```
#### 検索画面の初期状態

+++

```js:6-9
router.post('/find', (req, res, next) => {
    new MyData().where('id', '=', req.body.fstr).fetch().then((collection) => {
        var data = {
            title: 'Hello!',
            content: '※id = ' + req.body.fstr + ' の検索結果：',
            form: req.body,
            mydata: collection
        };
        res.render('hello/find', data);
    })
});
```
@[2](whereとfetchの２つのメソッドがある)
@[2](where(項目名,比較記号,値)で比較した結果が帰ってくる)
@[2](fetchはレコードを１つだけ取り出す)

+++

### ページネーションとは
 * データベースにたくさんデータが入るようになると全部表示するのは大変！ |
 * なら、ページ分けしよう！ |

+++

```js:6-10
Bookshelf.plugin('pagination'); //★fetchPage追加
```
#### paginationプラグインを使えるようにする処理

+++

```js:6-10
router.get('/:page', (req, res, next) => {
    var pg = req.params.page;
    pg *= 1;
    if (pg < 1){ pg = 1; }
    new MyData().fetchPage({page:pg, pageSize:3}).then((collection) => {
        var data = {
            title: 'Hello!',
            content: collection.toArray(),
            pagination:collection.pagination
        };
    …以下略…
```
@[1](「:page」にページの値が入る)
@[2](req.paramsの中にページの値がまとめられている)
@[5](fetchAllのPage版)
@[9](ページネーションに関する情報をまとめたオブジェクト)

+++

```html:6-11
<div>
<span><a href="/hello/1">&lt;&lt; First</a></span>
｜
<span><a href="/hello/<%= pagination.page - 1 %>">&lt;&lt; prev</a></span>
｜
<span><a href="/hello/<%= pagination.page + 1 %>">Next &gt;&gt;</a></span>
｜
<span><a href="/hello/<%= pagination.pageCount %>">Last &gt;&gt;</a></span>
</div>
```
@[1](最初のページ)
@[3](前のページ)
@[5](次のページ)
@[7](最後のページ)

+++ 

### まとめ
- 複数のデータベースに対応し変更処理が簡単なモジュール
    - Bookshelf |
- 検索の処理をする際に条件を設定して絞り込みを行うメソッド
    - where |
- ページ分けの機能
    - ページネーション |
    
---

## 7-1 DB版ミニ伝言板

+++

### 要件定義的なもの
* ユーザー名とパスワードを登録
* 初回アクセス時は自動でログインページへ
* メッセージを記入するフォームがある
* メッセージの一覧がリスト表記される
* 削除しない限りデータを保持し続ける
* アカウント作成のリンクがある
* ユーザーの投稿をまとめてみれる

+++

### まずは５つのモジュールをインストール
*  npm install --save express-session
*  npm install --save express-validator
*  npm install --save mysql
*  npm install --save knex
*  npm install --save bookshelf

+++

#### 1.XAMPを起動
#### 2.コントロールパネルでApacheとMySQLを起動 |
#### 3.phpMyAdminにアクセス |
#### 4.my-nodeapp-dbを選択 |

+++

### usersテーブル
![users](users.jpg)

+++

### messageテーブル
![message](message.jpg)


