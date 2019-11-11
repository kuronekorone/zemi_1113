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
- フォームがサーバーへ送信された時、その送られてきた値の内容をチェックし、
正常ならそのまま保存
問題があればエラーメッセージなどをつけてフォームを再表示させられる
- と、言うことでいつも通りインストール  |

+++

```js
    <input type="text" name="name"
                    value="<%= form.name %>">
    <input type="text" name="mail"
                    value="<%= form.mail %>">
    <input type="text" name="age"
                    value="<%= form.age %>">
    <input type="submit" value="作成">
```
 
