<!--
title:   デザイナーが1人でGatsbyを使ってサイトを作ったよ #1
tags:    Design,React,emotion,figma,gatsby
id:      ac5937fb0e54a875787a
private: false
-->
## この記事について

### 内容を一言でまとめると

フロントの知識があまりないデザイナーが、サイトを公開するに至った道のり。
ちなみに[続きの記事](2020-10-24_Design_React_emotion_figma_gatsby_c27b92e763f3833af6dd.md)も書きました。

### こんな人が読んでくれたら嬉しい

* 1からコードを書くことが苦手なデザイナー
* 自分のサイトを作って公開したことがないデザイナー

## 目次

1. [作ったサイト](#作ったサイト)
2. [使っている技術、サービスなど](#使っている技術サービスなど)
3. [GatsbyJSに決めるまでの経緯](#gatsbyjsに決めるまでの経緯)
4. [GatsbyJSに決めてからやったこと](#gatsbyjsに決めてからやったこと)
5. [ドメイン取得](#ドメイン取得)
6. [小さなプロトタイプ作り](#小さなプロトタイプ作り)
7. [コンテンツ作成](#コンテンツ作成)
8. [スタイリング](#スタイリング)
9. [Netlifyへの移行](#netlifyへの移行)
10. [スターター選定、プラグイン導入など](#スターター選定プラグイン導入など)
11. [まとめ](#まとめ)

## 作ったサイト

<img width="800" alt="Qiita記事用1.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9bcb1e98-2f66-45b2-f9e6-76ab4ff75826.png">

[自分のポートフォリオサイト](https://www.keisukewatanuki.work/)を作りました。
まだまだコンテンツは少ないのですが、これで完成ではありません。
これからの育ち方も含めて1つの作品にするつもりです。

[GitHubはコチラ](https://github.com/xrxoxcxox/playground)

## 使っている技術、サービスなど

* [Figma](https://www.figma.com/)（ビジュアルデータ作成）
* [GatsbyJS](https://www.gatsbyjs.org/)（静的サイトジェネレーター）
* [Emotion](https://emotion.sh/)（CSS in JSのライブラリ）
* [Netlify](https://www.netlify.com/)（ホスティングサービス）
* [zeroheight](https://zeroheight.com/)（デザインガイドライン作成）

## GatsbyJSに決めるまでの経緯

会社の業務でReactを使うことが増えていたので、
「**練習を兼ねてReactで作ろうかな〜**」くらいの軽い気持ちでいました。

しかし、作ろうとしていたのは個人のポートフォリオサイト。

* 静的なコンテンツ
* DOM操作はほとんどしない（はず）

わざわざReactで作る意味があるのかなあと少し悩みました。

そんな中、TwitterでGatsbyJS関連の記事が流れてきて初めて認知。
「**環境構築が簡単で表示も速いらしい、更新も楽そうだしこれを使ってみたいな**」と決定。

## GatsbyJSに決めてからやったこと

まずは[公式のドキュメント](https://www.gatsbyjs.org/docs/)を一通り読みました。

本当は日本語記事を見ながらやれば良いかと思っていましたが、**全然見つかりません**。
公式リファレンスをちゃんと読む練習も兼ねて挑戦。

[クイックスタート](https://www.gatsbyjs.org/docs/quick-start/)の通りにコードを書いてみてイメージを掴みます。
Gatsby CLIをインストールしてサイトの雛形を作成、develop, build, serveと基礎の基礎だけを理解しました。
ちなみに

* develop = 自分のPC上でサイトを立ち上げる、本番環境で人が見るコードとは違う
* build = 静的なファイルに書き出し、ここで出来たものをアップロードすれば公開できる
* serve = 本番環境と同じものを自分のローカル上で確認できる

というイメージです。
最初は正直これらの差もあまりわかっておらぬまま進めていましたが**どうにかなりました。**

その後は[チュートリアル](https://www.gatsbyjs.org/tutorial/)を全部やりました。
HomebrewやNode.jsの導入から教えてくれるので環境構築で詰まることは少ないのではないでしょうか……？
少なくとも今まで自分が見たことのあるチュートリアルの中ではかなり親切な方だと思います。

チュートリアルの全体は

1. 環境構築
2. JSXの書き方
3. CSSの当て方
4. Componentのネストによるレイアウトの仕方
5. データの扱い方（GraphQL）
6. `gatsby-source-filesystem`の扱い方（Gatsbyがどうやって各種データを保持していて、いかに出力するのか？みたいなことが書いてあります）
7. MarkdownからHTMLへの変換の仕方
8. 複数のページをMarkdownから出力するやり方
9. サイト公開の準備（SEO含む）

というものです。
（本当はこの後にThemeについてのチュートリアルともう少し突っ込んだチュートリアルがありますが、万人に必要なものではないと思います。楽しくなってきちゃった方だけで大丈夫かと。）

## ドメイン取得

チュートリアルを終えた後の気持ちは
「**全然理解してないけどとりあえず手を動かそう**」
でした。

が、ここで問題が発生します。

**自分のドメインを持っていないし、取得したこともありません。**

記憶が曖昧ですが、たしか「ドメイン 取得 おすすめ」などと検索。
出てきた記事内でオススメされるがままに[ムームードメイン](https://muumuu-domain.com/)で取得したはずです。

ドメイン取得は値段以外で何がどう違うのか未だに分かっておらず……。
詳しい方がいれば教えていただきたいです。

あとはこのタイミングで[ロリポップのレンサバ](https://lolipop.jp/)も契約していました。
一瞬だけ使った後に[Netlify](https://www.netlify.com/)へ移行済。

ちなみに取得したのは**.work**。
初年度だけでなく2年目以降も割と安かったのと、
「**デザイン以外のサイトコンテンツも作りたくなるかもしれないし、汎用的な方が良いよね！**」
と捕らぬ狸の皮算用。

## 小さなプロトタイプ作り

歴史を遡ったところ、一番最初に公開したのはコレのようです。

![当時のサイト.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ffd03bb3-1a77-d69a-ea2e-821e1327f6d0.gif)

* ページ遷移、なし
* コンテンツ、なし
* ただ文字がアニメーションしているだけ。

たったこれだけのサイトでしたが

* ドメインとレンサバを紐づけるのってどうやってやるんだ？
* 会社ではmergeすれば本番公開されるけど、今ってされてなくない？
* WHOISってなんだよ？
* よくわかんないけどポチポチしてたらhttpsになった、一体何が起こったのか？

などと分からないことだらけ。
結局Netlifyに移行したので必要なかった作業も多いのですが……色々調べる機会になりました。

自分でやってみたことでエンジニアの皆さんへの感謝がぶち上がり、一人で四苦八苦した価値がありました。
いつもありがとうございます。

## コンテンツ作成

<img width="1440" alt="スクリーンショット 2019-09-02 14.48.19.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/65c3e299-8975-78da-b10c-c76d724725f0.png">


意味深なアニメーションだけのトップページから脱却すべく、下層ページを作成しました。

テキストだけローカルで書いて、あとはせっせとFigmaにコピペ。
（Figmaには日本語入力バグがあってFigma上でテキストを打つのはかなりしんどい）
デザインデータを作るのはいつもやっているように進めるのみ。

[コンテンツ作成のポリシーなど](https://www.keisukewatanuki.work/about-this-portfolio)はポートフォリオに記載していますので良ければそちらをご覧ください。

## スタイリング

せっかくGatsbyというモダンなジェネレーターを使うのだからと、スタイリングも新しいものにチャレンジしてみました。

### ボツになったものたち

* [styled-components](https://www.styled-components.com/)
* [styled-jsx](https://github.com/zeit/styled-jsx)
* [CSS Modules](https://github.com/css-modules/css-modules)
* [JSS](https://cssinjs.org/?v=v10.0.0-alpha.24)

初めのしばらくは[styled-components](https://www.styled-components.com/)を使用。

機能的に悪い箇所はなかったのですが、なぜスタイル宣言時にタグを指定しなければならないのかが腑に落ちずあまり好きになれませんでした。

```jsx
// 公式にあるコード
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;
```

sectionかarticleかdivかなど……タグの意味と見た目は分離されているべきではないでしょうか？

また、はじめに宣言したタグから変更することも出来るのですが

```jsx
// 公式にあるコード
const Component = styled.div`
  color: red;
`;

render(
  <Component
    as="button"
    onClick={() => alert('It works!')}
  >
    Hello World!
  </Component>
)
```

どういうことかというと

* スタイリングされた`div`としてComponentを宣言する
* Componentの使用時に「このComponentを`button`扱いにする」という意味のpropを渡す

定義したスタイルは一緒なのに都度`as`を渡さなければならないのがイマイチです。
かといってタグ違い用に同じ見た目のComponentと複数作るのはもっとWET。

ファイルでComponentを分けているのにComponent内に見た目のためだけのローカルな（？）Componentが複数生まれるのもなんだかしっくり来ず……。

[styled-jsx](https://github.com/zeit/styled-jsx)も試しましたが、こちらも似たような理由でやめてしまいました。
あとはJSX内に毎度毎度別のタグっぽく`<style jsx>`を入れないといけないのもなんだか……。

その他[CSS Modules](https://github.com/css-modules/css-modules)やら[JSS](https://cssinjs.org/?v=v10.0.0-alpha.24)やらも検討しましたが採用には至らず。

### 今採用しているもの→Emotion

最終的には[Emotion](https://emotion.sh/)に落ち着きました。
[css PropのString Styles](https://emotion.sh/docs/css-prop#string-styles)を使ってスタイルを書いています。
Object Stylesだと[CSSType](https://www.npmjs.com/package/csstype)が使えるっぽいのですが、頭にすっと浮かぶプロパティはハイフン付きのものなので迷いつつString Stylesを選択。

実際のコードはこんな感じです。

```jsx:Button.js
import React from 'react'
import { Link } from 'gatsby'
import hexToRgba from 'hex-rgba'

import { css } from '@emotion/core'
import Color from '../styles/Color'
import Size from '../styles/Size'
import Typography from '../styles/Typography'

const root = css`
  display: inline-block;
  color: ${Color.White};
  background-color: ${Color.Blue};
  ${Typography.Body};
  font-weight: 500;
  text-decoration: none;
  border-radius: ${Size(1)};
  box-shadow: 0 ${Size(0.5)} ${Size(2)} ${hexToRgba(Color.Black, 16)};
  padding: ${Size(2)} ${Size(3)};
`

export default ({ children, to }) => (
  <Link to={to} css={root}>
    {children}
  </Link>
)
```

ColorとかSizeとかTypographyは自分で作った定数や関数で、いちいち値を書かなくて良いようにしています。

## Netlifyへの移行

ドメインをとったのがムームードメインで、当初は運営が同じGMOペパボさんで連携があるロリポップのサーバーを当初は借りていました。
しかし……ロリポップのマニュアルに載っていたのはFTPでファイルをアップロードするやり方。

はじめの内こそ

1. ローカルで開発
2. `Gatsby build`でpublicディレクトリに書き出し
3. 中身をまとめてアップロード

と律儀にやっていましたが……**正直面倒くさい！**
怠惰な性格には自信があるので、簡単に出来るやり方を模索していました。

調べているうちに[Netlify](https://www.netlify.com/)なら簡単に出来そうだったので使ってみることに。

初期設定の時期は**良い感じのタイミング**で**やるべきことの指示が出る**ので
言われるがままやっていたら

* GitHubでmasterにmergeしたら本番リリースされる設定
* 常時SSL化
* Netlify DNSの準備

などが終わっていました。
あれ？こんなんで良いんでしょうか……書いててちょっと不安になってきました。

## スターター選定、プラグイン導入など

若干時系列は前後しますが、Gatsbyで新規にサイトを作成するときは

```
gatsby new [SITE_DIRECTORY] [URL_OF_STARTER_GIT_REPO]
```

というコマンドを叩きます

自分はデフォルト設定になっている[gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default)から作りました。

スターターなのに若干CSSの指定が入り組んでいるような気がしますが……多分一番シンプルに始められます。
このスターターを使った時点で適用されているプラグインは

* gatsby-plugin-react-helmet
* gatsby-source-filesystem
* gatsby-transformer-sharp
* gatsby-plugin-sharp
* gatsby-plugin-manifest

これで全部のはずです。
具体的に何がどう動くというよりはGatsbyの根幹を作っているプラグイン達だと思います。

動くものが出来てきたら徐々にプラグインを増やしていきました。
下記のものは2019年8月27日現在のリストなので多分まだ増えると思います。

* [gatsby-plugin-emotion](https://www.gatsbyjs.org/packages/gatsby-plugin-emotion/)
  * 途中で説明したEmotionを使うために必要なプラグインです
* [gatsby-plugin-sitemap](https://www.gatsbyjs.org/packages/gatsby-plugin-sitemap/)
  * SEO用に入れました、サイトマップを出力してくれます
* [gatsby-plugin-netlify](https://www.gatsbyjs.org/packages/gatsby-plugin-netlify/)
  * 機能は色々ありますが、301転送をしたいがために入れました

## まとめ

多分フロントエンドエンジニアの方にコードレビューしてもらったらすごい数のご指摘をいただくと思うんですが、**それでも動いています**。

苦労しながらも一人で動くものを作れた経験は大きいと思っていて、引き続きこのサイトを使って新しい挑戦をしていくつもりです。

また、記事の冒頭に挙げた

* 1からコードを書くことが苦手なデザイナー
* 自分のサイトを作って公開したことがないデザイナー

こういった人たちに「自分も試しにやってみようかな」と思えるきっかけを提供できたら嬉しいです。

最後まで読んでいただいてありがとうございました。