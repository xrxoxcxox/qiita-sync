<!--
title:   SVGだけで作るノイズグラデーション
tags:    AdventCalendar2022,Design,SVG,デザイン
id:      2c95837c134ead32f591
private: true
-->
## この記事の概要

ノイズグラデーション、最近よく見かけますよね。
以下の画像のようなザラっとした質感のグラデーションです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/eccdb898-9801-e9cb-4b91-c41ef5f8da3c.png)

実はこれ、SVGだけで作れてしまいます。
というわけでその作り方を解説します。

## 最終形

細かい理屈は良いからとりあえず使いたい！という人は以下をコピペしていただければOKです。

### 中心からグラデーション

```xml
<svg
  width="200"
  height="200"
  viewBox="0 0 200 200"
  xmlns="http://www.w3.org/2000/svg">
  <filter id="displacementFilter">
    <feTurbulence
      type="fractalNoise"
      baseFrequency="0.8"
      numOctaves="3"
      result="fractalNoise" />
    <feDisplacementMap
      in2="fractalNoise"
      in="SourceGraphic"
      scale="150"
      xChannelSelector="R"
      yChannelSelector="G" />
  </filter>
  <mask id="mask">
    <circle
      cx="100"
      cy="100"
      r="100"
      style="
        fill: #FFF;
      "
    />
  </mask>

  <circle
    cx="100"
    cy="100"
    r="100"
    style="
      fill: #37BCE3;
    "
  />
  <circle
    cx="100"
    cy="100"
    r="100"
    style="
      fill: none;
      stroke: #003E96;
      stroke-width: 40;
      filter: url(#displacementFilter);
    "
    mask="url(#mask)"
  />
</svg>
```

### 端からグラデーション

```xml
<svg
  width="200"
  height="200"
  viewBox="0 0 200 200"
  xmlns="http://www.w3.org/2000/svg">
  <filter id="displacementFilter">
    <feTurbulence
      type="fractalNoise"
      baseFrequency="0.8"
      numOctaves="3"
      result="fractalNoise" />
    <feDisplacementMap
      in2="fractalNoise"
      in="SourceGraphic"
      scale="300"
      xChannelSelector="R"
      yChannelSelector="G" />
  </filter>
  <mask id="mask">
    <circle
      cx="100"
      cy="100"
      r="100"
      style="
        fill: #FFF;
      "
    />
  </mask>

  <circle
    cx="100"
    cy="100"
    r="100"
    style="
      fill: #37BCE3;
    "
  />
  <circle
    cx="-25"
    cy="225"
    r="200"
    style="
      fill: #003E96;
      filter: url(#displacementFilter);
    "
    mask="url(#mask)"
  />
</svg>
```

## 細かな解説

大きく分けて4ステップあります。

1. ノイズのもとになる図形を描く
2. ノイズをかける
3. マスクをかける
4. 背景色を追加する

### ノイズのもとになる図形を描く

最初のステップは簡単です。
単に円を描いているだけです。

```xml
<svg
  width="200"
  height="200"
  viewBox="0 0 200 200"
  xmlns="http://www.w3.org/2000/svg">
  <circle
    cx="100"
    cy="100"
    r="100"
    style="
      fill: none;
      stroke: #003E96;
      stroke-width: 20;
    "
  />
</svg>
```

上記のコードにより、以下のような結果が得られます。
`stroke`の仕組み上`viewBox`からはみ出てしまっていますが、ここでは問題にならないのでそのままにしておきます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bc3c31b4-36cf-e128-88dd-73b1f39044dd.png)

### ノイズをかける

次にノイズをかけます。

```xml
<svg
  width="200"
  height="200"
  viewBox="0 0 200 200"
  xmlns="http://www.w3.org/2000/svg">
  <filter id="displacementFilter">
    <feTurbulence
      type="fractalNoise"
      baseFrequency="0.8"
      numOctaves="3"
      result="fractalNoise" />
    <feDisplacementMap
      in2="fractalNoise"
      in="SourceGraphic"
      scale="150"
      xChannelSelector="R"
      yChannelSelector="G" />
  </filter>

  <circle
    cx="100"
    cy="100"
    r="100"
    style="
      fill: none;
      stroke: #003E96;
      stroke-width: 40;
      filter: url(#displacementFilter);
    "
  />
</svg>
```

現在の見た目はこちらです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/41d6d702-a286-6952-ed97-d5238f7d1b19.png)

全体的な流れとしては以下です。

1. idつきで`filter`を用意する
2. `filter`内の`feTurbulence`により、ノイズの種類や細かさを指定する
3. `filter`内の`feDisplacementMap`で`feTurbulence`を受け取り、`filter`をかける画像にどのようにかけるかを指定する
4. `filter`をかけたい要素（今回なら`circle`）の`style`に`filter: url(#filter-id)`の形式で指定する

それぞれのパラメーターの説明を細かくしようと思うとかなり長くなるのでざっくりとだけ説明します。

- `feTurbulence`
  - `type`
    - `fractalNoise`と`turbulence`がある
    - 今回でいうと、座標計算が`fractalNoise`の方がやりやすかったのでこちらを選択
  - `baseFrequency`
    - ノイズの細かさで、数値が大きいほど細かいノイズになる
  - `numOctaves`
    - ノイズの複雑さで、数値が大きいほど複雑なノイズになる
  - `result`
    - `feDisplacementMap`に渡す名前の指定
- `feDisplacementMap`
  - `in`
    - 色々な種類の指定があるけど、割と毎回`SourceGraphic`で良いような気がしているので省略
  - `in2`
    - `feTurbulence`から受け取る名前
  - `scale`
    - 大きいほどかかり方が強くなる（？）
      - これの理解が少し怪しいので、詳しい方がいたら是非コメントで教えてください
  - `xChannelSelector` | `yChannelSelector`
    - x軸とy軸どちらの座標にどのチャンネルを用いるか
    - どちらも同じものを選ばなければ良し、くらいな気がしている

MDN Docsには詳しく載っているので、より詳細に知りたい方はこちらをどうぞ。

https://developer.mozilla.org/en-US/docs/Web/SVG/Element/feTurbulence

https://developer.mozilla.org/en-US/docs/Web/SVG/Element/feDisplacementMap

### マスクをかける

ノイズをかけたらかなり広がってしまったので、本来描画したい範囲だけにマスクをかけます。

```xml
<svg
  width="200"
  height="200"
  viewBox="0 0 200 200"
  xmlns="http://www.w3.org/2000/svg">
  <filter id="displacementFilter">
    <feTurbulence
      type="fractalNoise"
      baseFrequency="0.8"
      numOctaves="3"
      result="fractalNoise" />
    <feDisplacementMap
      in2="fractalNoise"
      in="SourceGraphic"
      scale="150"
      xChannelSelector="R"
      yChannelSelector="G" />
  </filter>
  <mask id="mask">
    <circle
      cx="100"
      cy="100"
      r="100"
      style="
        fill: #FFF;
      "
    />
  </mask>

  <circle
    cx="100"
    cy="100"
    r="100"
    style="
      fill: none;
      stroke: #003E96;
      stroke-width: 40;
      filter: url(#displacementFilter);
    "
    mask="url(#mask)"
  />
</svg>
```

現在の見た目はこちらです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9fb52f4c-dbff-1526-fd1e-1d99648db82b.png)

ここはグラデーション自体には関係していないので省略します。

### 背景色を追加する

最後に、背景色を追加します。

```xml
<svg
  width="200"
  height="200"
  viewBox="0 0 200 200"
  xmlns="http://www.w3.org/2000/svg">
  <filter id="displacementFilter">
    <feTurbulence
      type="fractalNoise"
      baseFrequency="0.8"
      numOctaves="3"
      result="fractalNoise" />
    <feDisplacementMap
      in2="fractalNoise"
      in="SourceGraphic"
      scale="150"
      xChannelSelector="R"
      yChannelSelector="G" />
  </filter>
  <mask id="mask">
    <circle
      cx="100"
      cy="100"
      r="100"
      style="
        fill: #FFF;
      "
    />
  </mask>

  <circle
    cx="100"
    cy="100"
    r="100"
    style="
      fill: #37BCE3;
    "
  />
  <circle
    cx="100"
    cy="100"
    r="100"
    style="
      fill: none;
      stroke: #003E96;
      stroke-width: 40;
      filter: url(#displacementFilter);
    "
    mask="url(#mask)"
  />
</svg>
```

現在の見た目はこちらです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/cc60021d-03db-7ebc-f804-8c67cead0bb3.png)

こちらも、単に同じ色を配置しただけなので説明は省略します。

## 端から始まる方のグラデーション

実はコードはほとんど変わっていません。
違いは以下の2点だけです。

- strokeにノイズをかけるか、fillにノイズをかけるか
- ノイズをかけた要素をどこに配置するか

そのため、ノイズをかける図形を変えたりサイズを変えればもっと色々な種類が作れます。

## 最後に

ノイズグラデーションは格好いいので好きなのですが、画像で用意するしかないとなると取り回しがしづらいなあ……と思っていました。
ところが結構簡単にSVGだけで実装できたので、軽いし色を変えるのも簡単だしで、結構良いものが作れた気がします。

あとは`symbol`や`use`を使えばコードとしてももう少し綺麗にできるかもしれません。
そちらも試してみたら記事にしようと思います。

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox

Devトークでのお話してくださる方も募集中です！

https://jobs.qiita.com/employers/qiita-inc/dev_talks/489