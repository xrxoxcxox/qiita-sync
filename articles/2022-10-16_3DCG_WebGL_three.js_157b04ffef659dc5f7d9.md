<!--
title:   three.jsでデバッグのために入れるもの色々
tags:    3DCG,WebGL,three.js
id:      157b04ffef659dc5f7d9
private: false
-->
## この記事の概要

three.jsを触っていると、動かないコードを書いているのか、位置やサイズを間違えて画面内に描画できていないだけなのか、分からなくなることがあります。[^unskilled]
デバッグがてら色々なものを追加するのですが、その観点でまとまった資料を見ないので、記事にしました。

[^unskilled]: 単純なスキル不足な気もしますが、少なくとも私はよく迷います。

## Helpers

いかにもデバッグなどで使いそうな名前です。
ここで紹介していないものも、公式ドキュメント内で`helper`と調べると出てきます。

https://threejs.org/docs/?q=helper

### AxesHelper

キャンバス内にXYZ軸を表示します。
X軸が赤、Y軸が緑、Z軸が青です。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b8e920d7-051b-2077-f3a2-4c1a88d9ae64.png)

```javascript:使用例
const size = 5;
const axesHelper = new THREE.AxesHelper( size );
scene.add( axesHelper );
```

https://threejs.org/docs/api/en/helpers/AxesHelper.html

### GridHelper

XZ平面にグリッドを描画します。
グリッドを描画するエリアのサイズと、その分割数を指定できます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ec9d1375-f816-b14e-32e6-f67891d7ec37.png)

```javascript:使用例
const size = 10;
const divisions = 10;
const gridHelper = new THREE.GridHelper( size, divisions );
scene.add( gridHelper );
```

大抵上記で事足りますが、グリッド線の色も変えられます。
第3引数で中央の線、第4引数でそれ以外の線の色を指定します。

https://threejs.org/docs/api/en/helpers/GridHelper.html

### DirectionalLightHelper

ライトの位置と方向を可視化します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/113e32b9-ee92-b43d-a06c-e3edf471059f.png)

```javascript:使用例
const light = new THREE.DirectionalLight( 0xFFFFFF );
const helper = new THREE.DirectionalLightHelper( light, 5 );
scene.add( helper );
```

大抵上記で事足りますが、第3引数に色を渡せば、方向を示す線の色も変えられます。

https://threejs.org/docs/api/en/helpers/DirectionalLightHelper.html

私がDirectionalLightをよく使うのでDirectionalLightHelperを紹介していますが、PointLightHelperやSpotLightHelperもあります。

https://threejs.org/docs/api/en/helpers/PointLightHelper.html

https://threejs.org/docs/api/en/helpers/SpotLightHelper.html

## Stats

three.jsを使うならかなりの確率でアニメーションをさせると思います。
その際、FPSを監視して表示します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4a9f3a5f-c633-2d1c-7611-df55e3d99906.png)

アニメーションを描画する関数の中で呼び出せば監視されます。

```javascript:使用例
import * as THREE from "three";
import Stats from 'three/examples/jsm/libs/stats.module'

// 中略：rendererやscene、cameraなどの宣言

const stats = Stats();
document.body.appendChild(stats.dom);

function animate() {
  requestAnimationFrame(animate);
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  stats.update();
  renderer.render(scene, camera);
}

animate();
```

three.jsをdependenciesに追加するだけで使えるモジュールですが、three.js専用というわけではありません。
元々のリポジトリはこちらです。

https://github.com/mrdoob/stats.js

## OrbitControls

移動、回転、拡大縮小とカメラを操作できるようにします。
例えば配置した要素が大きすぎて画面外に描画された場合でも、OrbitControlsを有効にしておけば探しやすいです。

デバッグ目的のためのものではないと思うのですが、なんだかんだ確認のために使いがちです。

```javascript:使用例
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

// 中略：rendererやscene、cameraなどの宣言

const controls = new OrbitControls( camera, renderer.domElement );
controls.enableDamping = true;

//controls.update()はカメラのポジションなどを変更した後に呼び出さないといけない
camera.position.set( 0, 20, 100 );
controls.update();

function animate() {
  requestAnimationFrame( animate );
  // controls.enableDampingかcontrols.autoRotateがtrueに設定されている場合は必要。
  controls.update();
  renderer.render( scene, camera );
}
```

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox