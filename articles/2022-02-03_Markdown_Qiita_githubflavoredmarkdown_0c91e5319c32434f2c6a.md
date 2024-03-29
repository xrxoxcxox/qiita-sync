<!--
title:   QiitaのMarkdownパーサー変更にともなう、補足説明の書き方と注意点
tags:    Markdown,Qiita,GitHubFlavoredMarkdown
id:      0c91e5319c32434f2c6a
private: false
-->
## 記事投稿シリーズについて

QiitaでGitHub Flavored Markdown (以下GFM) に準拠したMarkdownパーサーが[ベータ版として公開されました！](https://blog.qiita.com/replace-markdown-parser-beta/)

[Qiitaアカウントによる説明記事](https://qiita.com/Qiita/items/e87ebe3138f62b0e8b58)にも書いてあるのですが、今までと記法が変わっている箇所があるので注意が必要です。
せっかく書いた記事の表示が崩れてしまうのは困るので、可能な限り防ぎたいと考えています！

そこで、記事を書く時に注意すべき記法などについて、Qiita運営から5日間に分けて10記事のシリーズで投稿することになりました！
少しでも皆様が執筆する際の助けになればと思います。

詳しくはQiitaアカウントによる説明記事に、投稿シリーズの説明や、他の記事一覧が載っていますのでぜひ他の記事も読んでみてください！

https://qiita.com/Qiita/items/e87ebe3138f62b0e8b58

GFMの記法とは少し違いますが、この記事ではQiita独自の「補足説明のための記法（以下ノート記法）」について説明します。

## ノート記法の説明

[2021年7月21日にリリースしたQiita独自の記法](https://qiita.com/release-notes#%E3%83%8E%E3%83%BC%E3%83%88%E8%A8%98%E6%B3%95%E3%82%92%E8%BF%BD%E5%8A%A0%E3%81%97%E3%81%BE%E3%81%97%E3%81%9F)です。

目を引く形で補足説明をしたい場合、補足したい内容を`:::note info`と`:::`で囲みます。
補足したい内容と`:::note info`と`:::`はそれぞれ別の行にする必要があります。
noteの後のinfoは省略可能です。
また、 noteの後に`warn`をつけると警告メッセージとして、`alert`をつけるとより強い警告メッセージとして表現することができます。

```
:::note info
インフォメーション
infoは省略可能です。
:::

:::note warn
警告
○○に注意してください。
:::

:::note alert
より強い警告
○○しないでください。
:::
```

:::note info
インフォメーション
infoは省略可能です。
:::

:::note warn
警告
○○に注意してください。
:::

:::note alert
より強い警告
○○しないでください。
:::

## パーサー変更にともなうアップデート

ノート記法の中で一部のマークダウンが使えるようになりました！

これまではプレーンなテキストと絵文字しか扱えませんでしたが、今回のアップデートにより以下をご使用いただけます！

- リンク
- 箇条書き
- コード
- 太字
- 斜体
- 打ち消し線
- 画像

実際にノート記法の中で使ってみるとこのように見えます。

:::note
[リンク](#リンク)

1. 順序有りの箇条書き
1. 順序有りの箇条書き
1. 順序有りの箇条書き

- 順序無しの箇条書き
- 順序無しの箇条書き
- 順序無しの箇条書き

`code`

**太字**

*斜体*

~~打ち消し線~~

画像↓
![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/064c6d3f-f241-8ff9-1a78-46a5100ebed7.png)
:::

正確に言えば他のMarkdownの書き方も使えるのですが、現在は表示が崩れたり余白が空きすぎたりしてしまいます。
こちらは順次アップデートしていきますのでお待ちください :bow:

## 最後に

記事投稿シリーズはこちらの記事をもって完了です。

全て読んでいただいた方、ありがとうございます！
まだの方は、他の記法についても[Qiitaアカウントによる説明記事](https://qiita.com/Qiita/items/e87ebe3138f62b0e8b58)でまとめていますので、合わせて読んでみてください！

また、Qiitaは今後も改善を続けていきます。
その中ではエディタの使い勝手向上や機能の追加も予定しており、皆さまに心地よく書いていただけるようにアップデートしていきます！

エディタを使ってみての感想も含め、[Qiita Discussions](https://github.com/increments/qiita-discussions/discussions)でいつでもお待ちしています。
ご意見がありましたら是非お寄せください！

---

↓記事投稿シリーズまとめ

https://qiita.com/Qiita/items/e87ebe3138f62b0e8b58

↓前の記事

https://qiita.com/mziyut/items/bc00ed7396bd60a717b8