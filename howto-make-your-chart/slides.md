# E2D3の
# 可視化テンプレートを
# 作ってみる

### E2D3基盤制作部

---

## E2D3 (Excel to D3.js) とは

データ可視化のテンプレートに<br>
Excelからデータを流し込むことのできるソフトウェア

1. エンジニアがデータ可視化テンプレートを<br>
   作って共有する
2. エンドユーザがそれにデータを流し込みそれを<br>
   ブログやSNSで公開する

---

## デモ

<image src="images/e2d3_demo.png" width="80%" />

---

## テンプレート開発に必要なもの

- [Node.js](https://nodejs.org/) v0.12.x
  - [nodebrew](https://github.com/hokaccha/nodebrew) (for MacOSX) or [nodist](https://github.com/marcelklehr/nodist) (for Windows) を使ってインストール
- Git
- [Google Chrome](https://www.google.com/chrome)
- [Microsoft Excel Online](https://office.live.com/start/Excel.aspx?ui=ja-JP)
  - Microsoftアカウントを作れば無料で使えます

### 使うと便利

- [Atom](https://atom.io/)

---

## 準備

### e2d3コマンドをnpm経由でインストール

```bash
$ npm install -g e2d3
```

### e2d3-contribレポジトリをクローン

```bash
$ git clone git@github.com:e2d3/e2d3-contrib.git
```

---

## テンプレート開発環境の起動

`e2d3-contrib`レポジトリをcloneしてきた<br>
ディレクトリ内で`e2d3`コマンドを実行

```bash
e2d3-contrib$ e2d3
[E2D3] Publish /Users/chimera/Sites/e2d3-server/e2d3/contrib
[E2D3] Webserver started at http://0.0.0.0:8000
[E2D3] Webserver(SSL) started at https://0.0.0.0:8443
```

実行するとブラウザが立ち上がり、<br>
`e2d3-contrib`内のチャート一覧が表示される。

表示されない場合は、<br>
http://localhost:8000/ にアクセス。

---

## 初めてのテンプレート作成

`e2d3`コマンドを実行した状態で、

1. `e2d3-contrib`ディレクトリ内で<br>
   `barchart-javascript`をコピー
  - この時点でブラウザ内のチャート一覧にコピーした名前で表示される
2. コピーしたチャート内のmain.jsファイルを変更して<br>保存する
3. ブラウザが自動的に更新され変更されたチャートが<br>表示される

---

## E2D3テンプレートの構成要素

<dl>
  <dt>main.js</dt>
  <dd>データ可視化で初めに実行されるファイル</dd>
</dl>
<dl>
  <dt>main.css</dt>
  <dd>main.jsの実行前に読み出されるCSSファイル</dd>
</dl>
<dl>
  <dt>thumbnail.png</dt>
  <dd>一覧画面に表示されるサムネイル画像(廃止予定)</dd>
</dl>
<dl>
  <dt>README.md</dt>
  <dd>一覧画面で表示されるチャートの説明</dd>
</dl>

---

## main.jsの基本構造

```js
//# require=d3

初期化ブロック:
  ここの位置で初期化を行う

function update(data) {

  データ更新ブロック:
    Excelでデータが更新される毎にdataにセルのデータが入って呼び出される

}
```

---

## 暗黙の変数&関数

<dl>
  <dt>root</dt>
  <dd>
    チャートを描画するべき先のHTML要素<br>
    d3やjQueryで`body`要素をselectしたりせずこれを使う
  </dd>
</dl>
<dl>
  <dt>function reload()</dt>
  <dd>
    topojson使用時等、非同期にデータ以外のファイルを読んだ後、`update()`を再度呼び出したいときに呼び出す
  </dd>
</dl>

---

## function reload()の使い方

`japanmap-javascript`を参照

```
初期化処理

d3.json('japanmap.json', function (error, json) {
  topojsonを使ってjsonとして読み込んだ地図を描画

  reload(); // update()をデータと共に明示的に呼び出す
});

function update(data) {
  一番初めに呼び出された時には、上記japanmap.jsonの読み込みが
  終わっていない場合があるのに注意
```

---

## チャートを作る際に気をつけて<br>欲しいこと

- 最終的にはWindowsのExcel2013で確認して欲しい
- 幅や高さを固定で作らない
  - 可能な限り`root.{clientWidth, clientHeight}`を元に計算する
- `update()`でデータが更新された際アニメーションすると嬉しい

---

## 今後の開発者向け拡張予定

- gist対応
  - `e2d3-contrib`のcloneを不要にする
  - (本当はこの説明会までに間に合わせたかった)
- AMD形式でない外部JSの読み込み方法の強化
