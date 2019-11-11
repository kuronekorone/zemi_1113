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
#### フォームに用意しているinputタグにvalue属性を追加
- フォームに再入力してくれる |
 
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

```ja:6-4 GET
router.get('/add', (req, res, next) => {
    var data = {
        title: 'Hello/Add',
        content: '新しいレコードを入力：',
        form: {name:'', mail:'', age:0}
    }
    res.render('hello/add', data);
});
```
* 初期状態の定義をするGET

+++






