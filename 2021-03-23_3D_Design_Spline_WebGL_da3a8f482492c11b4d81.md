<!--
title:   Splineを使ったらWebに3Dグラフィックを取り入れるのが簡単だった
tags:    3D,Design,Spline,WebGL,デザイン
id:      da3a8f482492c11b4d81
private: false
-->
## これは何

[Spline](https://spline.design/)という3D用のデザインツールを使ってみたら良い感じだったので書いた記事です。
ツールの紹介もしつつ、できるだけ簡単に、Webサイトに3Dグラフィックを取り入れるまでの流れを書いていきます。

## 完成品

![マウスで動かしているシーン](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1e5ddc68-2e66-1fc7-f24f-e57931c12885.gif)

[サンプルサイトをこちらからご覧いただけます。](https://xrxoxcxox.github.io/qiita-spline-sample/)

## Splineを触る前に

ここ数年デザイントレンドとして挙がる3Dグラフィックですが、正直ハードル高くないですか？
Webで3Dを扱いたいとなると[Three.js](https://threejs.org/)をよく見かけると思いますが、陰影のついていない立方体をクルクル回すだけでもこのコード量です。

![陰影のない立方体が回っている動画](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/71d66b2a-c261-5e45-8752-cc19cdf94a6c.gif)

```javascript:公式ドキュメントにあるコード
var scene = new THREE.Scene();
var camera = new THREE.PerspectiveCamera( 75, window.innerWidth/window.innerHeight, 0.1, 1000 );

var renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );

var geometry = new THREE.BoxGeometry( 1, 1, 1 );
var material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
var cube = new THREE.Mesh( geometry, material );
scene.add( cube );

camera.position.z = 5;

var animate = function () {
	requestAnimationFrame( animate );

	cube.rotation.x += 0.01;
	cube.rotation.y += 0.01;

	renderer.render( scene, camera );
};

animate();
```

それに対してSplineでは、最も簡単なやり方なら**ソフト上で出力されたiframeを埋め込むだけ**で3Dグラフィックを導入できます。
カスタム性やパフォーマンスを追求できるものでは無いと思いますが、パパッと良い感じのものができてとても楽しかったので是非紹介してみようと思った次第です。

## Splineの導入からWebに3Dグラフィックを取り入れるまで

### インストール

[公式サイト](https://spline.design/)にアクセスして少し下に行くと`Try the Alpha Release`と書かれています。
そう、まだアルファ版のソフトです。

使用しているマシンにあわせてインストールを進めてください。

![ソフトをダウンロードできるセクションのスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/57b4fb6a-9abb-4524-5189-c6cfd99223dd.png)

### 起動

インストール終了後、起動したら右上にある`+ New File`から新規データを作成します。

![Splineの起動画面](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/082b8ef3-362d-e20a-b3b3-9b67d55ff8fd.png)

### データの作成

画面が開けたら**ひとまず好きに色々なオブジェクトを配置してみましょう**。
上部にあるツールバーから立方体や球、円柱、ピラミッドなど様々な形状のものを呼び出せます。

![ツールバーの位置を示したスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/69196120-b525-ce54-c3d9-a10f2ff4ff70.png)

私はBlenderは多少触ったことがありますが、簡単に見える形状でも作るのは思ったより手間がかかるんですよね。

対してSplineの場合は幾何図形なら割と簡単に作ることができます。
雰囲気だけでオブジェクトを配置しても割とそれらしいものが出来上がるので適当に触るだけでも楽しいですよ。

今回は「できるだけ簡単に3DグラフィックをWebに取り入れる」がテーマなので、細かなツールの操作についてはスキップします。
[公式のドキュメントに操作方法などがまとまっている](https://docs.spline.design/)ので興味のある方はこちらをご覧ください。

### エクスポート

データが完成したら、`Publish & Share`のパネルからエクスポートしましょう。

![エクスポートの設定のUIのスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e7f2d64c-1cf1-276b-0f7e-105d6912e9f0.png)

個人的には以下の設定をおすすめします。

| プロパティ | 値 | 補足 |
| --- | --- | --- |
| Type | Public URL | この設定で出てくるiframeを埋め込むのが多分1番簡単[^1] |
| Detail | Default | HighとかLowとか試したけど、つるんとした幾何形態ならあんまり差が分からなかった |
| Rotate | Yes | ドラッグしてグルグル回せるようになる、楽しい |
| Pan | Yes | これは正直どっちでも良い |
| Zoom | No | Yesにしているとスクロールとバッティングしそう（特にトラックパッド） |
| Logo | Show | 無料版ではHideは選べない |

[^1]: Web Contentの方が色々できるポテンシャルはありつつ、若干ハードルが高そうな気がしたので今回はiframeを使うやり方を紹介しています。

そしてExportボタンを押すと、このようなダイアログが出るのでコピーしておきます。

![Export成功時のダイアログ](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5be92703-0eec-0094-b61e-db1c024b8fbf.png)

### HTMLに埋め込む

ここまで来たら後はもう簡単。
先ほどコピーしたiframeを任意のHTMLに埋め込むだけです。

HTMLとCSSをちょいちょいと整えたら完成です。

![マウスで動かしているシーン](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1e5ddc68-2e66-1fc7-f24f-e57931c12885.gif)

（最初に貼ったものと同じGIFです）

## 現状感じている良い点、イマイチな点

### 良い点

- ささっと作れる
    - 全てをコードで書くよりは遙かにグラフィックを作りやすい
    - 元から使える図形の数が豊富なので、組み合わせるだけでも絵になりやすい
- インターフェースが割ととっつきやすい
    - できることが限られている分、3Dをやったことがない人でもなんとなく触れる気がする
    - 今回は紹介していないけどアニメーション機能もあり、それも割と簡単
- エクスポートしたらすぐにWebに取り込める
    - iframeにせよjsファイルでの書き出しにせよ、かなり早く「動く」状態を作れる

### イマイチな点

- レンダリングの品質は高くない
    - 簡単にできるように作られているから、の裏返し
- 3Dデータとしての書き出しはできない
    - Web用に特化しているので、objなどの書き出しはできない
- なんか挙動がおかしい
    - アルファ版だから？
    - `command + Z`で全く予期しない形状の変更がおこったり、カメラがぶっ飛んで謎の空間をさまよったりした

ちなみにレンダリングの質は、SplineとBlenderで似たようなシーンを作って比較したので分かりやすいと思います。
素材の質に差をつけるとか、映り込みを表現するとか、陰影全般の自然さとか……。

| Spline | Blender |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/87ccdee6-5403-0b1d-dabf-519528e20017.jpeg) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b5101650-19a7-a0d0-e11b-bc6910d062db.jpeg) |

とはいえ「3Dが扱える」というだけであって、この両者を比較するのはナンセンスかもしれません。
Webデザインができるからといって、FigmaとPhotoshopを比較してもそもそもの土俵が違うように、Splineの目指しているのはリッチな描画ではないような気がします。

## まとめ

途中若干脱線してしまいましたが

1. GUIで簡単に3Dグラフィックを作れる
2. 作ったグラフィックをボタン1つでWeb仕様にエクスポートできる
3. 埋め込めば完成

簡単にWebに3Dグラフィックを取り入れられるSplineでした。