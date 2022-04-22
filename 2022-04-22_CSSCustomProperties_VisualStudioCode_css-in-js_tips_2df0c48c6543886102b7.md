<!--
title:   Visual Studio CodeのExtensionで、CSS in JSでもCSS Custom Propertiesを補完させる
tags:    CSSCustomProperties,VisualStudioCode,css-in-js,tips,新人プログラマ応援
id:      2df0c48c6543886102b7
private: false
-->
## この記事の概要

私は最近CSS in JSでスタイルを書くことが多いのですが、CSS Custom Properties（CSS変数）が補完されず微妙に非効率な思いをしていました。
これを解消できるVisual Studio CodeのExtensionを発見したので、簡単な使い方とともに紹介します。

また、`新人プログラマ応援 - みんなで新人を育てよう！`イベントへの参加記事でもあります。

https://qiita.com/official-events/3f21c92121aa125807b4

## 最終イメージ

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/67f51e75-47e1-9741-75d5-28a0e62938ca.png)

CSS Custom Propertiesを定義しているのも、それを呼び出しているのもTypeScriptのファイルです。
しかし、ちゃんと補完が効いています。

## Extension

こちらのExtensionです。

https://marketplace.visualstudio.com/items?itemName=vunguyentuan.vscode-css-variables

似たようなExtensionはいくつかありましたが、`.css`のファイルを触っている間しか補完が効かないなど……。
`CSS Custom Properties`や`CSS Variable`などで検索してヒットするものをひとしきり試した中ではこのExtensionが1番求めるものに近かったです。

### デフォルト設定

上記のドキュメントにある通りですが、デフォルトでは以下のファイルを対象にして補完されます。

```
[
  "**/*.css",
  "**/*.scss",
  "**/*.sass",
  "**/*.less"
]
```

また、以下のものは除外されています。

```
[
  "**/.git",
  "**/.svn",
  "**/.hg",
  "**/CVS",
  "**/.DS_Store",
  "**/.git",
  "**/node_modules",
  "**/bower_components",
  "**/tmp",
  "**/dist",
  "**/tests"
]
```

### `.js`や`.ts`内でCustom Propertiesを定義する際

styled-componentsの`createGlobalStyle`やEmotionの`Global`を使用している場合、`.js`や`.ts`のファイル内でCustom Propertiesを定義することもあると思います。

この場合、Visual Studio Codeの`settings.json`を開いて、以下のように記載します。

```json-doc
{
  // 既存の設定
  "cssVariables.lookupFiles": [
    "**/*.css",
    "**/*.scss",
    "**/*.sass",
    "**/*.less",
    "**/*.js",
    "**/*.ts"
  ],
}
```

グローバルな`settings.json`ではなく、ワークスペース固有の`settings.json`に対して指定するのももちろんOKです。

```json-doc
{
  // 既存の設定
  "cssVariables.lookupFiles": [
    "path/to/styles.js",
    "path/to/styles.ts"
  ],
}
```