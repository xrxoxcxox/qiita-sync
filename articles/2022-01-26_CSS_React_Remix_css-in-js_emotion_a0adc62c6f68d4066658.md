<!--
title:   RemixでEmotion(CSS in JS)を使う
tags:    CSS,React,Remix,css-in-js,emotion
id:      a0adc62c6f68d4066658
private: false
-->
## この記事の概要

以下の記事に続き、[Remix](https://remix.run/)におけるスタイリングを調査していたため記事にしました。
今回は[Emotion](https://emotion.sh/docs)を使ってみます。

https://qiita.com/xrxoxcxox/items/10c07abf585b939e1c5b

## 実験に使っていたリポジトリ

https://github.com/xrxoxcxox/qiita-remix-with-emotion

## EmotionにおけるServer Side Rendering

https://emotion.sh/docs/ssr

上記は公式ドキュメントですが、かいつまむと**条件にあわせて次のどちらかのアプローチをとる**こととなります。

- `:nth-child()`などの疑似クラスを使わない場合（Default Approach）
    - `renderToString(<App />)`をとするだけでOK
- `:nth-child()`などの疑似クラスを使う場合（Advanced Approach）
    - 通常の`@emotion/react`や`@emotion/styled`に加えて
    - `@emotion/server/create-instance`や`@emotion/cache`も用いて
    - `App`を`CacheProvider`でwrapして
    - `head`にスタイルを差し込む

Advanced Approachは箇条書きで書いても伝わりづらいと思うので、是非公式ドキュメントを一読ください。

`:nth-child()`の類は便利ですが、初めから使わないと決めていればやれないことはありません。
というわけで今回はDefault Approachを採用しました。

## 導入

[前回の記事でのstyled-componentsの実験](https://qiita.com/xrxoxcxox/items/10c07abf585b939e1c5b#styled-components)はかなり苦戦しましたし、最終的にwarningを解消できないまま……。

ところがEmotionは特に苦労することもなく使用できました。
というのも、初めから`app/entry.server.tsx`は以下のようになっていてEmotionの言う`Default Approach`がクリアされているのです。

```typescript:app/entry.server.tsx
import { renderToString } from "react-dom/server";
import { RemixServer } from "remix";
import type { EntryContext } from "remix";

export default function handleRequest(
  request: Request,
  responseStatusCode: number,
  responseHeaders: Headers,
  remixContext: EntryContext
) {
  const markup = renderToString(
    <RemixServer context={remixContext} url={request.url} />
  );

  responseHeaders.set("Content-Type", "text/html");

  return new Response("<!DOCTYPE html>" + markup, {
    status: responseStatusCode,
    headers: responseHeaders
  });
}
```

ただ、多少今まで通りにいかない箇所もあるためそれらの点について記載しておきます。

## 注意点

### jsx pragmaを省略できない

Emotionを単にインストールした状態だと、毎回ファイルの最初に`/** @jsx jsx */`と記載せねばなりません。
本来は`@emotion/babel-preset-css-prop`や`@emotion/babel-plugin`の使用によって省略できるのですが、RemixではBabel Pluginが使えないため毎回書く他無さそうです。

https://github.com/remix-run/remix/issues/717

Issueこそありますが、対応されないような気もしています……。

また、Babelの設定が変えられない都合上デフォルトでは`Fragment`の省略記法（`<></>`）も使えません。
以下のどちらかを実施する必要があります。

- `/* @jsxFrag React.Fragment */`と記載する
- `<React.Fragment>`（または`<Fragment>`）と都度記載する

### double render問題は変わらず

またも前回の記事との比較ですが、styled-componentsと比べれば全然許容できる範囲ではあるものの、スタイルが適用されるまでに一瞬レイアウトが崩れています。

| styled-compoennts | Emotion |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e5d09529-0b0b-38f1-2cd5-a9e1d17e90e2.gif) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d39fc8d3-dec2-fe14-f6a5-d40625dd3bc3.gif) |

実際のプロダクトで使う場合はどうすると良いのでしょうか。
この点は私の中に知見がないため、もし良いアイデアをお持ちの方がいればコメントいただけると非常に嬉しいです！