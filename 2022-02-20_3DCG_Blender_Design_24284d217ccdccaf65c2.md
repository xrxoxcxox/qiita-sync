<!--
title:   [LTスライド&原稿]簡単にできる範囲でも十分！3DCGでグラフィックの表現力が上がる！
tags:    3DCG,Blender,Design,デザイン
id:      24284d217ccdccaf65c2
private: false
-->
![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0ed1139e-e4bc-f985-308a-3efeadb4becc.png)

## この記事の概要

2022 年 2 月 20 日開催の「ゆるデザトーク」という LT イベントでの発表内容です。

## 内容

### 自己紹介

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/953f3fe2-0c96-29ba-56ff-394ac76cd59e.png)

みなさんこんにちは。

Qiita 株式会社でデザイナーをしている綿貫佳祐です。

とてもざっくりした経歴はこちら。
現在は Qiita の中でも CX 向上グループという「売上とか利益は置いといて、ユーザーの皆さんが嬉しいとか便利とか、そう思えるものだけを作る」部署でマネージャーをしています。

しかし今日は、普段の仕事とは全く関係のない「簡単な 3DCG でも表現のリッチさが上がる」という内容で発表します！

### 3DCG をやったことある人いますか？

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/6e161914-a1d2-dc03-6f31-bca02e074e5d.png)

みなさん 3DCG やったことありますか？僕は最近 Blender を触るようになりました。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e0d2eb98-77ea-59de-a083-0d6d2228597b.png)

週末にちょろっとだけ触る程度で大した知識も実力もないんですが、それでも「昨今のデジタルプロダクトデザインに活きそう」な手応えを感じています。

具体的なテクニックの紹介というよりは「こういうことが便利だよ、楽だよ」といった紹介を 3 つしようと思います。

「楽しそう！自分もやってみたいかも！」と思ってくれる人がこの場から生まれたら、今日の目標は達成です笑

### Photoshop では面倒くさい表現も 3DCG なら余裕な可能性アリ

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/103bcaec-f71f-598b-7d04-1def2c2ca201.png)

まず 1 つ目は「Photoshop では面倒くさい表現も 3DCG なら余裕……かもしれない」です。

例えばこんな感じ

| はためく布っぽい表現 + ホログラフィック風 | 液体表現 |
| --- | --------------------------------------------- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/504ac4e7-d4f3-6f35-f5f2-6dd83cd0598a.jpeg) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/254acfe1-6d44-2edf-e6bd-6d8c25a19725.png) |

どちらも多分、Photoshop で 1 から作ろうと思うと普通に面倒くさいと思います。

これらは、落下シミュレーションや流体シミュレーションなどを使えば割と楽にできてしまうんですね。

多少時間はかかりますけど、パラメーターをいじったら後は待っていれば Blender が勝手にやってくれます。

良い感じのタイミングを見計らって撮影するだけ、みたいなイメージです。

| 布にボールを落として、良い形状になったタイミングで撮影している |
| --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bdb2d737-c9b0-f237-bb70-961ee110296a.png) |
| 下半分の画面はホログラフィックっぽい質感を作るための設定 |

ソフトによって多少差はあると思いますが、多分最近の 3DCG ソフトならどれも似たような機能がある……はず。

### 撮影スタジオがPCの中にある感じ

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/57d261f7-d77a-7180-9e3f-79f48a7c3b62.png)

何かを撮影しようと思ったら、用意するものは結構あります。

例えばブツ撮りなら……。

![撮影するもの、カメラ（適切なレンズ）、背景（布や床）、照明（ストロボやレフ板）、小物](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/3041c723-3c01-c35e-6c70-2238d5eb44c1.png)

ちょっとしたものを取ろうと思ってもまあまあ準備が大変ですよね。

欲しい写真のイメージにあわせてレンズを買い足すなんてことになったら、相当お金がかかりますし笑

けど 3DCG であればこういったものをパソコンの中だけで準備ができます。

| レンダリング | 撮影風景 |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/38e23cdd-be23-dcf6-cfa8-1f3e33bbffd2.jpeg) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c152ecc5-218f-e748-4fde-a4cfa8fda318.png) |

| レンダリング | 撮影風景 |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0680cb15-234e-e757-7dfd-d5c01fd7d151.jpeg) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bfcbd99d-2004-2178-e6fb-638b047a8246.png) |

ライトの数、ボケの表現、小物類、全部 Blender 内で用意しています。

### 同じような表現でも 2D と比べて情報量がアップ

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/06ce18cf-6dd4-c13c-0c96-d3c04bda310f.png)

例えば幾何学模様ってよく使いますよね？

けど、単に幾何学図形を並べただけだとこざっぱりしていて、もう少し情報量を入れたいな……と思うシーンもあるんじゃないでしょうか？

というわけで簡単な幾何学模様を 3D で作ってみました。

| レンダリング + 文字入れ | Blender 上 |
| --- | -------------------------------------------------------- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d827db68-2ec5-39f8-da48-7991c91661af.jpeg) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ee2a8611-d7ce-69f9-8a96-2cc006710cc0.png) |

- 陰（shade）も影（shadow）もできて情報量アップ
- 光源の色で情報量アップ
- 照り返しやアンビエントオクルージョンで情報量アップ
  - アンビエントオクルージョンは日本語で何て言うか分からなかった……

今回は実施していませんが、質感の違いも結構容易に出せるのでそこでもさらに情報量アップが可能です。

### まとめ

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/00d9f2a5-0c69-c2ae-f4c6-25f17ab77f96.png)

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4d81f3ce-32a0-7505-66fa-16041cc879e7.png)

良くないですか？

今回紹介した内容は、人によっては 2~3 日で作れると思います。

いつもと違う手法を取り入れてみると、それだけでも発想が広がると思っています。

Blender は無料で使える太っ腹過ぎるソフトなので、試しに 1 度トライしてみませんか？

以上で僕の発表は終わります。ご清聴ありがとうございました！