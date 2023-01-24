# HTML&CSS
## HTMLとは
- webページの構成を設定する言語
- タグによってテキストに意味を持たせる

## 改行されるタグとされないタグ
- div,spanはともに単体では意味を持たないが、一部の範囲をくくること、そこに対し限定的にCSSを適用できる
  - div : ブロック要素→改行される
  - span : インライン要素→改行されない
  <br>→文章の一部単語にCSSを適用したいときなどに有効
### 改行されるタグ（ブロック要素）
親要素の幅いっぱいに広がる
- div 
- h1,h2,h3,...
- p : 見出し以外の本文
### 改行されないタグ（インライン要素）
- span
- a : リンク
- img : 画像

### インラインブロック要素
- インライン要素（横並び、改行されない）
＋
ブロック要素（width,height,margin,padding指定可）

### classとid
- どちらもCSSでセレクタとして利用できる
-  class : 同じWebページ内で同じclass属性の値を何度も使える
-  id : 同じwebページ内でid属性の値が重複してはならない

## URLを入力するとき
### src
- URLを埋め込む
- 画像（imgタグ）で使用する
### href
- URLに移動する
- CSSの指定やリンクで使用する
### linkとaの違い
- link : HTMLに他のファイルを読み込む
- a : ハイパーリンクをつくる
  
## 全体の構成
1. DOCTYPE宣言
2. html
   - head：Webページの設定情報、ページには表示されない
     - meta : 文字コード指定
     - meta : viewport設定（レスポンシブデザイン適用時）
     - title : ページタイトル
     - link : CSSの読み込み
       - rel : relation,リンク先オブジェクトとリンク元オブジェクトとの関係を指定する 
   - body : webページの中身、表示される部分
     - header : ヘッダー
     - footer : フッター
---

## CSSとは
- webページの見た目を設定する言語
- HTMLの要素に対して色、大きさ、配置などを指定する
- xxx { yyy : zzz ;}
  - xxx : セレクタ（どこの）
  - yyy : プロパティ（何を）
  - zzz : プロパティ値（どうする）

### プロパティ
- color : 文字の色
- font-size : 文字の大きさ
- font-family : 文字のフォント
- background-color : 背景色
- width : 要素の横幅
- height : 要素の高さ
- list-style:none; : リストの先頭マークを消去
- float : 要素の並び方
- padding : 要素の余白
- border : 枠線
- margin : 要素の外（枠線の外側）の余白
- background-image:url(); : 背景画像
- background-size:cover; : 背景画像1枚で表示範囲を埋め尽くす
- opacity : 要素の透明度
- letter-spacing : 文字の間隔
- display : 要素の種類を変更
- border-radius : 角を丸める
- text-align : テキスト、インライン要素、インラインブロック要素の配置
- transition : 変化の仕方
- line-height : 行の高さ
  - 要素の高さと同じ値にすると、文字が縦方向中央に配置される
- font-weight : 文字の太さ
- position:absolute; : 要素を重ねて表示
- position:relative; : 基礎の要素の左上を基準位置に設定
- box-shadow : 影の位置、色
- box-shadow:none; 影を消去
- position:fixed; : 要素の位置を固定
- z-index : 要素の重なりの順序を指定
  - positionプロパティと併用必須 
- box-sizing:border-box; : 要素幅の合計にpaddingとborderが含まれる<br>
  →レイアウトの管理が楽になる（特に％で幅を指定するとき）
  - 適用する際は、*(全ての要素)に対して指定するのが推奨される
- max-width : 要素の幅の上限<br>
  →画面幅が大きくなった時に広がりすぎないため、レスポンシブデザインに有用
- display:none; : 要素を非表示
- display:block; : 非表示にした要素を表示


### その他
- 要素の中央寄せ（広い範囲を囲うブロック要素）
  - .class名{width:○○px; margin:0 auto;}
  - autoを指定するときはwidthの指定必須
  - 上下にautoは指定不可 
- 要素の中央寄せ（インライン要素、インラインブロック要素）
  - text-align:center;
  - ボタンやテキストを中央寄せしたい場合に使う
- ボタンの挙動
  - セレクタ:hover{} : カーソルが乗っている時
  - セレクタ:active{} : クリックしている時
- アイコン表示
  - Font Awesomeを使うと便利<br>
  1.headでFont AwesomeのCSSファイルを読み込む<br>
  2.spanタグにclass="fa fa-アイコン名"を指定する
- 色を透明にする
  - 要素全体を透明にする
    - opacity:透過度;
  - 要素の背景の身を透明にする
    - background-color:rgba(r,g,b,透過度);
     

## レスポンシブデザイン
デバイスや画面サイズに合わせて、コンテンツのレイアウトを調整するためのもの
- メディアクエリ
  - ブラウザの画面サイズに応じてCSSのスタイルを設定する、レスポンシブデザインを実現するための手法
  - @media(条件){}
- レスポンシブデザインを適用するときは、幅の指定などをpxではなく％で行った方が楽（いちいち値を調整しなくていいから）   
- HTMLのheadでviewportを設定する
  - vierport : 表示領域
  - メディアクエリ用のCSSを通常のCSSと分けて作成する場合、CSSはviewportの後で読み込む
- 子要素が全てfloatでも、親要素の高さが子要素を包む高さを維持する
  -  floatの子要素は、親要素からみると存在しないように見えるため、何もしないと親要素の高さが0になってしまう
  1. htmlにclearクラスを持つdivを追加（中身はなし）
  2. cssにて、clearクラスにclear:left;を適用

## Flexbox
従来より簡単に要素を横並びにできるようになった、CSSの新機能
- display:flex : 指定した要素の子要素を横並びにする
- flex:auto : 指定した要素の幅を、親要素の幅に合わせて伸縮させる
- flex-wrap:wrap; : 子要素のサイズに応じて折り返す
  - 子要素にwidth:50%を指定し、親要素に上記の指定をすると、子要素2個で折り返される
- flex-direction:column; : 指定した要素の子要素を立てに並べる