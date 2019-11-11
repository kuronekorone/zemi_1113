## Node.js 超入門

### ECL 輪読発表
#### 石川優輝

---

## OUTLINE

- ### 6-1 バリデーション
- ### 6-2 Bookshelfデータベースを使いこなそう
- ### 7-1 DB版ミニ伝言板

---

## 6-1 バリデーション

+++

### バリデーションとはなんぞや？
- ざっくり言うと入力チェックしてくれる偉いやつ
- と、言うことでいつも通りインストール
--

+++

```js
    <body>
    <head>
        <h1><%= title %></h1>
    </head>
    <div role="main">
        <p><%- content %></p>
        <form method="post" action="/hello/add">
        <table>
            <tr>
                <th>NAME</th>
                <td><input type="text" name="name"
                    value="<%= form.name %>"></td>
            </tr>
            <tr>
                <th>MAIL</th>
                <td><input type="text" name="mail"
                    value="<%= form.mail %>"></td>
            </tr>
            <tr>
                <th>AGE</th>
                <td><input type="text" name="age"
                    value="<%= form.age %>"></td>
            </tr>
            <tr>
                <th></th>
                <td><input type="submit" value="作成"></td>
            </tr>
        </table>
        </form>
    </div>
</body>
```
 
