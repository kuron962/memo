# bat ファイル作成の備忘録

### 変数へ代入

- = の後にスペースを入れると代入がうまくいかない。

```
set 変数名=値
```

### 変数を使用

```
%[変数名]%
```

### 処理を実行したときのコマンドを表示させない

```
@echo off
```

### 置換

```
%[変数名]:[置換元]=[置換先]%
```

### 時刻を取得

```
%time%
```

### 一時停止

- /t で停止する時間を指定
- /nobreak でキー操作の影響を排除
- > nul で停止中待機時間カウントを非表示

```
timeout /t 5 /nobreak
```

### 起動中のポート一覧を取得

```
netstat -ano | findstr :ポート番号
```

### 起動中のプログラムを停止

- netstat を実行し、状態（LISTENING 等）の後の番号を控える。

```
taskkill /PID 控えた番号 /F
```

### プログラムの実行

- 指定したプログラムを呼び出した後、その終了を待たずに次のプログラムに進む
- プログラムは、cmd の別ダイアログが開き実行される
- start の後に /b をつけることで、別ダイアログは開かず、同一ダイアログで実行される

  ```
  start 実行するプログラム
  ```

- 指定したプログラムを呼び出し、終了するまで次のプログラムの実行を保留する
  ```
  call 実行するプログラム
  ```

## jar ファイルの実行

### やりたいこと

- jar ファイルを実行し、localhost:8080 で SpringBoot アプリケーションを起動
- npm run start により、localhost:3000 で Nuxt アプリケーションを起動

### 生じた現象

- localhost:3000 でフロントエンドアプリは正常に起動するが、localhost:8080 でバックエンドアプリが起動せず、API に接続できない。

  ```
  // jarファイルを実行
  start ./bookTower/bookTower/build/libs/bookTower-0.0.1-SNAPSHOT.jar

  // localhost:3000をブラウザで開く
  start http://localhost:3000

  // Nuxtアプリフォルダに移動
  cd ./front-bookTower/

  // Nuxtアプリを実行
  npm run start
  ```

- バックエンドアプリの起動に時間がかかり、フロントエンドアプリ起動時にまだ起動しておらず、アクセスできないと考えた。timeout でフロントエンドアプリ実行までにブランクを作ってみた。

  ```
  start ./bookTower/bookTower/build/libs/bookTower-0.0.1-SNAPSHOT.jar

  // 5秒のブランクを入れる
  timeout /t 5 /nobreak

  start http://localhost:3000
  cd ./front-bookTower/
  npm run start
  ```

- netstat で確認したところ、上のプログラムでは、localhost:8080 が起動していない様子。上のプログラム実行後、ブラウザで localhost:8080 にアクセスした後、localhost:3000 をリロードすると成功する。どうやら、アクセスしたタイミングで 8080 が起動しているぽい？
- start コマンドで、localhost:8080 の起動を組み込む。

  ```
  start ./bookTower/bookTower/build/libs/bookTower-0.0.1-SNAPSHOT.jar
  timeout /t 5 /nobreak

  start http://localhost:8080

  start http://localhost:3000
  cd ./front-bookTower/
  npm run start
  ```

- 上のプログラムで正常に起動はしたが、ブラウザにて localhost:8080 を開きたくない。無駄なタブが開いて邪魔。
- jar ファイルの実行時に localhost:8080 も起動したい。jar ファイルの起動方法には、① ダブルクリック（= start コマンド）と ②java -jar コマンドの 2 つがある。試しに java -jar コマンドを使用。

  ```
  java -jar ./bookTower/bookTower/build/libs/bookTower-0.0.1-SNAPSHOT.jar

  start http://localhost:3000

  cd ./front-bookTower/
  npm run start
  ```

- 起動はしたが、cmd が２つ開いて邪魔。start /b で新規ダイアログを開かずに実行できることを思い出し、使用。上のプログラム実行時、jar の起動に 6 秒ちょっとかかっていたため、timeout でブランクを入れた。成功。

  ```
  start /b java -jar ./bookTower/bookTower/build/libs/bookTower-0.0.1-SNAPSHOT.jar

  timeout /t 8 /nobreak

  start http://localhost:3000

  cd ./front-bookTower/
  npm run start
  ```
