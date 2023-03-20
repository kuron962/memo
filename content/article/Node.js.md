# Node.js
## Node.js とは
- JavaScriptをサーバサイドで動かすための環境、仕組み
- 非同期処理により、大量のアクセスを高速に裁くことが可能
  - ### 非同期処理
    - あるタスクを実行している最中に、その処理を止めることなく別のタスクを実行できる方式
    - > 並行処理：複数の処理を1つの主体が切り替えながらこなすこと<br>
並列処理：複数の処理を複数の主体で同時にこなすこと<br>
同期処理：複数の処理をこなす際、ある処理が別の処理の終了を待つような処理<br>
非同期処理：複数の処理をこなす際、ある処理は別の処理の終了を待たないような処理<br>
<br>
それぞれの言葉の目的<br>
並行処理：処理を「同時に進行させる」ことが目的<br>
並列処理：処理を「速くこなすこと」が目的<br>
同期処理・非同期処理：別の処理を「待つ必要があるか・ないか」を語ることが目的<br>
[処理方式の違い](https://qiita.com/kyabetsuda/items/384a57ff6b7250de40ad)
- モジュール機能により多数のパッケージが後悔されており、様々な機能を簡単に組み込むことが可能
- パッケージやフレームワークはnpmからインストールして使用する

## フォルダ構成
list-app(親フォルダ)
- app.js
- views (見た目に関するファイルを置く場所)
  - top.ejs (ビューファイル)
- public (CSSや画像を置く場所)
  - css (CSSを置く場所)
  - images (画像を置く場所)

## app.jsの構成
```
//expressパッケージの準備
const express = require('express');
const app = express();

//publicフォルダ内のファイルを読み込めるように設定
app.use(express.static('public'));

具体的な処理部分

//サーバ起動
app.listen(3000);
```

### 処理部分の主なコード
- ルーティング
  ```
  app.get('/(URLを記入)', (req,res) =>{
    //トップ画面を表示する処理
    res.render('top.ejs');
  });
  ```
  - URLに対応する処理を実行する
  - req(リクエストに関する情報),res(レスポンスに関する情報)を引数として受け取る
  - *req,resに入っている具体的な内容は？*
  - res.renderで指定したファイルを画面に表示させる
  - 最初にアクセスするページ（トップ画面）は、ルートURL('/')にする
  - req,resはJavaScript標準（＝最初から用意されている）オブジェクト
    - req.body.name属性 等に値を保持できる仕組みは、reqというリクエストに関する情報をすべて入れるオブジェクトがあり、そこに新しいプロパティを追加しているというイメージ 
   


## 主なパッケージ
- Express
  - Node.jsでWebアプリを開発するためのフレームワーク
    - ### フレームワーク ＝ 骨組み
      - システム開発時によく使う機能や設計などを予め用意してあるアプリケーション
    - ### ライブラリ ＝ 部品
      - 機能を持ったコードの集まり
    - ### パッケージ ＝ 部品を整理したもの
      - ライブラリの中で、似ている機能をまとめたもの
  - 使い方
  1. インストール(ターミナル)<br>
   ```npm install express``` 
  2. インストールしたパッケージの読み込み(app.js)<br>
   ```const express = require('express');```
  3. 使用する準備(app.js)<br>
   ```const app = express();```
  4. サーバーの起動(app.js)<br>
   ```app.listen(3000);```
     - localhost:3000でアクセス可能なサーバーを起動
  5. app.jsの実行(ターミナル)<br>
   ```node app.js```

- EJS
  - ブラウザに表示する見た目部分を定義
  - 役割としてはHTML
  - HTMLとJavaScriptのコード両方を記述できるテンプレートエンジン
    - *テンプレートエンジンとは？*
   ```
  <!DOCTYPE html>
  <html>
      <head>
          <title></title>
          <link rel = "stylesheet" href = "/css/style.css">
          ...
      </head>
      <body>
          ...
      </body>
  </html>
  ```
  - cssや画像は、publicフォルダを起点としたパスを指定

  - 使い方
  1. インストール(ターミナル)<br>
   ```npm install ejs``` 
  2. JavaScriptコードの書き方
   ```
   //変数の定義など、ブラウザに表示しない部分
   <% %>

   //変数の値など、ブラウザに表示する部分
   <%= %>
   ```
  3. フォームに初期値を表示<br>
   - value属性に値を指定すると初期値を表示できる
   ```
   <input value="<%=item.name%>" type="text>
   ```

- mysql
  - Node.jsをMySQLに接続するパッケージ
  -  使い方
  1. インストール(ターミナル)<br>
   ```npm install mysql``` 
  2. インストールしたパッケージの読み込み(app.js)<br>
   ```const mysql = require('mysql');```
  3. 接続情報を代入(app.js)<br>
   ```
   const connection = mysql.createConnection({
    //データベース名、パスワード等
   });
   ```
  4. クエリの実行(app.js)<br>
  - error : クエリが失敗したときのエラー情報
  - results : クエリの実行結果
   ```
   connection.query(
    'クエリ',
    //クエリ実行後の処理
    (error,results) =>{
        console.log(results);
        res.render('○○.ejs');
    }
   );
   ```
  5. EJSへの値の受け渡し（データ表示など）
   ```
   //app.js
   res.render('○○.ejs',{プロパティ名:値})
   ```
   ```
   //○○.ejs
   <% プロパティ名 %>
   ```
  6. データベースを変更<br>
   ```
   //○○.ejs
   <form action = "URL" method = "post">
   ```
   ```
   //app.js
   app.post('URL',(req,res) =>{

   })
   ```

  7. フォームの値を受け取る<br>
  - req.body.aaa(name属性)に入力値が入る
   ```
   //○○.ejs
   <form action = "URL" method = "post">
    <input type="" name="aaa">
   </form>
   ```
   ```
   //app.js
   //フォームの値を受け取るのに必要な定型文
   app.use(express.urlencoded({extended:false}));
   ```

  8. フォームからの値をクエリに使う(app.js)
   ```
   connection.query(
    'クエリ(入力値を使う部分は「?」)',
    [req.body.aaa(渡したい配列)],
    (error,results) => {
        //クエリ実行後の処理
    }
    );
   ```
  9. リダイレクト
   - 別のURLに再度リクエストさせる仕組み
   - postのときによく使う（リロードによって繰り返しデータベースの変更処理が行われるのを防ぐため）
   ```
   res.redirect('URL);
   ```
  10. ルートパラメータ
   - URLを利用してデータを受け渡す
   - req.params.ルートパラメータ名で値を受け取れる
   ```
   //○○.ejs
   //送信先URLにメモのidを含める
   <form action="/delete/<%=item.id%>" method="post">
   ```
   ```
   //app.js
   //ルーティングURLにルートパラメータを指定
   app.post('/delete/:id',(req,res) =>{
    //ルートパラメータの値を受け取ってコンソールに表示
    console.log(req.params.id);
   });
   ```

- express-session
  - セッションを管理するための機能を提供するパッケージ
  - 使い方
  1. インストール(ターミナル)<br>
   ```npm install express-session``` 
  2. インストールしたパッケージの読み込み(app.js)<br>
   ```const session = require('express-session');```
  3. 使用する準備(app.js)<br>
   ```
   app.use(
    session({
        secret: 'my_secret_key',
        resave: false,
        saveUninitialized: false,
    })
   )
   ```
  4. セッション情報にデータを保存
   - req.session..プロパティ名 = 値

  5. セッション情報を破棄
   - ```req.session.destroy((error) => {実行後の処理})```
   - errorには削除処理が失敗したときの情報が入る
 - bcrypt
   - ハッシュ化するパッケージ
   - 使い方
   1. インストール(ターミナル)<br>
    ```npm install bcrypt``` 
   2. インストールしたパッケージの読み込み (app.js)<br>
    ```const bcrypt = require('bcrypt');```
   3. ハッシュ化(app.js)<br>
     - hash : ハッシュ化された文字列
     - パスワードの強さ : 値が大きいほど強さ↑、処理速度↓
   ```
   bcrypt.hash(文字列,パスワードの強さ,(error,hash) => {

   });
   ```
    4. ハッシュ化された文字列と普通の文字列を比較(app.js)<br>
   - plain : 普通の文字列
   - hash : ハッシュ化された文字列
   - isEqual : 比較結果（true/false）
   ```
   bcrypt.compare(plain,hash,(error,isEqual) => {

   })
   ```
  

## app.use関数
- 全てのリクエストに対し、毎回実行される
- ルーティング処理の一番最初に書く
- next()で、リクエストに一致する次の処理（app.get等）を実行できる
- ```res.locals.プロパティ名 = 値```で値を代入すると、その値はレスポンスを返すまで使用可能になる
  - この値をEJSで使用する場合、```locals.プロパティ名```で使用可能
  ```
  app.use((req,res,next) => {
    res.locals.username = req.session.username;
    next();
  });
  ```

## EJSファイルで別のEJSファイルを呼び出す
- <%- include('ファイル名'); %>

## ミドルウェア関数
- リクエストとレスポンスの間にサーバーが実行する関数
- 複数のミドルウェア関数をつくれる
  - app.use関数は、どのルーティングでも実行される
  - ミドルウェア関数は、それを記述したルーティングのときに実行される
- 上から順に実行される
  ```
  app.post('URL',
   (req,res,next) => {
    処理１
    next();
   },
   (req,res) => {
    処理２
   }
  );
  ```
