# canvas

- HTML5 から導入された要素
- canvas タグが設置された箇所に JavaScript によって 2D もしくは 3D の図形を描画することができる

### メソッド

- 描画コンテキストを取得
  ```
  canvas = document.getElementById('canvas');
  if ( ! canvas || ! canvas.getContext ) {
    return false;
  }
  ctx = canvas.getContext('2d');
  screenWidth = canvas.width;
  screenHeight = canvas.height;
  ```
- clearRect(左端の x 座標,上側の y 座標,幅,高さ)
  - 指定した範囲のキャンバスを消す
  - アニメーションの各フレームの開始時に必要
  - clearRect() を呼び出した後、必ず新しいアイテムを描画し始める前に beginPath() を呼び出す
  - https://developer.mozilla.org/ja/docs/Web/API/CanvasRenderingContext2D/clearRect
- beginPath()
  - 現在のパスをリセットする
  - これから描画することを宣言する役割
- moveTo(x,y)
  - 新しいサブパスの開始点を座標指定する
  - あくまで描画の目以来であり、実際の描画は stroke()で行う
- lineTo(x,y)
  - 直前の座標と指定座標を結ぶ直線を引く
  - あくまで描画の目以来であり、実際の描画は stroke()で行う
- stroke()
  - 実際に描画する
  - サブパスを輪郭表示する
- bezierCurveTo(x1,y1,x2,y2,x3,y3)
  - ベジェ曲線を引く
- arc(中心 x,中心 y,半径,開始角度,終了角度,反時計周りか)
  - 円弧を描く
  - 角度の測り方は、反時計回りのフラグがどちらにせよ、3 時の方向を 0 度とした時計回りである
  - 角度の指定はラジアンで行う
    - ラジアン = 度 / 180 \* π
    - π は Math.PI で得られる
- ellipse(中心 x,中心 y,傾き,横半径,縦半径,開始角度,終了角度,反時計周りか)
  - 楕円を描く
- rect(左端の x 座標,上側の y 座標,幅,高さ)
  - 四角形を描く
- closePath()
  - 視点と終点をつなぐ線を引く
- fill()
  - 実際に描画する
  - サブパスの中を塗りつぶす
- fillText("文字列",x,y)
  - 指定した文字列を、x,y の位置を中心として塗りで描く
  - beginPath(),closePath(),fill()は不要
- strokeText("文字列",x,y)
  - 指定した文字列を、x,y の位置を中心として枠線で描く
  - beginPath(),closePath(),fill()は不要

### 属性

- lineWidth
  - 線の太さを表す属性
  - 初期値は 1
  ```
  // 線の太さを3に指定
  ctx.lineWidth = 3;
  ```
- fillStyle

  - 塗色を表す属性

  ```
  // 図形を塗る色を赤に指定
  ctx.fillStyle = 'red';

  // 図形を塗る色をカラーコードで指定
  ctx.fillStyle = '#990000';

  // 図形を塗る色をRGBで指定
  ctx.fillStyle = 'rgb(160,160,160)';

  // 図形を塗る色を透明値を含めて指定
  ctx.fillStyle = 'rgba(255,0,0,0.5)';
  ```

- strokeStyle

  - 線の色を表す属性

  ```
  // 線の色を赤に指定
  ctx.strokeStyle = 'red';

  // 線の色をカラーコードで指定
  ctx.strokeStyle = '#990000';

  // 線の色をRGBで指定
  ctx.strokeStyle = 'rgb(160,160,160)';

  // 線の色を透明値を含めて指定
  ctx.strokeStyle = 'rgba(255,0,0,0.5)';
  ```

- font

  - 文字を描く時の大きさと書体を指定

  ```
  ctx.font = "60px Serif";
  ```

- textAlign
  - 行揃えを指定
  - right,center,left から指定
  ```
  ctx.textAlign = "right";
  ```

### ポイント

- 塗りと枠線の順番による見た目の違い
  - 塗り → 枠線：枠線の太さによって本来の文字の内側が侵食される形で描かれる
  - 枠線 → 塗り：枠線の太さは関係なくなる形で描かれる
