<!--
title:   アイコンフォントとしてMaterial Iconsの様々なスタイルを使うには
tags:    CSS,Design,HTML,material-icon,デザイン
id:      f1c2a841fff9da84629a
private: false
-->
## これは何

- Material Iconsを使うにあたってデフォルト以外の4スタイルを使おうと思うと、意外と公式ドキュメントが分かりづらいです
    - デフォルト以外の4スタイル = Outlined, Rounded, Sharp, Two tone
- これらを使う方法をまとめました
- ちなみに検証で使っていたコードはGitHubで公開しています
    - 記事で公開するように体裁だけ整えました

https://github.com/xrxoxcxox/qiita-material-icons-tips

## 結論

全てのスタイルを使おうと思うなら、以下のコードを書けばOKです。

```html:このlinkをHTMLで読み込む
<link
  href="https://fonts.googleapis.com/css?family=Material+Icons|Material+Icons+Outlined|Material+Icons+Round|Material+Icons+Sharp|Material+Icons+Two+Tone"
  rel="stylesheet"
/>
```

## 何が困ったのか

2021年3月2日にアップデート（？）されて、Google FontsがMaterial Iconsを扱うようになりました。

https://material.io/blog/google-fonts-material-icons

ページ上部に[Developer guide](https://developers.google.com/fonts/docs/material_icons)へのリンクがあるわけですが、`Icon font for the web`の章ではGoogle Fonts経由でアイコンとスタイルシートを読み込む方法が紹介されています。

```html
<link
  href="https://fonts.googleapis.com/icon?family=Material+Icons"
  rel="stylesheet"
>
```

そして、Google Fonts上では使いたいアイコンを選択するとコードスニペットを表示してくれます。
例えば`Two tone`なアイコンを選択するとクラス名には`material-icons-two-tone`を使いなさいとのことですが、**このままコピペしても動きません。**

![チェックアイコンを選択し、コードスニペットが表示されている画像](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ed4725f8-78fb-5a68-2f09-f18e42f97a9b.png)

どうすれば動くかというと、`link`の中身をこのように変える必要があります。

```diff_html:公式のコード
  <link
-   href="https://fonts.googleapis.com/icon?family=Material+Icons"
+   href="https://fonts.googleapis.com/icon?family=Material+Icons+Two+Tone"
    rel="stylesheet"
  >
```

スタイルにあわせて`href`を変える必要があるのが、私が見る限りDeveloper guide内で明言されておりませんでした。

（もしどこかに記載があった場合はすみません。私の確認不足です。）

## それぞれのスタイルを使用するためのリンク

### Filled（デフォルト、ドキュメント内で紹介されているもの）

```html
<link
  href="https://fonts.googleapis.com/icon?family=Material+Icons"
  rel="stylesheet"
>
```

### Outlined

```html
<link
  href="https://fonts.googleapis.com/icon?family=Material+Icons+Outlined"
  rel="stylesheet"
>
```

### Rounded

```html
<link
  href="https://fonts.googleapis.com/icon?family=Material+Icons+Round"
  rel="stylesheet"
>
```

### Two tone

```html
<link
  href="https://fonts.googleapis.com/icon?family=Material+Icons+Two+Tone"
  rel="stylesheet"
>
```

### 全てを使いたい場合

```html
<link
  href="https://fonts.googleapis.com/css?family=Material+Icons|Material+Icons+Outlined|Material+Icons+Round|Material+Icons+Sharp|Material+Icons+Two+Tone"
  rel="stylesheet"
/>
```

## 番外編：どのように発見したか

上手く表示できないため、本家のサイトでは一体どんなコードが書かれているんだろう……と思い開発ツールで確認してみました。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/648a87eb-4093-0a26-9e78-45a0929fab31.png)

すると上の画像にあるように、`material-icons-two-tone`のクラスを指定しているファイルが`css2?family=Material+Icons+Two+Tone`という名前なのが分かりました。

もしかしてと思い`href`もこれにあわせて書き換えたら上手く表示されたため、解決。