<!--
title:   rollup-plugin-postcssを調べていたメモ
tags:    ComponentLibraries,css-modules,postcss,rollup.js
id:      b47968ede7b348f5e578
private: false
-->
## これは何

- コンポーネントライブラリを作るためにRollupを使っています
    - https://www.npmjs.com/package/rollup-plugin-postcss
- RollupでCSS Modulesで書いたスタイルをバンドルするために`rollup-plugin-postcss`を使っていて、設定にハマってしまったのでメモとして残す記事です

:::note alert
この記事は「単にライブラリを使用するだけでは解決できなさそうで、ライブラリ自体にPull Requestを出すでもしないといけない」ことを示すための記事であり、根本的な解決には至りません。ライブラリ使用を諦めるか、次善策として別なオプションを使うかを判断するのに参考にしてもらえれば幸いです。
:::

## 前提

CSS Modulesを扱うために`rollup.config.js`の中でpluginsで`rollup-plugin-postcss`を使い`extract: true`と`modules: true`を指定していました。

```javascript:rollup.config.js
// ...いくつかのプラグインのimport
import postcss from "rollup-plugin-postcss"

export default [
  {
    input: "src/index.ts",
    output: [], // 中身は省略
    plugins: [
      // ...いくつかのプラグインの呼び出し
      postcss({ extract: true, modules: true })
    ]
  }
]
```

現時点ではビルドしたフォルダの構成はこのようになっています。

- build
    - components
    - index.css
    - index.d.ts
    - index.js
    - index.js.map

## 変えようと思った箇所と理由

「`extract: true`にするとcssファイルを1つにバンドルしてくれるから良いじゃん」と思っていたものの、よくよく考えると以下のようなことが起きるので設定を変えようと考えました。

- ライブラリを使う際、グローバルなスタイルとして`import 'library-name/index.css'`のように指定しないといけない
- ライブラリが大きくなれば当然CSSのファイルも大きくなる

## トライしたことと分かったこと

ひとまず、単に`extract: true`を抜いてビルドしてみます。

```diff_javascript:rollup.config.js（変更後）
  export default [
    {
      input: "src/index.ts",
      output: [], // 中身は省略
      plugins: [
        // ...いくつかのプラグインの呼び出し
-       postcss({ extract: true, modules: true })
+       postcss({ modules: true })
      ]
    }
  ]
```

するとビルドしたフォルダの構成がこのように変わりました。

- build
    - components
    - node_modules
        - style-inject
            - dist
                - style-inject.es.js
                - style-inject.es.js.map
    - index.d.ts
    - index.js
    - index.js.map

`index.css`が消えたのは狙い通りですが何故か`node_modules`が生成されてしまっています。

この状態でimportしても`Uncaught Error: Cannot find module '../node_modules/style-inject/dist/style-inject.es.js'`と表示され、使えません。

依存関係に`style-inject`を含めるなど試していたのですが、結局上手くいきませんでした。

色々調べていたらこんなIssueを発見。

https://github.com/egoist/rollup-plugin-postcss/issues/381

要約&意訳&拙訳ですが、以下のような話をしています

- `Uncaught Error: Cannot find module '../node_modules/style-inject/dist/style-inject.es.js'`と出てしまう
- `style-inject`を依存関係に含めつつ、ビルドされたコードのうち'../node_modules/style-inject/dist/style-inject.es.js'の部分を'style-inject'に書き換えてしまえば動いた
- 実は2021年5月にPull Requestが作られているものの、テストが失敗しているのでマージされていない
    - https://github.com/egoist/rollup-plugin-postcss/pull/375
- しかも`style-inject`にはスタイルを逆順に注入する問題が見つかり、詳細度の違いで意図せぬスタイルを引き起こすかもしれない
    - https://github.com/egoist/style-inject/issues/23

力技で書き換えるだけならまだしも、書いたのと逆順にスタイルが注入されるのは困りそうです。

## 最終的に

`extract: true`のままにするか、他のライブラリでCSS Modules対応を考えるか、いっそCSS in JSでライブラリのスタイルを組み直すか……。
いずれにせよ、別な方法を考えないといけないことだけは分かりました。

同じ箇所にハマって時間を使ってしまう人が減れば良いなと思って投稿します。