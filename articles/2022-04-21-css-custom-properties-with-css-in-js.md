<!--
title:   Visual Studio CodeのExtensionで、CSS in JSでもCSS Custom Propertiesを補完させる
tags:    VisualStudioCode, css-in-js, CSSCustomProperties, tips, 新人プログラマ応援 
-->
## この記事の概要

私は最近CSS in JSでスタイルを書くことが多いのですが、CSS Custom Properties（CSS変数）が補完されず微妙に非効率な思いをしていました。
これを解消できるVisual Studio CodeのExtensionを発見したので、簡単な使い方とともに紹介します。

また、`新人プログラマ応援 - みんなで新人を育てよう！`イベントへの参加記事でもあります。

https://qiita.com/official-events/3f21c92121aa125807b4

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
