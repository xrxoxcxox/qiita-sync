<!--
title:   特定の要素だけのスクリーンショットが欲しいとき
tags:    Chrome,Safari,firefox,tips,スクリーンショット
id:      8f7d6772021c1f5429a1
private: false
-->
## この記事の概要

日頃スクリーンショットを撮る機会は多いと思います。
そんな中でも「ドロップダウンメニューだけを写したい」のように、特定の要素だけのスクリーンショットが欲しいタイミングはありませんか？

実は各種ブラウザには初めから「特定の要素だけのスクリーンショット」を撮る機能があります。
範囲選択でのスクリーンショットでは、余白が揃えづらい・要素の幅や高さにピッタリと一致した画像は取得しづらいですよね。
ちょっとしたテクニックとして紹介します。

## 準備するもの

以下のブラウザならどれでもOKです。
（他のブラウザでもできそうな気はしますが今回は試していません。）

- Google Chrome
- Firefox
- Safari

## やり方

微妙な差こそありますが、どのブラウザでもほぼやり方は一緒です。
一旦Chromeを用いて説明します。

1. 目的のページを開く
1. 開発者ツールを開く
    - <img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/55fd0b29-6754-8ad8-f88a-eaedb65f72a4.png" width="600" alt="">
1. インスペクタなどから撮影したい要素をアクティブにする
    - <img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b846dd2d-820f-88aa-d67b-04f73ad17ea3.png" width="600" alt="">
1. 右クリック > Capture node screenshot
    - <img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/3d8bb5df-c434-1186-b5bf-416e58f4493b.png" width="404" alt="">
1. 保存するディレクトリを選んで完了

コンテキストメニューに記されているラベルは以下のようになっています。

| ブラウザ | バージョン | ラベル |
| --- | --- | --- |
| Google Chrome | 97.0.4692.71 | Capture node screenshot |
| Firefox | 96.0.1 | Screenshot node |
| Safari | 14.0.3 | Capture Screenshot |

:::note warn
Safariだけはnodeのスクリーンショットであると明示されていませんが、ちゃんと選んだnodeだけを撮影してくれます。
:::

## 仕上がりの差

当たり前と言えば当たり前ですが、ほとんど違いはありません。
ただしSafariだけは背景色が指定されていない場合は透化した画像として保存します。

背景色が欲しいとき・要らないときがあると思うので覚えておくと便利かもしれません。

| ブラウザ | 撮影したスクリーンショット |
| --- | --- |
| Google Chrome | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1b4aeac2-d24a-de58-4bdb-ff99d3d155e5.png) |
|  |  |
| Firefox | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/37c4c0bd-6a48-2317-058b-974b57a0372a.png) |
|  |  |
| Safari | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7eeb9565-6935-ce52-8f38-d58e384cb40d.png) |

## 余談

この記事の内容は本当に些細な小ネタですが、これを知るまで自分はわざわざグラフィックソフトを立ち上げて切り抜く場合すらありました。
（ある程度ちゃんとした資料を作るようなとき、中途半端に他の要素が入っているとノイズになるので、つい……）

知ってからはこのやり方で切り抜いていますが、周りの人に定期的に教えている機能の1つです。
それならいっそ記事にしてしまうか〜という背景でした。