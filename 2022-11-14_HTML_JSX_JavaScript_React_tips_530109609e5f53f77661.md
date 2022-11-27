<!--
title:   JSXの中で文字列を展開するときはテンプレートリテラルを使う方が綺麗なHTMLになる
tags:    HTML,JSX,JavaScript,React,tips
id:      530109609e5f53f77661
private: true
-->
## この記事の概要

JSXを書いていてこんな感じの文字列の展開、していませんか？

```jsx
export const Text = () => {
  const text = "foo";
  return <p>{text} bar</p>;
}
```

基本的に問題は無いんですが、最終的なHTMLを綺麗にするならテンプレートリテラルにした方が良いので記事にしました。

## 現状

先ほどの内容をDev toolsで見てみましょう。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f04b8b91-f28c-1408-a32b-e3c6252fa437.png)

描画されたHTMLは次のようになっています。

```html
<p>
  "foo"
  " bar"
</p>
```

何が起きているかというと、プレーンなテキストとJavaScriptを展開したテキストとが、別のテキストノードとして指定されてしまっています。

## 綺麗にする書き方

ひとまとめにしたい文字列を`{``}`で括り、その中で展開するだけです。

```diff_jsx
  export const Text = () => {
    const text = "foo";
    return (
-     <p>{text} bar</p>
+     <p>{`${text} bar`}</p>
    );
  }
```

Dev toolsで見てみましょう。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/6134e419-7bef-7107-7fd9-cc1247a7b5f1.png)

描画されたHTMLは次のようになっていて、整っています。

```html
<p>foo bar</p>
```

## おまけ

JSXでなく通常のJavaScriptの場合は`normalize()`をかけるだけで綺麗になります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/43e21737-59ec-5dfc-9997-1b1ef8399d96.png)

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox

Devトークでのお話してくださる方も募集中です！

https://jobs.qiita.com/employers/qiita-inc/dev_talks/489