# AR マーカー対応の Web サイトをつくる

## やりたかったこと

- AR マーカーを web カメラで読み取ると、3D モデルが現れるサイトをつくる
- AR 名刺の作成を目指す

## 持っていた知識

- AR マーカーってなんぞや？
- web サイトで AR を実現する方法は何も知らない
- 基本的な HTML、JavaScript の知識はある

## 前提知識

### AR マーカーとは

## 手順

1. AR マーカーの材料となる画像を用意

- 今回は figma にて作成
- 模様が細かすぎるとマーカーにしたときに認識されないことがある模様。

2. AR マーカーを作成

- [AR マーカー作成サイト](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html) にアクセス
- 用意した画像をアップロード
- マーカーと画像をダウンロード
  - マーカー：.patt ファイル。これがマーカー本体。
  - 画像：.png ファイル。配布する際などはこちらを使う。
- 周りを囲っているボーダーの幅によって認識されないことがあるため注意。[参考](https://note.com/agw/n/n36ea390f5db1)

3. web サイトを作成（フォルダ構成はあくまで参考。変更可。）

   1. レポジトリを作成し、git にて管理する。
   2. レポジトリ内に asset フォルダを作成し、3D モデルデータを格納する。[参考](https://arqiita.com/emuyuu/items/98d04a671ed0d648334a)
   3. レポジトリ内に pattern フォルダを作成し、ダウンロード済の.patt ファイルを格納する。
   4. レポジトリ内に index.html を作成。以下コードを記述。

      ```
      <!DOCTYPE html>
      <html>
        <head>
          <!-- A-Frame を読み込む -->
          <script src="https://aframe.io/releases/0.8.2/aframe.min.js"></script>

          <!-- AR.js を読み込む -->
          <script src="https://cdn.rawgit.com/jeromeetienne/AR.js/1.6.2/aframe/build/aframe-ar.js"></script>
          <script src="https://cdn.rawgit.com/donmccurdy/aframe-extras/v5.0.0/dist/aframe-extras.min.js"></script>

          <!-- webサイトのタイトルを指定 -->
          <title>manabe kuzuya</title>

          <!-- css を読み込む -->
          <link rel="stylesheet" href="./css/style.css" />
        </head>
        <body>
          <!-- シーンを追加。真っ白のキャンバスを用意する感覚。表示するものは全てこのタグ内に書いていく。 -->
          <a-scene
            embedded
            arjs="debugUIEnabled:false;"
            renderer="gammaOutput: true;"
            vr-mode-ui="enabled: false"
          >
            <!-- アセット（表示したい3Dモデル）を読み込む -->
            <a-assets>
              <a-asset-item
                id="demo"
                src="./asset/demo.glb"
              ></a-asset-item>
            </a-assets>

            <!-- マーカーとオブジェクトを対応づける（読み込んだ3Dモデルを使う場合） -->
            <!-- マーカーのファイルを指定 -->
            <a-marker type="pattern" url="./pattern/pattern-ironman.patt">
              <!-- 表示するモデルをidで指定し、その位置や大きさ等を指定 -->
              <a-entity gltf-model="#ironMan" scale="1 1 1" animation-mixer>
                <!-- 表示するモデルの動きを指定 -->
                <a-animation
                  attribute="rotation"
                  from="20 0 0"
                  to="20 360 0"
                  direction="alternate"
                  dur="4000"
                  repeat="indefinite"
                  easing="ease"
                ></a-animation>
              </a-entity>
            </a-marker>

            <!-- マーカーとオブジェクトを対応づける（A-Frameで用意されているモデルを使う場合） -->
            <!-- マーカーのファイルを指定 -->
            <a-marker type="pattern" url="./pattern/pattern-sphere.patt">
              <!-- 表示するモデルをタグで指定し、その位置や大きさ等を指定 -->
              <a-sphere scale=".25 .25 .25" position="0 .5 0" color="#111">
                <!-- 表示するモデルの動きを指定 -->
                <a-animation
                  attribute="position"
                  to="0 1 0"
                  direction="alternate"
                  dur="2000"
                  repeat="indefinite"
                >
                </a-animation>
              </a-sphere>
            </a-marker>

            <!-- カメラを用意 -->
            <a-entity camera></a-entity>
          </a-scene>
        </body>
      </html>
      ```

4. web サイトを公開

- 今回は無料で静的 web サイトの公開ができる Netlify を使用。
- [参考](https://zenn.dev/shu00011/articles/0e5937ca1ff85b)

5. web サイトにスマホからアクセスし、サイト内のカメラで AR マーカーを読み取る

- ダウンロード済の AR マーカー（.png ファイル）をサイト内のカメラで読み取ると、3D モデルが表示される！
