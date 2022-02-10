<!--
title:   Webで使うモーショングラフィックスをAfter Effectsで作るための最初の一歩
tags:    AfterEffects,Design,アニメーション,デザイン,モーショングラフィックス
id:      887de8b56d09b7e7db3c
private: false
-->
## これは何

- After Effectsを用いたモーショングラフィックスの作り方のうち、基本的な考え方を説明した記事です
- Webやアプリに実装する際の使い方や考え方を重視しています

## 起動→新規コンポジション作成

とにもかくにも、まずはAfter Effectsを起動します。
若干レイアウトは違うかもしれませんが、こういう画面が出てくると思います。

画面の中心あたりにある「新規コンポジション」を選択します。

![起動直後のスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/eb3584b1-edea-0fe3-9550-f635f689570c.png)

するとコンポジション設定というウィンドウが出現します。
設定する箇所がたくさんありそうに見えますが

- 幅
- 高さ
- デュレーション（時間）

この3つだけ設定すればだいたい大丈夫です。
また、後から変えることもできるので、何も触らず次に進んでしまっても構いません。

![コンポジション設定のウィンドウ](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/401027e6-0741-bed6-2668-77aaf9048574.png)


### コンポジションとは？

少し表現は違いますが、ソフトウェア開発でいうコンポーネントのようなものだと思ってください。
扱いやすい単位でコンポジションを作り、それらを組み合わせて1つのモーショングラフィックスを作ります。

小さいコンポジションならレイヤー1枚だけも有り得ますし、大きめのコンポジションだと複数枚のレイヤーや色々なエフェクトを組み合わせて構成します。

また、はじめからコンポジションとして作ることも、ある程度バラバラに作ったあとにコンポジション化することもできます。[^1]

[^1]: 後からコンポジション化することをプリコンポーズすると言います。

## After Effectsにおける「位置」の理解

※これは「細かいことは良いから早く動かしてみたい」という方は読み飛ばしても大丈夫なセクションです。

はじめてのコンポジションを作ったことですし早速要素を配置して動かしたいのですが、先に設定を変えたり概念を理解しておいた方が良い箇所があります。

### 環境設定

まず、`After Effects > 環境設定 > 一般設定`から`アンカーポイントを新しいシェイプレイヤーの中央に配置`にチェックを入れます。

![「アンカーポイントを新しいシェイプレイヤーの中央に配置」にチェックを入れた画面](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/df3306f6-5757-d59a-ddbf-738f7729fa18.png)

ここにチェックを入れていないと、要素を配置した際にアンカーポイントが「シェイプの中心」ではなく「コンポジションの中心」に配置されてしまいます。[^2]

[^2]: なお、この設定をオンにしてもテキストレイヤーを配置した際はアンカーポイントがオブジェクトの中心に来てくれないので上手いこと頑張る必要があります。

初期設定では、CSSでいう`transform-origin`が`center`でも`top left`でもなく、全然関係ない箇所に設定されてしまうようなイメージです。
座標を計算してアニメーションを作るとして、非常にやりづらいのでチェックを入れるのを強く推奨します。

### シェイプの配置の仕方

PhotoshopやIllustratorと同様に、ドラッグして図形を描画できるのですが、Webなどに慣れた人だったら以下の方法をオススメします。

| スクリーンショット | 説明 |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/21a61524-71b3-2341-a3bc-1a45b435cb60.png) | 上部にあるツールバーから図形ツールを長押しして、好きな図形を選ぶ |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1fdb4328-aa7d-8c48-482f-b6e9109e7fcc.png) | 選択できた状態（スクリーンショットでは長方形を選択）でマークをダブルクリック |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/26083979-6047-7b1b-a142-a3e0846b64ea.png) | コンポジションの幅と高さいっぱいに長方形が描画される |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b55fd7b5-9819-eda3-be57-4798c5a5621b.png) | レイヤーから、`シェイプレイヤー > コンテンツ > 長方形 > 長方形パス > サイズ`を開き、値を調整（例では1280x720） |

なぜこのような面倒な工程を踏むかというと……。

まず、普通にドラッグして図形を作成してみます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b8e85c13-5863-b62e-48fa-54828ae72465.png)

その後、サイズを調整して中心に整列します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9040de53-7853-41ad-2243-9426f0af0cd8.png)

レイヤーに注目してください。
アンカーポイントが`-459.6, -9.5`というなんとも中途半端な値ですね。

問題ないと言えばないのですが、座標を計算して「正しく」動かしたいときは厄介になってしまうタイミングもあります。
あとは、シェイプが持っている値が中途半端なのが気持ち悪いので、自分はこのように細かく設定しています笑

## キーフレームについて

### キーフレームとは

相当簡単にまとめてしまいますが、どんなアニメーションを作るにしても以下の考えが基本です。

- 任意のプロパティの値の幅を決める
    - X座標を`0`から`960`まで
    - 角度を`45°`から`90°`まで、など
- 時間の幅を決める
    - フレームが`10`から`30`まで、など
- 決めた時間の最初から最後にかけて、プロパティを変化させる

これらの情報を「キーフレーム」というものに記録して、変化を作っていきます。

### キーフレームの打ち方

「このタイミングで動かしたい」というフレームに時間インジケーター（右側に囲った青いツマミみたいなもの）をあわせて、変更したいプロパティのストップウォッチマークを押します。

![時間インジケーターとストップウォッチを示したスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/2c20047a-1d18-1eef-9bec-3dc7b18f2a23.png)

その後、画面上のオブジェクトを動かすか、タイムラインパネルの数字を直接触ります。
するとキーフレーム間を補間するようにアニメーションします。

### 具体的な例

下の例は、`位置`というプロパティにキーフレームをいくつか打っていて、25フレーム目と30フレーム目をピックアップして説明します。

- 25フレーム目では`位置`が`1440, 540`
- 30フレーム目では`位置`が`960, 540`

自然言語的に翻訳するなら「25フレーム目から5フレームかけて、X座標を1440から960に移動させる」という指示を出している状態です。

|  | スクリーンショット | 補足 |
| --- | --- | --- |
| 25フレーム目 | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5d5ad066-90ca-2c32-7def-c9de0d466dd6.png) | `位置`が`1440, 540` |
| 30フレーム目 | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/8066ca31-bb0d-c55f-689b-4e3e20aa44de.png) | `位置`が`960, 540` |

もちろん「5フレームかけてX座標と角度と透明度を変化させる」といった具合で、複数のプロパティを同時に変化させることも可能です。

## 主要なプロパティ

After Effectsには非常にたくさんの機能があるがゆえに、どこから手を出して良いか分からなくなりがちです。
まずは一番基礎となる4つのプロパティに絞って

- 位置
- スケール
- 回転
- 不透明度

これらだけを触ってみるのが良いでしょう。

### 位置

その名の通り位置を動かします。
基本はXY座標で、Z座標（奥行き）をもたせることもできます。
Web用のモーショングラフィックスを作るなら大抵はXY座標で済むのではないでしょうか。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5e146dc1-2b7c-07ba-9adc-36feaf77f496.gif)


### スケール

大きさを変えます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/405c6361-4175-050f-7525-b9ab36c68a47.gif)


### 回転

gifにした関係で分かりづらくなってしまいましたが、回転しています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bf7fd60b-6b42-f783-476c-3687c513d90e.gif)


### 不透明度

光の点滅っぽいですが、不透明度が変わっています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c9d00e3f-b20f-50fe-0ca0-f03711dc4924.gif)


### 4つとも組み合わせると……

上記4つのプロパティを同時に変更させるとこのようになります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/feb689e0-d7ee-c006-6660-189f045a68b3.gif)


## イージング

今のままでも動いているは動いていますが、若干違和感があります。
変化の仕方が一定なので物理法則に沿っていないように見えるんですね。

そこで動きに緩急をつけるんですが、それをイージングと呼びます。

イージングをつけたいキーフレームを

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ed2f8e97-5d99-75c5-5097-e2313bfbfa0f.png)

ドラッグして選択して

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/2280fade-157c-71f0-51f6-4826beebd0f7.png)

`F9`を押すと形状が変わります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7c9d479f-4e70-e343-a1b1-90bd7f0e30ad.png)

先ほど作ったものと、`F9`でイージングを適用したものを並べるとこのようになります。
gifにしている関係で分かりづらいですが、少しスムーズな動きになっているのが分かるでしょう

| | イージングなし | イージングあり |
| --- | --- | --- |
| 位置 | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/71ed1acc-1fea-6671-f922-9c5fdabe4c9d.gif) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/19b8808e-855a-bd7d-aeda-c338ddf4a50c.gif) |
| スケール | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/dcf1bc77-a96e-d664-7c56-a8472b4cee95.gif) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/87ce6ab1-ee12-0e3a-86a9-7d27e8cd8578.gif) |
| 回転 | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/35660287-1aa3-a289-2e3e-a5a82e55fe72.gif) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1c53ab9f-8d56-c54a-7043-8a6119becee1.gif) |
| 不透明度 | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/62eab5d3-f3b9-b6da-d4fe-4a93f8df801b.gif) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a1fa7fbf-aff8-0ecd-b093-2beb8679aae9.gif) |
| 4つ全部 | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f33ce95b-e265-833d-5584-4786f56278d9.gif) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bfbdbb0c-7be9-f563-35b3-7427d6b563f9.gif) |

イージングにも色々な種類がありますが、細かいことはおいておいてこの`F9`が無難な変化をしてくれます。
もう少し突っ込んで気になる場合は

- イーズイン
- イーズアウト
- イーズインアウト

といった違いを調べてみると面白いと思います。

### 余談、Material Designのイージング

Webやアプリを作るなら多かれ少なかれMaterial Designを参考にしていると思いますが、After Effectsでの再現方法も公式が書いてくれています。
（正確にいうとアーカイブされたドキュメントで、最新版ではないのですが……）

https://material.io/archive/guidelines/motion/duration-easing.html

例えば`Standard curve`では`After Effects: Outgoing Velocity: 40% Incoming Velocity: 80%`と紹介されています。

これを再現するには、まずキーフレームを選び

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/13ed2839-721e-5c77-4215-ccfdad71151e.png)

`command + shift + K`を押すとこのような画面が出てきます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/21409bbd-40a3-9662-15ad-ba649512ffd5.png)

- 入る速度 = Incoming Velocity
- 出る速度 = Outgoing Velocity

なので、それぞれを80%と40%にするとMaterial Designの動きを再現できます。

`Deceleration curve`、`Acceleration curve`、`Sharp curve`を再現したいときも、同じ要領で数字を変えればOKです。

## 書き出し

今回は具体的な動画の作り方のテクニックは紹介していませんが、書き出しの仕方を紹介します。

`ファイル > 書き出し > Adobe Media Enocder キューに追加...`を選択して

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a4c2bc40-8a31-e8af-6f78-5b25d9a43c04.png)

Adobe Media Encoderが起動するので、右上の再生ボタンを押します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/808b4209-ee34-5bd8-a480-5e0e1d212c56.png)

すると動画が保存されます。めでたしめでたし。

## まとめ

- コンポジションを作成する
    - たくさん作れるけど、まずは1つでOK
- キーフレーム
    - 変化させるタイミングにキーフレームを打って、プロパティを変化させていく
- まずはじめに使うプロパティ
    - 位置
    - スケール
    - 回転
    - 不透明度
- イージング
    - 自然な変化を演出する
- 書き出し
    - 書き出し用ソフトを起動して動画ファイルとして書き出す