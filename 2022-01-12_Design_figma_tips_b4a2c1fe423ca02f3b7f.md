<!--
title:   Figmaのファイル名に日本語をつけると共有URLが長くて困る？実は短くできます
tags:    Design,figma,tips,デザイン,小ネタ
id:      b4a2c1fe423ca02f3b7f
private: false
-->
## 多分みんな経験していること

1. Figmaのファイル名に日本語をつける
1. Share > Copy link
1. 以下のような、とても長いURLが生成される
    1. `https://www.figma.com/file/abcdefghijklmnopqrstuv/%E6%97%A5%E6%9C%AC%E8%AA%9E%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E5%90%8D`
1. ドキュメントに載せるときに場所を食う、若干見づらい

## 解決策

上記の例で出したURLで言えば、以下のように削って大丈夫です。

`https://www.figma.com/file/abcdefghijklmnopqrstuv`

※`abcdefghijklmnopqrstuv`の部分はファイルによって違います。後述するfile keyというものです。

## 仕組み

まずはFigma APIを見てみます。

https://www.figma.com/developers/api#get-files-endpoint

> The file key can be parsed from any Figma file url: https://www.figma.com/file/:key/:title.

file keyを取得するための説明ですが、FigmaのファイルのURLの構造は上記のようになっているみたいです。

恐らく、識別子である`key`の後に`title`がついているのに過ぎないので、消してもちゃんとアクセスできる……ということだと思います。

## 何故気付いたのか

1. URLにファイル名がついているのは気付いていた（英語のファイル名はもちろんエンコードされないのでURLにそのまま反映されている）
1. ファイルのタイトルを変えた
1. 過去にドキュメントに載せていたURLから飛んでもちゃんとアクセスできる
1. 再度Copy linkをするとURL内のタイトルは変わっていた

てっきりファイルを新規作成した際の名前がURLとしてはずっと使われているのかと思ったのですが、そうではないらしいことに気がつきました。

「URLのうち、ファイル名に相当する部分を消したらどうなるんだろ」と思って消してみたら問題なくアクセスできたので、ほぼ偶然気づいたようなものです。