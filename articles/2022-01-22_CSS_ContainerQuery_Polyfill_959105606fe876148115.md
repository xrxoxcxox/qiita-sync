<!--
title:   GoogleChromeLabsが提供しているContainer Query Polyfillを試してみた
tags:    CSS,ContainerQuery,Polyfill,コンテナクエリー,ポリフィル
id:      959105606fe876148115
private: false
-->
## この記事の概要

タイトルにあるように、GoogleChromeLabsが提供しているContainer Query Polyfillを試してみたので、その記録をまとめました。

リポジトリはこちらです。

https://github.com/GoogleChromeLabs/container-query-polyfill

ちなみに自分が実験していたリポジトリはこちらです。

https://github.com/xrxoxcxox/qiita-css-container-query-polyfill

## Container Query（コンテナクエリー）について

記事の本筋から逸れるので、補足程度に記載しておきます。
興味のある人は読んでみてください。

<details>
<summary>コンテナクエリーと、ついでにメディアクエリーの話</summary>
<div>

### コンテナクエリーって何？

コンテナクエリーとは、**要素のサイズ変化**に応じてスタイルを変化させられるCSSの書き方です。

例えば、要素の幅が480px以下になると左の見た目、480pxより大きければ右の見た目、といった制御ができます。

| width <= 480px | width > 480px |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ff4b1a0c-8ead-5b9d-477c-0c95f39eb537.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0cde91f0-79ca-91c4-328d-f10fd0f6fe55.png) |

これまではメディアクエリーを使ってレスポンシブデザインを実装していた部分が、今後はコンテナクエリーに置き換えられていくのではないかと思っています。

### メディアクエリーとは何が違うの？

メディアクエリーを使ったスタイリングの場合、**ビューポートのサイズ変化**に応じてスタイルを変化させます。
対してコンテナクエリーは**要素のサイズそのものの変化**に反応してスタイルを変化させます。

メディアクエリーを使っていた際は「画面幅が960px以下になると3カラム表示は窮屈だから2カラムに変えよう」といった考え方でスタイルを書いていたと思います。
ですがコンテナクエリーの場合は「1つの要素幅が320px以下になると文字が読みづらいからレイアウトを変えよう」のような考え方ができるようになるわけです。

最近はReactやVueに代表されるように、コンポーネントを組み合わせてビューを作っていくことが多いですよね。
メディアクエリーを使ったスタイリングの場合はビューポート全体に関心を向けないといけません。
コンポーネント内に書くのは少し躊躇してしまいます。

その点、コンテナクエリーであれば「自分自身の幅が○○px以下だったら見た目が変わる」なので良い分離ができるはず。
FigmaやSketchなどのグラフィックツールでのスタイル切り替え検証もむしろ容易になると予想しています。

</div>
</details>

## インストール方法

npmで公開されているので、一般的なライブラリと同様にインストールできます。

```shell
npm i container-query-polyfill
```

もしくはCDN経由で使用します。

```html
<script src="https://unpkg.com/container-query-polyfill/cqfill.iife.min.js"></script>
```

今回は軽く試したかっただけなのでCDN経由で使用しました。

:::note warn
こちらのポリフィルはChrome（またはEdge）の88以上 or Firefoxの78以上 or Safariの14以上のバージョンでしか動きません。
ResizeObserverとMutationObserverと:is()を使っている関係です。
:::

## 使用方法

インストールさえしたら後は通常通りのコンテナクエリーの書き方をするだけです。

例えば以下のようなHTMLがあり、`wrapper`の幅の変化に合わせて`item`のスタイルを変えたいとしましょう。

```html
<div class="wrapper">
  <div class="item">
    <!-- 何かしらの要素 -->
  </div>
</div>
```

この場合CSSは次のように書きます。

```css
.wrapper {
  container-name: wrapper;
  container-type: inline-size;
}

.item {
  /* 通常の幅のときのスタイル */
}

@container wrapper (width <= 256px) {
  .item {
    /* wrapperが256px以下になった際のスタイル */
  }
}
```

コンテナクエリー自体の書き方はまだあまり日本語の記事がありませんが、MDN Docsを見ればOKだと思います。

コンテナクエリーを指定するのは要素自体ではなく親要素であることや、`container-type`と`container-name`という見慣れないプロパティに若干戸惑うかもしれませんが、ほぼメディアクエリーでの書き方と一緒です。

https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Container_Queries

## 実際に出力されるコード

今回の利用はあくまでポリフィルですから、2022年1月現在で動くコードで挙動を再現しているだけです。
興味本位でどのようになっているかを見てみました。

:::note warn
内部実装を読んで理解できるほどのスキルは無いため、あくまで出力された結果の紹介です。
:::

まず、今回はこういったコードを書いています。（説明するのに最低限のコードだけ）

```html:index.html
<!-- 省略 -->
<main class="main">
  <a href="#" class="card"><!-- 省略 --></a>
  <a href="#" class="card"><!-- 省略 --></a>
  <a href="#" class="card"><!-- 省略 --></a>
  <!-- いくつか繰り返し -->
</main>
<!-- 省略 -->
```

`css:style.css
.main {
  container-name: main;
  container-type: inline-size;
}

.card {
  display: flex;
  gap: 16px;
}

@container main (width <= 480px) {
  .card {
    flex-direction: column;
    gap: 8px;
  }
}
`

これをデベロッパーツール上で見ると……

| width <= 480px | width > 480px |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/899d085d-89bd-a4ad-4e9e-395bd8ee2db9.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/700202f6-e3f7-2fcb-2f2e-c9fcd6a6a3cb.png) |

出力結果は意外とシンプルで、`selector`が`:is(selector).cq_${uid}`になって`@container`内で指定したスタイルが追加されている形式でした。

最終的な出力をしているは以下のfunctionです。

https://github.com/GoogleChromeLabs/container-query-polyfill/blob/faf2600f7cd7b16e500f40676dd1b144005bd186/src/engine.ts#L660

`:is()`は無くても動く気がするのですが、どうしてついているのでしょう……？
詳細度の問題かなと思ったのですが、手元で動かしている分では`:is()`無しでもちゃんとスタイルがあたっていて、解明しきれませんでした。

## ハマった箇所

スタイルの確認をしたいだけだからと思って、`index.html`をChromeで直接開いてみたら以下のようなエラーが出てしまいました。

```
cqfill.iife.min.js:2
Fetch API cannot load file:///Path/to/directory/style.css. URL scheme "file" is not supported.
```

READMEには記載がありませんでしたが、CORSエラーになってしまうのでローカルサーバーを立ち上げて解消です。

今回テスト用に作ったリポジトリには`index.html`と`style.css`しか入れていなかったのでVSCodeの拡張機能である[Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)でささっと試しました。

## まとめ

- コンテナクエリーが一足先に試せる
- npmでのインストールかCDN経由での適用のどちらでもOK
- ローカルサーバーの立ち上げ、忘れずに