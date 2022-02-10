<!--
title:   RemixでのCSSまわりに関する調査（2022年1月現在）
tags:    CSS,React,Remix,TypeScript,css-in-js
id:      10c07abf585b939e1c5b
private: false
-->
## この記事の概要

巷で話題の[Remix](https://remix.run/)ですが、スタイリングに関する記事はあまり見当たりません。
そのため自分で調査してまとめてみました。

まだ変化も大きいと思いますが、直近で導入を考えている人にとって役立てば幸いです。

## 公式ドキュメントにある情報

https://remix.run/docs/en/v1/guides/styling

### 主な方法

Remixは`<link rel="stylesheet">`の形式でスタイルを読み込み、適用するのが基本方針のようです。

以下の2つの方法が示されていますが、自前のスタイルを一切書かなくて済むサイトは少ないでしょう。
というわけで、実質的に2番目の方法は必須だと思われます。

1. リモートのスタイルシートを用いる
1. 通常のスタイルシートを用いる

#### リモートのスタイルシートを用いる

公式ドキュメントから例を拝借します。

```typescript:app/root.tsx
import type { LinksFunction } from "remix";

export const links: LinksFunction = () => {
  return [
    {
      rel: "stylesheet",
      href: "https://unpkg.com/modern-css-reset@1.4.0/dist/reset.min.css"
    }
  ];
};
```

例が示しているように、実質reset系のCSSやCSSフレームワークなど、サイト全域で使うものを指定することになるんだと思います。

:::note warn
もちろん`root.tsx`以外でもこの書き方自体はできます。
ですが、importするだけして未使用だった場合に警告は出ないなど管理が煩雑になる印象しかありませんでした。
:::

#### 通常のスタイルシートを用いる

1. CSSファイルを作成する
1. 各コンポーネントのファイルで`style`をimport
1. 各コンポーネントのファイルで`function links()`としてexport
1. 各ページのファイルで`links as XXLinks`といった形式でimport
1. 各ページのファイルの`links()`内で、スプレッド構文で`XXLinks`を展開

結構大変なのですが、イメージがつきづらい気がするので自分で試していたときのコードを載せます。

いくつかのコンポーネントを作り、それらをimportしてページを作る、という至って普通のシーンなのですが……。

```typescript:コンポーネントのファイル（他にもいくつか存在する）
import { ComponentPropsWithRef, forwardRef } from "react";
import styles from "~/styles/footer.css";

type Props = {
  className?: string;
} & ComponentPropsWithRef<"footer">;

export const links = () => [{ rel: "stylesheet", href: styles }];

export const Footer = forwardRef<HTMLElement, Props>(
  ({ className, ...props }, ref) => {
    return (
      // 省略
    );
  }
);
```

`styles`ファイルに作成したCSSをimportしつつ、このコンポーネントからもexportしています。

```typescript:ページのファイル
import { Header, links as headerLinks } from "~/components/RegularStylesheets/Header";
import { Navigation, links as navigationLinks } from "~/components/RegularStylesheets/Navigation";
import { FeedItem, links as feedItemLinks } from "~/components/RegularStylesheets/FeedItem";
import { Ranking, links as rankingLinks } from "~/components/RegularStylesheets/Ranking";
import { Footer, links as footerLinks} from "~/components/RegularStylesheets/Footer";
import globalStyles from "~/styles/global.css";
import pageStyles from "~/styles/page.css"

export function links() {
  return [
    { rel: "stylesheet", href: globalStyles },
    ...headerLinks(),
    ...navigationLinks(),
    ...feedItemLinks(),
    ...rankingLinks(),
    ...footerLinks(),
    { rel: "stylesheet", href: pageStyles },
  ];
}

export default function SomePage() {
  return (
    // 省略
  );
}
```

それぞれのコンポーネントで定義した`links`を`as`を使って別名でimportし、`links()`の中で展開しています。
コンポーネントが増える度にこちらの指定も増えるので、大した手間では無いものの面倒くさいのは否めません。

あと、これはあくまでimportやexportの仕方であってクラス名の衝突を防ぐのは自分で頑張る必要があります。
この方法のときはRemixが面倒を見てはくれません。

### Tailwind CSS

現状ではTailwindで書くのが唯一の方法な印象です。

1番導入が楽で、挙動の怪しい箇所も無さそうでした。

JITモードやapplyを使うかどうかはチームのルール次第なのでここでは触れません。

### styled-components

動くと言えば動きますが、全体的に怪しいです。

まず、公式にあるexampleの通りだと読み込み時に一瞬ちらつきが発生します。
`style`に埋め込まれるはずの文字列が一瞬画面全体に描画されてしまいました。

| リロードを繰り返している様子 |
| --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e5d09529-0b0b-38f1-2cd5-a9e1d17e90e2.gif) |

色々調べた末、[こちらのGist](https://gist.github.com/georgehenderson/d42d3817d15fad133df5bf7df47c5a20)を参考にして書いたら一応チラつきは改善されました。

ですが……``Prop `className` did not match. Server: "foo" Client: "bar"``をwarningが出るようになってしまい最終的に解決まで至れませんでした……。

一応、今私が書いたコードを置いておきます。
究明まで至れずすみません。

```typescript:entry.server.tsx
import { renderToString } from "react-dom/server";
import { RemixServer } from "remix";
import type { EntryContext } from "remix";
import { ServerStyleSheet } from "styled-components";
import StylesContext from "./StylesContext";

export default function handleRequest(
  request: Request,
  responseStatusCode: number,
  responseHeaders: Headers,
  remixContext: EntryContext
) {
  const sheet = new ServerStyleSheet();
  try {
    let html = renderToString(
      sheet.collectStyles(
        <StylesContext.Provider value={null}>
          <RemixServer context={remixContext} url={request.url} />
        </StylesContext.Provider>
      )
    );
    const styles = sheet.getStyleTags();
    html = html.replace("</head>", `${styles}</head>`);
    responseHeaders.set("Content-Type", "text/html");
    return new Response("<!DOCTYPE html>" + html, {
      status: responseStatusCode,
      headers: responseHeaders,
    });
  } catch (error) {
    console.log(error);
  } finally {
    sheet.seal();
  }
}
```

### PostCSS

あくまで`autoprefixer`などの付与がメインだと思うので今回は検証していません。

## 挙がっているIssueなど

調査をする中で見つけたIssueを貼っておきます。
styled-componentsでのチラつきの話はRemix側にもstyled-components側にもIssueが立っており、まだ解決できなさそうな雰囲気を感じています。

### Remix

https://github.com/remix-run/remix/issues/1032

https://github.com/remix-run/remix/issues/1325

### styled-components

https://github.com/styled-components/styled-components/issues/3660

### vanilla-extract

https://github.com/seek-oss/vanilla-extract/discussions/178

### stitches 

https://github.com/modulz/stitches/issues/908