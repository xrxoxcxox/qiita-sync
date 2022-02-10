<!--
title:   Live2DとVTube StudioでMacユーザーもアバターZoom
tags:    Live2D,Qiitan,VTubeStudio,zoom
id:      c31221abc1e0d38858d4
private: false
-->
## これは何

- MacユーザーでもZoom上でアバターを被って会議をする方法
    - FaceRig（あるいは次世代のAnimaze）はWindows専用なのでMacユーザーは少しツラい

## 用意するもの

若干タイトルと重複しますが

- [Live2D](https://www.live2d.com/)
    - アバター（Live2Dのモデル）を準備するために使います
    - 自分で作らない人は公式マーケットの[nizima](https://nizima.com/)からモデルを購入しましょう
    - ちなみに今回の記事ではFREE版で出来る範囲です
- [VTube Studio（モバイル用版）](https://apps.apple.com/us/app/vtube-studio/id1511435444)
    - アバターのフェイストラッキングをするために使います
    - デスクトップ版のVTube Studioと接続します（有料）
- [VTube Studio（デスクトップ版）](https://denchisoft.github.io/)
    - Macの画面上にアバターを映し出すために使います
- [CamTwist](http://camtwiststudio.com/)
    - デスクトップ版のVTube StudioをCamwistで取り込むことでZoomにアバターを投影できます

## アバターの準備

- 詳細なモデルの作り方はこの記事では省略します
- [公式の動画チュートリアル](https://www.youtube.com/playlist?list=PLqbLt-S6_fh6U_Y7a8CyaA05GqvcJmNln)が充実しているので、こちらを見ながら制作するのをオススメします
- 今回作成したのはこちら、Qiita公式マスコットのQiitan
    - ![qiitan-live2d.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f2f22fed-00d5-362b-d213-195a538209e8.png)
- Live2Dエディタ上で、ランダムにパラメーターを動かして動作チェックが出来ます
    - ![qiitan-movie.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/700913d7-316c-3e76-3aab-5fcb77c4dcbf.gif)
- モデルが完成したら`ファイル→組み込み用ファイル書き出し→moc3ファイル書き出し`で書き出します


## VTube Studioの準備

- モバイル版、デスクトップ版ともにインストールします
- モバイルから設定画面へ入りPRO版を購入します
    - <img width="375" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7120e350-6bfa-27f4-73e1-86d2cfb48840.png" alt="" />
    - <img width="375" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/6708143a-f771-7f7c-0273-ea5924ab872e.jpeg" alt="" />
    - 自分は既に購入してしまっているので画面が違いますが……
    - 価格は2,820円（2020年12月現在）でした
- デスクトップ版から設定画面に移って……
    - ![desktop-top.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/3233db8e-3233-b8bf-7046-ec078052661d.png)
- サーバーを起動後、「IPを示す」ボタンを押して出てきた値をメモします
    - ![desktop-setting.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d49fa3af-f44d-ab04-3ac6-90529f6494b2.png)
- モバイルに戻り、設定画面を開きます
    - <img width="375" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7120e350-6bfa-27f4-73e1-86d2cfb48840.png" alt="" />
- PCと接続とストリーミングモードをONにします
    - <img width="375" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e5f6eaf7-66b8-adc9-8288-ffd44060c0d5.png" alt="" />
    - 成功すればステータスランプが緑になります
- 「アバターの準備」で書き出したフォルダをMacの`VTubeStudio.apwo/Contents/Resources/Data/StreamingAssets/Live2DModels`内に保存します
- 画像の場所から自分のモデルを選びます（今回はアイコンを作っていないので「No Icon」になっています）
    - ![desktop-import.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/3fde346d-b856-0353-3bb9-f1a5884e51fe.png)
    - ![desktop-select-model.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/16e6daa8-0d8b-7e58-18d8-b0d067a48037.png)
- モバイルとデスクトップ間でVTube Studioが正しく接続できていればモデルが動きます
    - ![qiitan-movie-2.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7d008875-edb5-104a-a314-919691a863ea.gif)

## CamTwistの準備

- CamTwistを起動して以下を選択する
    - Video Sources → Desktop+
    - Settings → Confine to Application Window → VTube Studio
- ![CamTwist](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/377f86d0-06ab-1483-d13a-2bd6a58661df.png)

## Zoomの設定

- ビデオ設定→カメラ→CamTwistを選択
    - ![スクリーンショット 2020-12-30 18.44.42.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7005171a-12c1-69bc-39b2-2284af76d23b.png)
- Zoom上でアバターが動きます
    - ![qiitan-movie-3.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/117981fa-f2b1-aa99-dec7-51201adbbac8.gif)