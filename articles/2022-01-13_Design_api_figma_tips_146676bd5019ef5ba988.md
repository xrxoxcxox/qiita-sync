<!--
title:   Figma APIで特定File内のFrame名を全てcsvに出力する
tags:    Design,api,figma,tips,デザイン
id:      146676bd5019ef5ba988
private: false
-->
## この記事の概要

「Figmaで特定のFileのFrame名を一気にCSVに書き出すことはできないか？」という質問をもらいました。
この要望を叶えるための記事です。

そして、@hiloki@githubさんがJavaScriptで取得するバージョンの記事を書いていたので、こちらもどうぞ。

https://qiita.com/hiloki@github/items/d6f51c0fc65449ea1fad

## 方針

- Figma APIを使う
- 1つのFile内の全てのPageのルートにあるFrame名を全て取得する
- csvファイルとして保存する

## 準備

### Figmaのアクセストークンを取得する

`Settings > Account > Personal access tokens`

より、名前をつけてトークンを発行します。
忘れないようにメモしておいてください。

### `jq`のインストール

この後ターミナルでコマンドを叩くのですが、整形する用に`jq`を使います。

```shell
brew install jq
```

## コマンド

以下のコマンドを叩けば取得できます。

```shell
curl -H 'X-FIGMA-TOKEN: YOUR_FIGMA_TOKEN' 'https://api.figma.com/v1/files/YOUR_FILE_KEY?depth=2' \
| jq -r '.document.children[] | [.name, .children[].name] | @csv' \
| > /path/to/directory/any-name.csv
```

## コマンド内のあれこれを解説

### YOUR_FIGMA_TOKEN

`Figmaのアクセストークンを取得する`で取得したトークンです。

### https://api.figma.com/v1/files

Fileの情報を取得するためのエンドポイントです。

https://www.figma.com/developers/api#get-files-endpoint

### YOUR_FILE_KEY

各Fileの共有URLをコピーすると、以下のような書式になっています。

`https://www.figma.com/file/:key/:title`

この`key`をコマンドの中に適用します。

### depth=2

もう一度先ほどのURLを貼っておきます。

https://www.figma.com/developers/api#get-files-endpoint

この中の`depth`の項目を引用。

> Positive integer representing how deep into the document tree to traverse. For example, setting this to 1 returns only Pages, setting it to 2 returns Pages and all top level objects on each page. Not setting this parameter returns all nodes

書いてあるように`2`を指定すると、ページと各ページのすべてのトップレベルオブジェクトが返されます。
`1`だとPage名までしか取れないので、`2`を指定しました。

### .document.children[]

返ってくる結果のうち、トップレベルに`thumbnailUrl`や`role`など今回は不要な情報が混ざっています。
実際のデザインデータの情報だけを取得したいため、こちらを指定します。

### [.name, .children[].name]

最初の`.name`がPageの名前。
`.children[].name`が全てのトップレベルの要素の名前です。

`.children[].name`だけだとどのPageのFrameだったかが分かりづらくなってしまうかなと思い、先頭にPage名を入れておきました。

### @ csv

jsonをcsvに変換します。

### > /path/to/directory/any-name.csv

出力結果を任意のディレクトリに保存しているだけです。