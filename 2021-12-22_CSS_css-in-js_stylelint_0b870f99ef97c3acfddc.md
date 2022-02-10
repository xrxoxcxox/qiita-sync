<!--
title:   stylelint-orderはobject styleだと動かない
tags:    CSS,css-in-js,stylelint
id:      0b870f99ef97c3acfddc
private: false
-->
## この記事の概要

[Stylelint](https://stylelint.io/)や、そのプラグインである[stylelint-order](https://github.com/hudochenkov/stylelint-order)をCSS in JSで使う際、object styleで書くと並び変えが上手くいきません。

ちなみに、object styleとは以下のような記載の仕方を指します。

```javascript
const style = css({
  backgroundColor: '#55c500',
  color: '#fff',
  fontSize: 20,
  padding: '16px 24px',
})
```

名前の通りですが、文字列としてスタイルを記述するのではなくオブジェクトとして記述するやり方ですね。

## 伝えたいこと

1番伝えたいのはCSS in JSでstylelint-orderが動かないこと自体ではありません。

「object styleであることが大事だから、orderは諦めよう」とか「orderの方が大事だからstring styleにしよう」とか話し合うきっかけになれば幸いで、そのために記事にまとめようと思いました。

## 背景

以前私が関わっているプロジェクトにてｍCSS in JSの方針を議論してこんな着地をしたことがあります。

- 使うライブラリ
    - Emotion
        - 他で実績があるのと、私がある程度詳しいため
- 書き方
    - object style
        - せっかくJavaScriptの世界で書いているのだから、1つの長い文字列よりもデータとして取り回しがしやすいはずのオブジェクトを使おう
- プロパティ順
    - アルファベット順
        - プロパティ順に特にこだわりはないものの、無秩序なのも……という感情だけ

さて、pre-commit時にstylelint-orderが走るようにセッティングしてしばらく運用していましたが、ある日気付きました。

「**プロパティ、全く並び変られてないぞ？**」

タイトルの通りですがstylelint-orderはobject styleでは動かないのですが、私は手でスタイルを書く時点でアルファベット順に書く癖があったのでstylelintが動いていないのに気付いていませんでした。

しかし誰かが新しいスタイルを書いた際、既存のコードの後ろにスタイルを追加していたので以下のような感じになっており、そのままでPull Requestが出ていたので気付いたのです。

```javascript
const style = css({
  width: '100%',
  margin: 16,
})
```

## 公式の見解

2019年と随分古いですが、Issueが立っていました。

https://github.com/hudochenkov/stylelint-order/issues/99

ここに書いてある通りですが、最後のカンマが状態で順番を入れ替えるとエラーになってしまうためだそうです。

```
{
  a: 0,
  c: 2,
  b: 1
}
```

`
{
  a: 0,
  b: 1
  c: 2,
}
`

JavaScriptの文法を修正するのは、このプラグインの範囲外なので対応するつもりはないとのことでした。

## まとめ

- stylelint-orderはobject styleだと動かない
- チームの方針が……
    - object styleでスタイルを書くことが優先なら、CSSのプロパティ順を自動で並び変えるのは諦める（or stylelint-orderではない手段を考える）
    - プロパティ順にルールを持たせることが優先なら、object styleではなくstring styleで書く