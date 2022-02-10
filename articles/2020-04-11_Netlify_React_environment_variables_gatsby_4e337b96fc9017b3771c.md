<!--
title:   GatsbyJS + Netlifyで環境変数を利用するのに迷った話
tags:    Netlify,React,environment_variables,gatsby
id:      4e337b96fc9017b3771c
private: false
-->
私は自分のポートフォリオサイトを[GatsbyJS](https://www.gatsbyjs.org/)で作成して[Netlify](https://www.netlify.com/)でホスティングしています。
タイトルの通り、環境変数で迷った話とその解決策を書きます。

## この記事を要約すると

Gatsbyで作ったサイトをNetlifyなどでホスティングするなら、環境変数の名前は全て`GATSBY_`から始めるのが楽。

## 本題に入る前に

迷った話の前に、GatsbyとNetlifyそれぞれの環境変数の設定の仕方について書きます。
既にご存知の方はこのセクションは飛ばしてください:pray:

### Gatsbyの環境変数

[公式ドキュメントはこちら](https://www.gatsbyjs.org/docs/environment-variables/)

Gatsbyはデフォルトでは`.env.development`と`.env.production`の2種類の環境変数に対応しています。

以下のコードを書くと`process.env.ENVIRONMENT_VARIABLES`の書式で環境変数を呼び出せるようになります。

```javascript:gatsby-config.js
require("dotenv").config({
  path: `.env.${process.env.NODE_ENV}`,
})
```

`gatsby develop`を叩いたときは`.env.development`が
`gatsby build`または`gatsby serve`を叩いたときは`.env.production`が読まれます。

### Netlifyの環境変数

team > 対象のsite > Site settings > Build & deploy > Environment > Environment variables >
Edit variables

と進むとNetlify上で環境変数を設定できます。

以下はスクショ

![Frame 2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/cd21481e-cf1f-7140-bc60-73e00b432ed3.png)

![Frame 7.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1460fec4-ff26-cb41-b49c-3f0364a847fc.png)


![Frame 6.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/763df89f-308e-e4a5-e419-daab8c194515.png)


![Frame 5.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/77dd68bc-bba0-7427-c678-cc2ab6fab7fc.png)

![Frame 4.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/21ffeace-7381-4bf9-cd2a-6fc72262745c.png)


![Frame 3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/cae40dd2-dcdd-692e-3557-2d69e3bf013d.png)


`Key`と`Value`、それぞれに任意の値を入れてSaveすれば次回のデプロイから有効化されます。

## 何に迷ったか

やっと本題です。
`.env.*`ファイルをリポジトリにアップしているのは良くありません。

そのためやりたかったことは以下です。

1. `.env.development`と`.env.production`、ともにはローカルだけに置いて開発時だけ使う
1. Netlifyに環境変数を保存、本番のビルドではこちらを使用

ですが普通のやり方でNetlifyで保存した環境変数がビルド時に使われることはありませんでした。

## 解決策

**`.env.*`の中の変数も、Netlifyに保存する変数も、どちらも`GATSBY_`から始める**

でした。

`.env.development`と`.env.production`以外の場所に保存されている環境変数であっても、`GATSBY_`プレフィックスを持っていれば使用できるようです。

## おまけ

ここまで色々書いたんですが、私のサイトは開発環境でも本番環境でも使用する変数の中身は一緒です。

そのため`gatsby-config.js`の記載を以下のように変更し、

```diff:gatsby-config.js
- require("dotenv").config({
-   path: `.env.${process.env.NODE_ENV}`,
- })
+ require("dotenv").config()
```

`.env.development`と`.env.production`を`.env`に統合し、

`.env`内の変数に全て`GATSBY_`プレフィックスを付与すれば、

```diff:.env
- ENV_VAR = XXXXX
+ GATSBY_ENV_VAR = XXXXX
```

同じ値を2つのファイルに記載する必要がなくなって少し楽になりました。
めでたしめでたし。