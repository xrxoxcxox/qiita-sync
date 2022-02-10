<!--
title:   FigJamからFigmaにコピペしてもそのまま使えるオブジェクト・そうでないオブジェクト
tags:    Design,figjam,figma,デザイン
id:      cc891880d3b0dc717029
private: false
-->
## この記事の概要

Figma社から出ているオンラインホワイトボードツールのFigJamですが、一部のオブジェクトはFigmaにコピペしてもそのまま使えます。
それらの特徴を記事にしました。

## FigJamのオブジェクトと、それぞれの説明

### Marker, Highlighter

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/dd44e511-666e-e344-42c7-bc9a32b011e2.png)

コピペした際は通常のPathと同様に扱われる。

### Shape

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/8c03639a-125e-8c7d-09f4-6c18b67148a6.png)

どの形状であってもOK。（テキストを追加・編集できる）
コピペした際はFigJamと同様に、テキスト量にあわせてサイズが変わる。

### Sticker

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4fcf0ff0-dee4-65a5-907a-774bdd75f0ef.png)

コピペ可能なものの、Figma上ではサインの有無を変更できない。

### Text

コピペした際は通常のTextと同様に扱われる。

### Connector

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4c251f39-81e4-16e9-7020-4d32f29156b0.png)

FigJamからFigmaにコピペして1番有用だと思われる。
Frame同士を繋げられて、なおかつ繋いだFrameを動かしても追従してくれる。

### Stamp

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7bde5e9a-74ab-fc76-6ab8-f8cecc7434d0.png)

コピペは可能なものの、Figma上で入れ替えたりはできない。

### Widgets

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/686584e9-bd4e-e686-6fec-3d86f6221194.png)

Widgetsのままではペーストできない。
`Widgets cannot be pasted into Figma`と表示され、すぐ横に`Paste as layers`と書かれたボタンがある。
そちらを押下すると通常のグラフィック要素としてペーストされる。

### Media

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e4d8f526-6404-66ea-ca0c-c86b46aa3a65.png)

Mediaのままではペーストできない。
`Embeds cannot be pasted into Figma`と表示され、すぐ横に`Paste as layers`と書かれたボタンがある。
そちらを押下すると通常のグラフィック要素としてペーストされる。

### Code block

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b9a8450a-4519-010e-8b6a-81262186feae.png)

ペーストできない。
`Can't paste code blocks from FigJam to Design Files`と表示されるのみ。

WidgetsとMediaは`Paste as layers`ボタンを押せばグラフィック要素としてペースト可能だったものの、こちらは一切ペーストできない。