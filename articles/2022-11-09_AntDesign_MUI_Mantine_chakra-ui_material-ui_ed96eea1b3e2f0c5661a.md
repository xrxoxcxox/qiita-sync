<!--
title:   色々なUIライブラリのモジュール設定を調べた
tags:    AntDesign,MUI,Mantine,chakra-ui,material-ui
id:      ed96eea1b3e2f0c5661a
private: true
-->
## この記事の概要

色々なUIライブラリを調べている中で、意外とES Modules対応の仕方やディレクトリ構成がバラバラなのに気がつきました。
自分の備忘録も含めて記事にしました。

## 調べたライブラリ

- [MUI](https://mui.com/)（旧 Material-UI）
- [Ant Design](https://ant.design/)
- [Chakra UI](https://chakra-ui.com/)
- [Mantine](https://mantine.dev/)

どのパッケージもDual Packageで、CommonJS + faux ESMの構成でした。

### @mui/material

記事執筆時のバージョンは`5.10.13`です。
概観は以下のようになっていました。

```shell:ディレクトリ概観
@mui/material
├── Accordion
├── AccordionActions
├── AccordionDetails
…
├──esm
│   ├── Accordion
│   ├── AccordionActions
│   ├── AccordionDetails
│   …
…
├── index.d.ts
├── index.js
└── package.json
```

```json-c:package.json抜粋
{
  "main": "./index.js",
  "module": "./esm/index.js",
  "types": "./index.d.ts",
}
```

（ディレクトリから察するだけで真意は分かりませんが）CommonJSが先にあり、後からfaux ESMの内容が`esm`ディレクトリに追加されていそうな気がします。

### @mui/joy

記事執筆時のバージョンは`5.0.0-alpha.53`です。
概観は以下のようになっていました。

```shell:ディレクトリ概観
@mui/material
├── Alert
├── AspectRatio
├── Avatar
…
├── node
│   ├── Alert
│   ├── AspectRatio
│   ├── Avatar
│   …
…
├── index.d.ts
├── index.js
└── package.json
```

```json-c:package.json抜粋
{
  "main": "./node/index.js",
  "module": "./index.js",
  "types": "./index.d.ts",
}
```

faux ESMがデフォルト、しかしCommonJSでの対応も必要だから`node`ディレクトリを作った気配がします。（命名から察するに）
`@mui/material`とは逆です。

### Ant Design

記事執筆時のバージョンは`4.24.1`です。

```shell:ディレクトリ概観
antd
├── es
│   ├── _util
│   ├── affix
│   ├── alert
│   …
│   ├── index.d.ts
│   └── index.js
├── lib
│   ├── _util
│   ├── affix
│   ├── alert
│   …
│   ├── index.d.ts
│   └── index.js
…
└── package.json
```

```json-c:package.json抜粋
{
  "main": "lib/index.js",
  "module": "es/index.js",
  "typings": "lib/index.d.ts",
}
```

CommonJSとfaux ESMを並列に扱っている感じがします。
`typings`が`lib`を指定していますが`es`ディレクトリの中にも`.d.ts`ファイルが存在していて、重複していそうです。
悪い影響が出るようなものではなさそうですが、ちょっとしっくりきません（個人的に、です）。

### Chakra UI

記事執筆時のバージョンは`2.3.7`です。

```shell:ディレクトリ概観
@chakra-ui
├── accordion
│   ├── dist
│   │   ├── index.cjs.js
│   │   ├── index.d.ts
│   │   └── index.esm.js
│   └── package.json
├── alert
│   ├── dist
│   │   ├── index.cjs.js
│   │   ├── index.d.ts
│   │   └── index.esm.js
│   └── package.json
├── anatomy
│   ├── dist
│   │   ├── index.cjs.js
│   │   ├── index.d.ts
│   │   └── index.esm.js
│   └── package.json
…
├── react
│   ├── dist
│   │   ├── index.cjs.js
│   │   ├── index.d.ts
│   │   └── index.esm.js
│   └── package.json
…
└── visually-hidden
     ├── dist
     │   ├── index.cjs.js
     │   ├── index.d.ts
     │   └── index.esm.js
     └── package.json
```

```json-c:@chakra-ui/react/package.json抜粋
{
  "main": "dist/index.cjs.js",
  "module": "dist/index.esm.js",
  "types": "dist/index.d.ts",
  "exports": {
    ".": {
      "import": "./dist/index.esm.js",
      "require": "./dist/index.cjs.js"
    },
    "./package.json": "./package.json"
  },
}
```

`@chakra-ui`の直下にフラットにコンポーネントが展開されていて驚きましたが、インストールしたのは`@chakra-ui/react`なのでそのディレクトリの`package.json`を確認しました。

CommonJSとfaux ESMを並列に扱っています。
Ant Designは並列に扱いつつも`main`と`typings`が`lib`で`module`が`es`となり、若干CommonJS寄り（？）に見えたのに対し、Chakra UIはすべて`dist`を指定していて完全に平等（？）に見えます。

### Mantine

記事執筆時のバージョンは`5.7.1`です。

```shell:ディレクトリ概観
@maitine/core
├── cjs
│   ├── Accordion
│   ├── ActionIcon
│   ├── Affix
│   …
├── esm
│   ├── Accordion
│   ├── ActionIcon
│   ├── Affix
│   …
├── lib
│   ├── Accordion
│   ├── ActionIcon
│   ├── Affix
│   …
└── package.json
```

```json-c:package.json抜粋
{
  "main": "cjs/index.js",
  "module": "esm/index.js",
  "types": "lib/index.d.ts",
}
```

CommonJSとfaux ESMを並列に扱っていて、CommonJSもfaux ESMも`types`は`lib`のものを共用していそうです。
`lib`じゃなくて`types`とかの名前の方が自然な感じもしますが、何か意図があるのでしょうか……。

## 最後に

記事の冒頭にも書きましたが、どのライブラリも割と独自の構成なのが驚きです。
何かベストプラクティスがあってどのライブラリもそれに則っているのだとばかり思っていました。
しかしよく考えると「UIライブラリの作り方」みたいな情報発信や意見交換は普段あまり見ない気がしますし、意外とみんな手探りなのかもしれません。

もし自分で作るなら、Chakra UIから各モジュールの`package.json`を抜いたようなものを作りたい気持ちでいます。

MUIやAnt Designのような形式だと`d.ts`のファイルが重複して勿体なく思い、Mantineの形式だとディレクトリが分かれていて中身を見るときに大変そうに思いました。
（つまり、ほとんど感情の問題です）

---

最後まで読んでくださってありがとうございます！
Twitter でも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox

Dev トークでのお話してくださる方も募集中です！

https://jobs.qiita.com/employers/qiita-inc/dev_talks/489