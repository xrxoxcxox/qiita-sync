<!--
title:   three.jsでBlenderのカメラを使う
tags:    3DCG,AdventCalendar2022,Blender,three.js
id:      cc7662fcf51182cc161c
private: true
-->
## この記事の概要

Blenderで作ったシーンをglTFに書き出してthree.jsから呼び出した際、そのままではカメラが適用されません。
別途three.js内でカメラを作成しても良いですが、目視で合わせるのも大変なため、Blenderで定義したカメラをそのまま使う方法を記事にしました。

## Blenderからの書き出し設定

まずはじめに、書き出し時の設定が必要です。

`File > Export > glTF 2.0 (.glb/.gltf)`を選択します。
このとき開いたウィンドウの右側にある`Include`を開きます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1b969624-3b3f-047a-e367-142f68bf6ab1.png)

すると色々な項目が出現します。
今回はカメラのデータを含めたいので`Data > Cameras`にチェックを入れます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/cf9e929e-1e6a-45ff-ab6b-6be1cf316e8f.png)

これで設定は完了です。
あとはウィンドウ右下の`Export glTF 2.0`を押して保存します。

## three.jsでの読み込み

先に完成形のイメージをお見せします。

```javascript
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"; // 1

const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(window.devicePixelRatio);

const renderArea = document.getElementById("render");
renderArea.appendChild(renderer.domElement);

const scene = new THREE.Scene();

let camera; // 2
const loader = new GLTFLoader();
loader.load("path/to/file.glb", (gltf) => {
  scene.add(gltf.scene);
  camera = gltf.cameras[0]; // 3
  animate();
});

const animate = () => {
  // code for some kind of animation
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
};
```

上記のコードの説明です。

1. まずは`GLTFLoader`をimport
1. 空の`camera`を作成
1. `gltf.cameras[0]`（Blender内で作成したカメラのうち、1つ目）を、既に定義した`camera`に代入
    1. もちろん複数のカメラがあれば`cameras[1]`や`cameras[2]`で取得可能

three.jsを使うのであれば、ロードしたモデルをアニメーションさせる場合がほとんどだと思います。
その際に`loader.load`の内側だけで`camera`を定義してしまっては、アニメーション用の関数に`camera`が渡せなくなってしまいます。
そのため、グローバルにカメラを用意し、後で書き換えるように実装しました。

## 最後に

改めて整理すると非常に簡単な内容なのですが、リアルタイムでは結構困惑しました。
自分用の備忘録含めて、誰かのお役に立てれば幸いです。

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox

Devトークでのお話してくださる方も募集中です！

https://jobs.qiita.com/employers/qiita-inc/dev_talks/489