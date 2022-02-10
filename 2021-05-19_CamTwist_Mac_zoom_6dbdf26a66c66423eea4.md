<!--
title:   Zoomの画面共有で動画がカクつく→CamTwistで解消できます
tags:    CamTwist,Mac,zoom
id:      6dbdf26a66c66423eea4
private: false
-->
## これは何

- Zoomで画面共有をすると動画がカクつく問題を（ある程度）解消するための記事です
    - 単なるブラウザ操作を共有するだけなら良いんですが、動画とかアニメーションつきスライドとかを流すとコマ落ちが目立ちます
        - 画面のアニメーションを実装して、パッとフィードバックが欲しいときなども、画面共有だとあまりよろしくなさそうです
    - なお、この記事の内容を実践しても全てが解消できるわけではありません
        - ネットワークの速度なども関係します
- なお、CamTwistはMacでしか使えません :pray:

## 概要

1. [CamTwist](http://camtwiststudio.com/)をインストールする
1. 共有したい画面をキャプチャする
1. Zoomの`カメラを選択`から`CamTwist`を選ぶ
1. （必要に応じて）スポットライトビデオをオンにする

## CamTwistをインストールする

まずは以下のサイトにアクセスします。

http://camtwiststudio.com/

`Donwload Now`のボタンからdmgファイルをダウンロードし、ウィザードに沿ってインストールします。

## 共有したい画面をキャプチャする

インストール完了後CamTwistを起動したら以下のようなウィンドウが出ます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/760d4620-53bc-9887-f8b1-189ff82dfab3.png)

この記事の話では、触るのは以下の2つだけです

- 1番左側にある`Step 1: Select a video source`
- 1番右にある`Settings`

### Step 1: Select a video source

`Desktop+`を選択して`Select`を押します。
以上です。

### Settings

1. `Full Screen`のチェックを外す
1. `Confine to Application Window`にチェックを入れる
    1. 一応分かりやすさをアップさせるために`Filter out untitled windows`にチェックを入れておくと良いとと思います
1. `Select from existing windows`にチェックを入れる
    1. チェック下のselectから、共有したいウィンドウを選択します

## Zoomの`カメラを選択`から`CamTwist`を選ぶ

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/3e0dc676-05fb-0012-d40e-3ae2cb1c371f.png)

画面左下のビデオの設定から`CamTwist`を選びます。

1つ前のセクションでの設定が上手くいっていれば、ブラウザやKeynoteなど、自分が選択した画面が表示されていることでしょう。

## （必要に応じて）スポットライトビデオをオンにする

この設定だとあくまで「誰かのカメラに写っているのがPC上の画面」でしかないので、参加人数が多いとよく見えません。
スポットライト機能をオンにして、大きく映すと見やすくなります。

https://support.zoom.us/hc/ja/articles/201362653-%E5%8F%82%E5%8A%A0%E8%80%85%E3%81%AE%E3%83%93%E3%83%87%E3%82%AA%E3%81%AB%E3%82%B9%E3%83%9D%E3%83%83%E3%83%88%E3%83%A9%E3%82%A4%E3%83%88%E3%82%92%E5%BD%93%E3%81%A6%E3%82%8B

公式のヘルプにもあるとおり、参加者のビデオにカーソルを合わせて`...`をクリックして`スポットライトを追加`を選びます。