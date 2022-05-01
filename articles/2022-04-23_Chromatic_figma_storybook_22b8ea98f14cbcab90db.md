<!--
title:   FigmaのStorybookプラグインを試してみた
tags:    Chromatic,figma,storybook
id:      22b8ea98f14cbcab90db
private: false
-->
## この記事の概要

2022年3月31日に、StorybookからFigma用プラグインが出ました。

https://storybook.js.org/blog/figma-plugin-beta/

「FigmaとStorybookを統合し、モックアップとコードの二度手間をなくそう（意訳）」という世界観は非常に良さそうなので、軽く試してみました。

> <img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4dd05570-7699-6c72-6ea1-06479671454e.png" alt="" width="500">
>
> 上記の記事から画像を引用

## 結論

過度に期待を煽ってしまうのも良くないので先に結論を書いておきます。
この記事を書いている2022年4月23日現在では、まだそこまで利用価値が高いとは言えなさそうでした。

というのも、Figma上で比較はできるのですが各種Controlsが触れないなど「見た目上問題無さそうか」の確認くらいしかできなさそうです。
幅や余白などを数値で示す機能はありますが、Storybookを開けば済む話というか……「Figmaプラグインでこそ便利になる」という印象はありませんでした。

とは言え導入方法やできることをまとめた記事はほぼ見かけないので、これからの役に立つかもと思いこのまま投稿します。

## 導入の前に

パブリッシュ済みのStorybookが必要です。
[こちらの記事](https://www.chromatic.com/docs/figma)ではChromaticからパブリッシュしたStorybookでのみ使えるような書き方になっていますが、筆者はセルフホストしているStorybookを持っていないので使用可否を試せていません。
もしかしたら大丈夫なのかもしれませんが、Chromaticが推奨されているので乗っかっておく方が何かと楽だとは思われます。

また、Chromatic経由でのパブリッシュの場合、Projectの`Manage > Collaborate > Visibility`を`Public`にしておかないとプラグインが使えません。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5b80c27a-1414-6587-22ff-28273540d2d4.png)

ちなみに、この記事ではChromaticの説明はしませんが別な記事で導入方法を紹介しているので良ければこちらも一緒にご覧ください。

https://qiita.com/xrxoxcxox/items/1ddfe5f8f4917f9c0999

## 導入方法

まずはプラグインをインストールします。

https://www.figma.com/community/plugin/1056265616080331589/Storybook-Connect

Figma上でプラグインを起動するとこのようなウィンドウが出現します。

<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5cddbfcc-622b-b929-08ad-cb0c835dc54d.png" alt="" width="500">

`Sign in with Chromatic`を押すと、認証用のコードとChromaticへ遷移するためのリンクが出現。

<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/398cf5c3-69a9-9327-c7bc-37e7a96e45cd.png" alt="" width="500">

ブラウザが立ち上がり、連携のページが開きます。
先ほど表示されたverify用の数字を入力して次へ進みます。

（記事を書くために後でスクリーンショットを撮ったのでコードが違っていますが、本当は同じコードです。）

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c1cba05a-3165-3373-3cd6-bd266b4d3b0d.png)

コードが正しければ色々と聞かれます。
すべてオンにして次へ進みましょう。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/dc549ca7-0132-0874-be58-4e915d71561b.png)

完了するとこのような画面に。
ここまで来たらウィンドウは閉じてしまって大丈夫です。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/2643b821-74d7-e15d-3115-900aab4f095a.png)

Figmaに戻ると、先ほどのプラグインのウィンドウの内容が変わっています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9e9a78db-8998-d4e3-d5cf-8fd268912706.png)

リンクしたいコンポーネントを選択すると、URLを入力する欄が出現。
Storybookの該当コンポーネントのURLを入れて次に進みます。

:::note warn
[導入の前に](#導入の前に)でも記載しましたが、ChromaticのVisibilityがPublicになっていないと正しいURLを入れてもエラーになってしまいます。
:::

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f94f8efb-45d4-b112-c62f-04bfc25cd6af.png)

連携が完了すると、このようにStorybookでの結果が表示されています。
これを見て、Figmaの方はフォントに`Noto Sans JP`を指定していたけど、Storybookでは何も指定していなかった、と思い出しました。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ae851346-d303-c48a-bf96-f75e02ef2720.png)

gridやmeasureなど、通常のStorybookで使える機能はそのままです。

| grid | change size | measure | outline |
| --- | --- | --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/87c16a01-82c8-fccb-d333-1d1d7e810d91.png) | ![11-change-size.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/feb880df-57ac-0d9e-53b6-acae8df73e37.png) | ![12-measure.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f764d795-a9ea-825e-fce0-b8d4ccc8d940.png) | ![13-outline.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7632a52c-81db-70c7-20e0-fbfe394130c8.png) |

[結論](#結論)で記載したとおり、Controlが使えないとか、子Story（？）を切り替えられないため「見ておしまい」という印象があります。

強いて言えばリンクしたコンポーネントを選択すると右サイドバーにStorybookのリンク（`Open story in browser`）が出現するため、迷わずジャンプできるのがユニークな利点でしょうか。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bd4dee66-3739-e76c-1e82-a2effb98bfdf.png)

## まとめ

- もともとStorybookとChromaticを使っているなら導入は非常に簡単
- しかし、このプラグインでの連携をするためだけに新たにChromaticを検討するほどとは思えない
- とは言え現在ベータ版なのでこれからの進化に期待

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox