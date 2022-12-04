<!--
title:   Emotionで自動付与されるベンダープレフィックスを外す
tags:    AdventCalendar2022,React,css-in-js,emotion
id:      f62ad271127773be1072
private: true
-->
## この記事の概要

CSS in JSのライブラリである[Emotion](https://emotion.sh)を使うと自動でベンダープレフィックスが付与されます。
されるのですが、その内訳があまり現実に即していないラインナップです。

というわけでベンダープレフィックスを外す方法を記事にしました。

:::note warn
autoprefixerを外すのを推奨する記事ではなく、あくまで「こういうアプローチもある」という紹介です。
:::

## 検証した際の環境

| パッケージ | バージョン |
| --- | --- |
| react | 18.2.0 |
| react-dom | 18.2.0 |
| @emotion/react | 11.10.5 |
| @emotion/cache | 11.10.5 |
| typescript | 4.6.4 |
| vite | 3.2.3 |

## デフォルトの挙動とその問題点

分かりやすい例として`display: flex;`を使用した際に出力されるCSSをお見せします。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/246fa915-592d-4f25-d518-cd775cd02ae0.png)

打ち消し線が入っていて少し見づらいですが、`display: flex;`以外に以下の3つの指定が追加されています。

- -webkit-box
- -webkit-flex
- -ms-flexbox

例えば`-webkit-flex`は、Chromeでいうとバージョン21から28まで必要でした。
`-ms-flexbox`はIE 10のために必要です（IE 11では不要です。）

この記事を書いている2022年12月現在、ここまでのサポートが必要な場面がどれだけあるのでしょう？

[Autoprefixer CSS online](https://autoprefixer.github.io/)で確かめてみると`Browsers: > 0.01%`の指定でようやく`-webkit-flex`と`-ms-flexbox`が現れます。
よしんば0.01%まで対応しようと思っても、`-moz-box`はEmotionからは生成されていないため不十分です。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/37dd277d-938b-a547-0fa2-e688444b4a17.png)

そもそも現在はベンダープレフィックスはあまり使う必要がありません。

かつてCSSにおいて実験的なプロパティを出す際、ベンダープレフィックス付きで実装していました。
今は、フラグや設定によってユーザーが制御して追加できるようになっています。（あくまで傾向の話ですが。）

https://developer.mozilla.org/ja/docs/Glossary/Vendor_Prefix

:::note
ただしそうは言っても必要なベンダープレフィックスもあります。
そちらは以下の記事でまとめました。
https://qiita.com/xrxoxcxox/items/3e0e34003a45d3618b29
:::

## 解決策

1. `createCache`に`stylisPlugins: []`を指定する
1. `CacheProvider`でラップする

### `createCache`に`stylisPlugins: []`を指定する

`@emotion/react`や`'@emotion/styled`の他に`@emotion/cache`が必要になるのでインストールします。

```shell
npm i @emotion/cache
```

https://emotion.sh/docs/@emotion/cache

Reactのルートになるファイルを編集します。
先程インストールした`@emotion/cache`から`createCache`をインポートし、適当な名前で定義します。
その中で`stylisPlugins: []`を指定します。

```javascript:App.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import createCache from '@emotion/cache'

const myCache = createCache({
  key: 'css',
  stylisPlugins: [], // これが重要
})

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

Emotionは内部で[stylis](https://github.com/thysultan/stylis)CSSプリプロセッサーを使用しています。
デフォルトでは`stylisPlugins`の中に`prefixer`が指定されているのを「何も使用しない」と上書きしているのがこのコードです。
ちなみにEmotionでは`prefixer`以外は使用していないので、この変更による他の影響は起きないはずです。

https://github.com/emotion-js/emotion/blob/d8a13bcae81812d3dff643bcf446709f965f0909/packages/cache/src/index.js#L44

---

ちなみに`createCache`の中の`key`は、今回の内容には何も関係ないのですが指定しなければなりません。
`createCache`を使わないときは`'css'`が指定されていて、特に変更したい意図もないのでこう記載しています。

https://github.com/emotion-js/emotion/blob/d8a13bcae81812d3dff643bcf446709f965f0909/packages/cache/src/index.js#L28

### `CacheProvider`でラップする

ここは特に説明する箇所はありません。
よくある感じで、Providerでラップするだけです。

```diff_javascript:App.jsx
  import React from 'react'
  import ReactDOM from 'react-dom/client'
  import App from './App'
  import createCache from '@emotion/cache'
+ import { CacheProvider } from '@emotion/react'

  const myCache = createCache({
    key: 'css',
    stylisPlugins: [],
  })

  ReactDOM.createRoot(document.getElementById('root')).render(
    <React.StrictMode>
+     <CacheProvider value={myCache}>
        <App />
+     </CacheProvider>
    </React.StrictMode>
  )
```

https://emotion.sh/docs/cache-provider

これにより、無事ベンダープレフィックスが消えました。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b3bb92c5-6fdd-5fe6-a974-92605c5bbfc0.png)

## 最後に

「browserlistを更新するとかで解決するでしょ？」と思っていたのに、随分とハマってしまいました。
実はstylisの`prefixer`プラグインは、どのプロパティにどのベンダープレフィックスをつけるかをハードコーディングしています。

https://github.com/thysultan/stylis/blob/master/src/Prefixer.js

そのため`prefixer`プラグインを使わないという強硬策に出る他ありませんでした。
検索してもあまり出てこない話題だったので困っている人自体が少ないのかもしれませんが……どなたかのお役に立てれば幸いです。

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox

Devトークでのお話してくださる方も募集中です！

https://jobs.qiita.com/employers/qiita-inc/dev_talks/489