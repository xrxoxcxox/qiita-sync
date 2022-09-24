<!--
title:   Reactにて、CSSファイルもCSS in JSライブラリも無しにスタイリングできないか実験し、失敗した記録
tags:    CSS,React,css-in-js
id:      e24ec739db993bd41ec1
private: false
-->

## この記事の概要

私が日頃Reactを触っている際、CSS in JSをよく使います。

一口にCSS in JSと言っても、書き味やバンドルサイズなどそれぞれ違いがあって、選定に悩むこともしばしば。
要はJavaScriptの中でCSSが書ければ良いのだから、別な方法もあるのではないか？と思って実験した内容をまとめました。

この記事自体は役に立つものではないと思いますが、同じ轍を踏む人が減れば良いなと思って投稿します。

## 結論

動くっちゃ動くけど明らかに実用的でない。
（測定すらしていませんが、するまでもなくこれは駄目だと思いました。）

## 試したこと

1. コンポーネント内に`<style>`を埋め込む
1. コンポーネント内に`react-helmet-async`を用いて`<style>`を埋め込む
1. `useId`を用いてユニークなクラス名を生成しようとする
1. `nanoid`を用いてユニークなクラス名を生成する

### コンポーネント内に`<style>`を埋め込む

`<style>`とてelementですから、埋め込めないわけはありません。

```jsx:App.js
import { Component1 } from "./Component1";

export default function App() {
  return (
    <Component1 />
  );
}
```

```jsx:Component1.js
export const Component1 = () => {
  return (
    <>
      <style>
        {`
          .component1 {
            color: red;
          }
        `}
      </style>
      <p className="component1">This is component 1.</p>
    </>
  );
};
```

```html:出力されたコード
<div id="root">
  <style>
    .component1 {
      color: red;
    }
  </style>
  <p class="component1">This is component 1.</p>
</div>
```

当然でしたが、HTMLの中に`<style>`が入っています。
なぜか勝手に`<head>`の中に`<style>`を挿入してくれるつもりでいたので、途方もなく間違えていました。

### コンポーネント内に`react-helmet-async`を用いて`<style>`を埋め込む

タイトルやOGPなどを書き換えるときに使われがちな`react-helmet-async`ですが、`<head>`に`<style>`を挿入する用途で使ってみます。

かつては`react-helmet`をよく見た気がしますが、`componentWillMount`に関するwarningが出たり、アップデートが止まっていたりで、今では`react-helmet-async`に取って代わられた感があります。

```jsx:App.js
import { HelmetProvider } from "react-helmet-async";

export default function App() {
  return (
    <HelmetProvider>
      <Component1 />
    </HelmetProvider>
  );
}

```

```jsx:Component1.js
import { Helmet } from "react-helmet-async";

export const Component1 = () => {
  return (
    <>
      <Helmet>
        <style>
          {`
            .component1 {
              color: red;
            }
          `}
        </style>
      </Helmet>
      <p className="component1">This is component 1.</p>
    </>
  );
};
```

```html:出力されたコード
<head>
  <style data-rh="true">
    .component1 {
      color: red;
    }
  </style>
</head>
<body>
  <div id="root">
    <p class="component1">This is component 1.</p>
  </div>
</body>
```

ひとまず良さそうです。

ただ、今はクラス名を固定の文字列で指定しているので衝突の可能性があります。
CSS in JSではクラス名被りを回避してくれるわけですが、そこを再現しないとお話になりません。

### `useId`を用いてユニークなクラス名を生成しようとする

React 18から搭載された`useId`を使えばライブラリ要らずかと思っていましたが、一瞬で打ち砕かれました。

```jsx:Component1.js
import { useId } from "react";
import { Helmet } from "react-helmet-async";

export const Component1 = () => {
  const id = useId();
  return (
    <>
      <Helmet>
        <style>
          {`
            .class-${id} {
              color: red;
            }
          `}
        </style>
      </Helmet>
      <p className={`class-${id}`}>This is component 1.</p>
    </>
  );
};
```

`useId`を使うと`:r1:`のような書式のIDが生成されます。
CSSのクラス名は（エスケープ無しには）`:`の文字が使えないので、スタイルを書いても無効になってしまいました。

IDが生成された後で`:`を`\:`に書き換えるようなコードを書けば行けそうな気もしますが、負債になりそうな気配がします。
あと、そもそもReactのページに以下のように書いてありました。

> useId generates a string that includes the : token. This helps ensure that the token is unique, but is not supported in CSS selectors or APIs like querySelectorAll.

使用をやめておきます。

### `nanoid`を用いてユニークなクラス名を生成する

バンドルサイズが小さめのものが良かったので、`nanoid`を選びました。

```jsx:Component1.js
import { Helmet } from "react-helmet-async";
import { nanoid } from "nanoid";

export const Component1 = () => {
  const id = nanoid();
  return (
    <>
      <Helmet>
        <style>
          {`
            .class-${id} {
              color: red;
            }
          `}
        </style>
      </Helmet>
      <p className={`class-${id}`}>This is component 1.</p>
    </>
  );
};
```

```html:出力されたコード
<head>
  <style data-rh="true">
    .class-OX16EEr6Ok45TFzdAB0Ef {
      color: red;
    }
  </style>
</head>
<body>
  <div id="root">
    <p class="class-OX16EEr6Ok45TFzdAB0Ef">This is component 1.</p>
  </div>
</body>
```

どことなく良さそうな気配があります。
しかし、コンポーネントが1つだけ、1種類だけというのはあり得ません。
いくらか増やします。

```jsx:App.js
import { HelmetProvider } from "react-helmet-async";
import { Component1 } from "./Component1";
import { Component2 } from "./Component2";

export default function App() {
  return (
    <HelmetProvider>
      <Component1 />
      <Component1 />
      <Component2 />
    </HelmetProvider>
  );
}
```

```html:出力されたコード
<head>
  <style data-rh="true">
    .class-OX16EEr6Ok45TFzdAB0Ef {
      color: red;
    }
  </style>
  <style data-rh="true">
    .class-GDwDl8eKx5PafHuUXw4oR {
      color: red;
    }
  </style>
  <style data-rh="true">
    .class-Lavssca60UD3PGzSTU1Fs {
      color: blue;
    }
  </style>
</head>
<body>
  <div id="root">
    <p class="class-OX16EEr6Ok45TFzdAB0Ef">This is component 1.</p>
    <p class="class-GDwDl8eKx5PafHuUXw4oR">This is component 1.</p>
    <p class="class-Lavssca60UD3PGzSTU1Fs">This is component 2.</p>
  </div>
</body>
```

同じコンポーネントであっても重複して`<style>`に登録されていますし、1つずつのコンポーネントのスタイルが`<style>`として差し込まれているので効率が悪そうです。
コンポーネント内で`nanoid`と`Helmet`を使っているのでよく考えれば試すまでもなくこうなりますね。

あとはテンプレートリテラルで無理矢理書いているので補完も効かず、書き心地も良くありません。
そんなこんなで、世間で出回っているライブラリの便利さを実感して手を止めてしまいました。

実験はここで終了です。

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox