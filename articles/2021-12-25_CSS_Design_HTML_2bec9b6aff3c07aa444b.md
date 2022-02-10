<!--
title:   CSSで描く、少し理解しやすい吹き出しのしっぽ
tags:    CSS,Design,HTML,デザイン
id:      2bec9b6aff3c07aa444b
private: false
-->
[Qiita株式会社 Advent Calendar 2021](https://qiita.com/advent-calendar/2021/qiita)（2）の9日目の担当は、CX向上グループの@xrxoxcxoxです！

https://qiita.com/advent-calendar/2021/qiita

## この記事の概要

`CSS 吹き出し`で調べると大抵borderを使ったテクニックが出てきます
しかし、実際に描画される大きさがパッと理解しづらくないですか？

記述量は少し増えるものの、頭で理解しやすい吹き出しのしっぽの書き方を記事にしてみました。
`clip-path`を使うのがポイントです。

## 共通するスタイル

以下のHTML, CSSは全てに共通するので先に抜き出しておきます。

```html:HTML
<div class="base balloon">
  clip-pathを使った吹き出し1
</div>
```

`css:CSS
.base {
  background-color: #fff;
  border-radius: 1rem;
  font-weight: bold;
  line-height: 1;
  padding: 2rem;
  position: relative;
}

.base::after {
  content: "";
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
}
`

## パターン1：左右対称

| イメージ |
| --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1bbd42dc-2444-d2b7-64e7-aeb880538e74.png) |

よく紹介されているのは以下のようなコードです。

```css:CSS
.balloon::after {
  border: 1rem solid transparent;
  border-top: 1rem solid #fff;
}
```

`border`を全体に指定した上で`border-top`だけオーバーライドする……。
なんとも不思議なコードですね。

`clip-path`を使うとどうなるでしょう？

```css:CSS
.balloon::after {
  background-color: #fff;
  width: 2rem;
  height: 1rem;
  clip-path: polygon(0 0, 50% 100%, 100% 0);
}
```


幅が2rem、高さが1remの矩形の中に`0 0, 50% 100%, 100% 0`の座標で点が打たれている、と多少脳内でレンダリングしやすくありませんか？

## パターン2：片側に寄ってる

| 片側に寄ってる |
| --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/06c518bc-5593-6af9-3cc5-a93728bcd7f1.png) |

よく紹介されているのは以下のようなコードです。

```css:CSS
.balloon::after {
  border-top: 0.5rem solid #fff;
  border-right: 1rem solid transparent;
  border-bottom: 0.5rem solid transparent;
  border-left: 1rem solid #fff;
}
```

またなんとも不思議な感じがしますよね。

こちらも`clip-path`に置き換えてみましょう。

```css:CSS
.balloon::after {
  background-color: #fff;
  width: 2rem;
  height: 1rem;
  clip-path: polygon(0 0, 0% 100%, 100% 0);
}
```

先ほどとほぼ同様です。

幅が2rem、高さが1remの矩形の中に`0 0, 0% 100%, 100% 0`の座標で点が打たれています。

パターン1よりも更に`border`のテクニックが複雑になっているので`clip-path`のシンプルさが際立っている気がします。

## まとめ

- 吹き出しのしっぽをCSSで描くときはclip-pathを使うと
    - 記述量が少し増えるものの
    - デザインのモックアップを見たままコードに変換しやすい