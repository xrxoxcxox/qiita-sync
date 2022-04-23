<!--
title:   Material Design IconsがMaterial Symbolsに進化していた
tags:    Design,MaterialDesign,figma,tips,デザイン
id:      c7946d6b50589087f802
private: false
-->
## この記事の概要

Google FontsのTwitterアカウントにて、何やら気になる告知がありました。
どうやらMaterial Designにおけるアイコングラフィーがアップデートされたようです。

https://twitter.com/googlefonts/status/1516934123700383744

変更点について簡単にまとめてみました。

## Material SymbolsとMaterial Design Iconsの違い

https://fonts.google.com/icons

既にGoogle Fontsのサイトはアップデートされています。
過去のページは見れませんが、Figmaのコミュニティファイルとプラグインを並べて見ると比較ができそうです。

### Material Symbols

Figmaのプラグインが公開されています。

https://www.figma.com/community/plugin/1088610476491668236/Material-Symbols

起動するとこのような見た目。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/fcd4d72a-f225-f197-9586-ab4261079733.png)

変更できるプロパティは以下の5つです。

- Style
    - Outlined
    - Rounded
    - Sharp
- Weight
    - 100
    - 200
    - 300
    - 400
    - 500
    - 600
    - 700
- Fill
    - On
    - Off
- Grade
    - -25 (Dark theme)
    - 0 (Normal)
    - 200 (Emphasis)
- Optical size
    - 20dp
    - 24dp
    - 40dp
    - 48dp

### Material Design Icons

こちらはプラグインではなくファイルです。
ライブラリとして使う用途です。

https://www.figma.com/community/file/1014241558898418245/Material-Design-Icons

変更できるプロパティは1つのみです。

- Style
    - filled
    - outlined
    - rounded
    - sharp
    - two-tone

### Material Symbolsで新たに追加されたプロパティ

#### Weight

font-weightと同じようなイメージです。
数字が小さくなれば細く、大きくなれば太くなります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/fc32fd8f-6fa6-2ce7-3ccd-7f5e753187f2.png)


#### Fill

Offなら線だけ、Onなら塗りあり、のイメージです。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/8bc6a18c-81a3-ef2c-52dc-03fbd88e7035.png)

#### Grade

Weightに似ていますが、Weightよりももう少し小さな変更用のプロパティだと思われます。
ダークモードで使用すると少し見え方が強すぎるため、`-25`を用いてほんの少しだけサイズや線幅を控えめにする……というイメージです。

ダークモードのカラーパレットを作る際、ライトモードよりも少し彩度を低くするのが定石なのと似ている気がします。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/fe991a8b-d18c-0572-ab9d-d5788b72e31a.png)

Figmaのプラグインでは`Dark theme`, `Normal`, `Emphasis`と記載されていますが、サイトでは`-25`, `0`, `200`と記載されています。
表記揺れでしょうか。


#### Optical size

同一のアイコンを単に拡大縮小をすると印象が変わってしまうため、それぞれのサイズにあわせた太さのバリエーションがあります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b2cadaf1-4d0a-09df-469f-e8b1385fba98.png)

### Material Symbolsで削除されたプロパティ

styleのうち、`filled`と`two-tone`がなくなっていました。

`filled`に関しては`Fill`プロパティで別個に切り出されたので、名前が変わっただけと言えるでしょう。
`two-tone`は……あまり使われていなかったのでしょうか？
個人的には`two-tone`を使いたい場面がなかったため、なくなったことに問題は感じていません。
もし多用していた人はFillなどに置き換える必要がありそうです。

## まとめ

- 新しくMaterial Symbolsになったことで、細かい調整がしやすくなった
    - Weight, Grade, Optical size
- 基本的に進化した項目ばかりだけど、`two-tone`だけは削除されている

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox