# Vue.js
- HTML,CSS,JavaScriptが様々な形で繋がったが、その定型がなくバラバラだった
  →Vue.js等のフレームワークが注目を集める

## 使い方
```
  new Vue()({
    el: '#',   //Vueインスタンスを適用するHTMLのクラス・ＩＤを指定
    data: {},  //dataを定義、静的なデータのみ扱い可能
    computed: {}, //動的なデータを扱い可能、参照するdataが変更された場合のみ作動、()はつけない
    methods: {} //メソッドを定義、再描画される度に実行される（computedに比べいちいち動作してしまう）、()をつける
  })
```

## ディレクティブ
- Vue.js専用の属性
- v- が必ず付く

### v-text
- text

### v-once
- 一度描画されたらその内容を変更しない

### v-html
- htmlとして読み取り
- クロスサイトスクリプティング（XSS）の危険性あり
  →絶対に、ユーザーからの入力値を直接代入しない！！！！

### v-bind
- 属性に対してデータを結びつける
- 属性を引数にとる
- v-bind:href="url"
- :href="url"
- v-bind="{}"

### v-on
- DOMが提供するイベントが発生したときに特定の処理をする
- イベントを引数にとる
- [イベント表](https://developer.mozilla.org/ja/docs/Web/Events)
- @で省略可

### v-for
- key属性を必ずつける
- key属性を指定するときは、templeteではなくdivにする（templeteは表示されないため、keyが効力を発揮しない）

## イベント修飾子 ???
- .stop
- .prevent

## キー修飾子
- キーボードを指定
- v-on:keyup.enter //enterキーを押して離したとき

## 非同期処理、同期処理
- returnは同期処理でしか使えない →computedは非同期処理に向かない
- →非同期処理をしたい場合、watchを使う

### compornent
- 繰り返したいとき