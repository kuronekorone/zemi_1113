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

```html
    <input type="text" name="name"value="<%= form.name %>">
    <input type="text" name="mail"value="<%= form.mail %>">
    <input type="text" name="age"value="<%= form.age %>">
    <input type="submit" value="作成">
```
 
