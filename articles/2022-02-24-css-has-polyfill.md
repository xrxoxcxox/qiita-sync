<!--
title: CSSの:has()のポリフィルがあったので試してみた
tags: CSS,polyfill
-->

## この記事の概要

CSS を書いていて「`.child`クラスを持っている要素だけスタイルを変えたいんだよなあ」なんてシーン、ありませんか？
私はしょっちゅうあります！

Selectors Level 4 では`:has()`という疑似クラスが定義されていて上記の夢を叶えてくれるのですが、Can I use...を見るとほぼ全滅です。
2022年2月24日時点の結果は以下をご覧ください。

<picture>
  <source type="image/webp" srcset="https://caniuse.bitsofco.de/static/v1/css-has-1645699122582.webp">
  <source type="image/png" srcset="https://caniuse.bitsofco.de/static/v1/css-has-1645699122582.png">
  <img src="https://caniuse.bitsofco.de/static/v1/css-has-1645699122582.jpg" alt="Data on support for the css-has feature across the major browsers from caniuse.com">
</picture>

そんな`:has()`ですが、ポリフィルを見つけたので試しに使ってみた記事です。

https://github.com/csstools/postcss-plugins

## 使い方

といっても公式ドキュメントに書いてあることぐらいしかやっていません。
一応実験してみたリポジトリを公開しているので、以下を触っていただいてもOKです。

https://github.com/xrxoxcxox/qiita-css-has-polyfill

### 準備

以下のようなHTMLとCSSを用意しました。（話を簡単にするために一部を省略しています）
トライしていることは以下の2点だけです。

- `h2`の色を`#55c500`にする
- ただし、`span`を持つ`h2`は`#4097db`にする

```html:index.html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css">
    <title>:has() polyfill</title>
  </head>
  <body>
    <div class="container">
      <h1>:has() polyfill test</h1>
      <div class="contents">
        <h2>
          I have <span>span element</span>.
        </h2>
        <h2>
          I don't have span element.
        </h2>
      </div>
    </div>
  </body>
</html>
```

```css:style.css
h2 {
  color: #55c500;
}

h2:has(span) {
  color: #4097db;
}
```

なお、現段階ではどちらの`h2`も`#55c500`のままですが、これであっています。

![](../images/css-has-polyfill-01.png)

### 実際に動かす

ひとまずテストなので`npx`で実行します。

```bash:ターミナル
npx css-has-pseudo style.css --output dist.css
```

すると`dist.css`が生成されます。
元のCSSから変更のあった箇所だけを抜粋すると、以下のようになります。

```diff_css:style.cssとdist.cssの差分
  h2 {
    color: #55c500;
  }

+ h2[\:has\(span\)] {
+   color: #4097db;
+ }

  h2:has(span) {
    color: #4097db;
  }

+ /*# sourceMappingURL=data:application/json;base64,以下は省略 */
```

次にHTMLを編集します。

```diff_html:index.html
  <!DOCTYPE html>
  <html lang="ja">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
-     <link rel="stylesheet" href="style.css">
+     <link rel="stylesheet" href="dist.css">
+     <script src="https://unpkg.com/css-has-pseudo/dist/browser-global.js"></script>
+     <script>cssHasPseudo(document)</script>
      <title>:has() polyfill</title>
    </head>
    <body>
      <!-- 省略 -->
    </body>
  </html>
```

そして何かしらサーバーを立ち上げてHTMLを確認してみます。

するとどうでしょう。見事`span`を子要素に持つ`h2`だけが`#4097db`に変わりました。

![](../images/css-has-polyfill-02.png)

## 起きていること

サーバーを立ち上げた後のHTMLを見てみると、元のコードから変わっている箇所があります。

```diff_html:index.html
  <body>
    <div class="container">
      <h1>:has() polyfill test</h1>
      <div class="contents">
-       <h2>
+       <h2 :has(span)="">
          I have <span>span element</span>.
        </h2>
        <h2>
          I don't have span element.
        </h2>
      </div>
    </div>
  </body>
```

そして先ほどのCSSの差分をもう一度見てみましょう。

```diff_css:再掲、style.cssとdist.cssの差分
  h2 {
    color: #55c500;
  }

+ h2[\:has\(span\)] {
+   color: #4097db;
+ }

  h2:has(span) {
    color: #4097db;
  }

+ /*# sourceMappingURL=data:application/json;base64,以下は省略 */
```

`span`を持っている`h2`だけを探して`:has(span)`というattributeを付与し、それをセレクターとして活用しているようです。

Attribute selector自体はどのブラウザでも動きますから、安心して使えそうです。

余談ですが、`title`とか`href`とか、ちゃんと存在するattributeでなくてもセレクターとして使えるのを初めて知りました。

## 実践投入できる？

内容自体は良いかなと思ったのですが、コマンドに`--watch`のようなオプションが見当たりませんでした。
変更する度にコマンドを叩くのは流石に骨が折れますし、他との兼ね合い（Sassのコンパイルなど）もあってどうしようか悩み中。

今の私の力だけでは難しそうでしたが、上手いことやればできそうな気もする……という不完全燃焼な記載をここに残します。

なお、調べたついでに存在するオプションを一覧にしておきます。

```
-o, --output          Output file
-d, --dir             Output directory
-r, --replace         Replace (overwrite) the input file
-m, --map             Create an external sourcemap
--no-map              Disable the default inline sourcemaps
-p, --plugin-options  Stringified JSON object with plugin options
```
