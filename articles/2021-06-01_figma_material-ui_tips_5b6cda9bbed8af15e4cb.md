<!--
title:   FigmaのInteractive componentsでMaterial-UIのText Fieldを再現してみる
tags:    figma,material-ui,tips,ネタ
id:      5b6cda9bbed8af15e4cb
private: false
-->
## これは何

- タイトルの通り、FigmaのInteractive componentsでMaterial-UIのText Fieldの変化を再現してみた記事です
    - 一部再現しきれていませんが、パッと見はかなり近いものができたと思います
- また「[3000文字Tips - 知ると便利なTipsをみんなへ届けよう](https://qiita.com/official-events/d523df99d6479293ffa7)」イベントへの投稿記事でもあります

## そもそもMaterial-UIのText Fieldってどんなもの

https://material-ui.com/components/text-fields/

公式ドキュメントはこちらです。

いくつか種類がありますが、今回は1番ベーシックな見た目のものを再現してみます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e79b28ab-2acf-5f2f-4ccb-5f3117d9a76a.gif)

## 実際に作ってみた

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a8d6645a-e06e-2ea3-0023-a3d955481b80.gif)

だいたい良い感じではないでしょうか？

[作業したFigmaファイルはこちらです。](https://www.figma.com/file/ArCLltBwFSEFnLSl7RQBbz/Qiita-Material-UI-Text-Field-Mockup?node-id=0%3A1)
2021年6月1日現在β版であるInteractive componentsの機能を使っているので、人によっては再生できないと思いますが、構造を見ることはできるはずです。

## 作り方

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/112384bb-e62c-d8c9-4538-7f35abf52224.png)

まずは展開した状態から作りました。

ポイントは2つです

1. 上に移動するラベルを高さ12pxのFrameで囲っておく
    1. プレースホルダーっぽい見た目のときが16px、上に移動したときが12px
    1. 実際のMateril-UIでも、上に移動したラベルの高さ込みでコンポーネントが構成されている
1. 後から太くなるボーダーを高さ1pxのFrameで囲っておく
    1. hoverや入力時に太くなるのでFrameで囲っておかないとレイアウトがずれる

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7bca26f9-ca11-7fcb-d334-ac695e0830ca.png)

次はプレースホルダー状態を作りました。

Frameはそのままの位置で`Clip content`のチェックを外し、ラベルの位置と大きさだけ変更
これでレイアウトに影響を与えず動かせます

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7a4cdc11-bc9f-9fa0-e7f2-e958cb8b832d.png)

hoverや入力中の見た目も、1pxのFrameで囲った上で`Clip content`のチェックを外し、ボーダーの高さを2pxに増やしています。
こちらも全体のレイアウトに影響を与えずに太くできています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d96922da-a3d6-de52-ddce-470f4cce1d19.png)

あとはComponent化→Variants設定をした上でPrototypeモードでトリガーを与えていきます。

| 状態 | トリガー1 |　変化する先1 | トリガー2 | 変化する先2 |
| --- | --- | --- | --- | --- |
| デフォルト | Mouse enter | ホバー状態 |  |  |
| ホバー状態 | Click | 入力中 | Mouse leave | デフォルト |
| 入力中 | Click | 入力完了 | Key/gamepad (Backspace) | デフォルト |
| 入力完了 | Key/gamepad (Backspace) | デフォルト |

`Mouse enter`と`Mouse leave`はひとまとめにして`While hovering`でも良いのですが、`While hovering`だと何故か微妙にチラつくシーンがあるのでこのようにしてみました。

ちなみにAnimationは全てSmart animationで200msで動かしています。

## 後書き

ここまで書いて気がつきましたが、なぜMaterial Designを参照せずいきなりMaterial-UIを真似ていたのでしょう。
自分でもわかりませんが、だいたい再現できたので良しとします。