# BOOKSHELF 開発でのつまづき

### DB を使わないデータ保持の方法

#### やりたかったこと

- DB を利用しない（将来的に個人情報の入ったデータを扱いたくなった場合、DB にデータを保持したくない）
- 一度データを登録したらアクセスし直した時も保持したままでいてほしい
- 検索結果や登録途中のデータは一時的な保持で OK
- 別デバイスで利用する際、データを共有したい

#### 実現方法

- localStorage、Store、csv ファイルの組み合わせ
  | 保存場所 | 保存したいもの | 具体的な保存内容 |
  |---|---|---|
  |localStorage | 一度入力したらずっと保持してほしいデータ<br>セキュリティ上の責任から、ローカルに保持したいデータ|バックアップデータの保存先 URL（保存先をクラウドにするとなったら必要）<br>登録済の書籍情報|
  |Store|一時的に保持したいデータ<br>リロードしたら消えて OK なデータ|書影 API からの検索結果<br>捜査対象の書籍情報|
  |csv ファイル|人と共有したいデータ<br>異なるデバイス間で共有したいデータ|登録済みの書籍情報|

### データを csv データに変換する

#### 前提

元データは、オブジェクトの配列

```
items = [
  {
    id:"1",
    image:"サムネ画像のURL",
    isbn:"isbnコード",
    title:"本のタイトル",
    type:"紙"
  },
  {
    id:"2",
    image:"サムネ画像のURL",
    isbn:"isbnコード",
    title:"本のタイトル",
    type:"電子"
  },
  ...
]
```

#### 利用メソッド

- Object.keys()
  - 指定されたオブジェクトが持つプロパティの 名前の配列を、通常のループで取得するのと同じ順序で返す
  - https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/keys
- .join()
  - 配列（または配列風オブジェクト）の全要素を順に連結した文字列を新たに作成して返す
  - https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/join
- Null 合体演算子 (??)
  - 左辺が null または undefined の場合に右の値を返し、それ以外の場合に左の値を返す
  - https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing
- JSON.stringify()
  - JavaScript のオブジェクトや値を JSON 文字列に変換する
  - https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#replacer_%E3%81%A8%E3%81%97%E3%81%A6%E9%96%A2%E6%95%B0%E3%82%92%E7%94%A8%E3%81%84%E3%82%8B%E4%BE%8B
- スプレッド構文（...）
  - https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Spread_syntax
- Blob()
  - 引数で与えられた配列から Blob オブジェクトを返す
  - https://developer.mozilla.org/ja/docs/Web/API/Blob/Blob
- Document.createElement()
  - 指定された HTML 要素を生成する
  - https://developer.mozilla.org/ja/docs/Web/API/Document/createElement
- URL.createObjectURL()
  - 引数で指定されたオブジェクトを表す URL を含む DOMString を生成する
  - https://developer.mozilla.org/ja/docs/Web/API/URL/createObjectURL
- .download()
  - download 属性が設定された a タグをクリックした場合、ブラウザはユーザーをそのコンテンツのページにナビゲートするのではなく、 コンテンツをファイルに保存する
  - HTML5 からの機能
  - https://javascript.keicode.com/newjs/download-files.php
- HTMLElement.click()
  - 要素のマウスクリックをシミュレートする
  - https://developer.mozilla.org/ja/docs/Web/API/HTMLElement/click
- Element.remove()
  - 所属するツリーから要素を削除する
  - https://developer.mozilla.org/ja/docs/Web/API/Element/remove

#### 実装

```
// 配列の1つ目の要素を取り出し、そのkeyを取得
const header = Object.keys(items[0]);
// ['id', 'title', 'isbn', 'image', 'type']

// header行を文字列化する
const headerString = header.join(",");
// id,title,isbn,image,type

// key,valueを受け取り、valueに該当する値を返す関数を定義
const replacer = (key, value) => value ?? "";

// header行以外のデータを文字列化する
const rowItems = items.map((row) =>
  header.map((fieldName) => JSON.stringify(row[fieldName], replacer))
  .join(",")
);

// headerとコンテンツ部分を結合する
const csv = [headerString, ...rowItems].join("\r\n");

// BOM付UTF-8にすることで文字化けを防ぐ
const bom = new Uint8Array([0xef, 0xbb, 0xbf]);

// 配列をBlobオブジェクトに変換する
const blob = new Blob([bom, csv], {
  type: "text/csv",
});

// csvファイルを生成し、ダウンロードする
// aタグを生成する
const link = document.createElement("a");
// 生成したaタグのhref属性に、生成したBlobオブジェクトを表すURLを設定する
link.href = URL.createObjectURL(blob);
// a要素にdownload属性を設定する
link.download = "books.csv";
// a要素をclickした時の動作を再現する
link.click();
// DOMツリーから要素を削除する
link.remove();
```

### localStorage と Store 間のデータ受け渡し

- localSrorage はすべてのデータを文字列化して保存しているため、オブジェクトをそのまま保存することができない

#### localStorage のデータを json 化する

```
JSON.parse(localStorage.getItem("key名"));
```

#### オブジェクト を 文字列化する

```
JSON.stringify(data);
```
