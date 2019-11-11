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

### まとめ

- 入力された値をチェックする機能
    - バリデーション |
- <input>タグにvalue属性を追加する
    -  フォームに再入力するため |
- isEmail
    - メールアドレスのみ受け付け |
- isInt
    - 整数のみ受け付け |

---







