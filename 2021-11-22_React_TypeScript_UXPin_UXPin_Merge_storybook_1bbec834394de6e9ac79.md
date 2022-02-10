<!--
title:   0から作るUXPin Merge + TypeScript + Storybookの環境 その3
tags:    React,TypeScript,UXPin,UXPin_Merge,storybook
id:      1bbec834394de6e9ac79
private: false
-->
## これは何

- [この記事](2021-11-09_React_TypeScript_UXPin_UXPin_Merge_storybook_d75d9a00c8102216e0ae.md)と[この記事](2021-11-11_React_TypeScript_UXPin_UXPin_Merge_storybook_73e1dff5eea8d1aa4599.md)の続きで、[UXPin Merge](https://www.uxpin.com/merge)をTypeScriptとともに導入するための記事です
    - [こちらのPull Requestに今回の記事で変更した部分が全て載っています](https://github.com/xrxoxcxox/qiita-uxpin-with-typescript/pull/2)
- 前回はCSS Modulesを導入しましたが、筆者はCSS in JSの方が好みなため更に入れ替えます
    - 使い慣れているのもあり、[Emotion](https://emotion.sh/docs/introduction)の導入を説明します

https://www.uxpin.com/merge

https://emotion.sh/docs/introduction

https://github.com/xrxoxcxox/private-npm-package-test-export

## Emotionを使う準備

### ライブラリのインストール

まずはこちらのコマンド。

```terminal:ターミナル
yarn add -D @emotion/react
```

:::note warn
Emotionにはstyled-component記法でスタイルを書くための@emotion/styledもありますが、筆者がstyled-component記法をあまり好まないためこの記事では省略しています。
:::

### JSX Pragmaを省略するために

```terminal:ターミナル
yarn add -D @emotion/babel-preset-css-prop
```

`diff_json:tsconfig.json
  {
    "compilerOptions": {
      "target": "es5",
      "module": "commonjs",
      "esModuleInterop": true,
      "forceConsistentCasingInFileNames": true,
      "strict": true,
      "skipLibCheck": true,
-     "jsx": "react"
+     "jsx": "react-jsx",
+     "jsxImportSource": "@emotion/react"
    }
  }


```diff_javascript:.storybook/main.js
  module.exports = {
    stories: [
      "../stories/**/*.stories.mdx",
      "../stories/**/*.stories.@(js|jsx|ts|tsx)",
    ],
    addons: [
      "@storybook/addon-links",
      "@storybook/addon-essentials",
      "storybook-css-modules-preset",
    ],
+   babel: async (options) => ({
+     ...options,
+     presets: [...options.presets, "@emotion/babel-preset-css-prop"],
+   })
  }
```

上記の設定はしなくても動くのですが、そうするとtsxファイルの先頭に毎回

```typescript:SomeComponent.tsx
/** @jsxImportSource @emotion/react */
```

と記載しないといけなくなってしまいます（[JSXプラグマというもので、詳しくはこちらをご覧ください](https://emotion.sh/docs/css-prop#jsx-pragma)）

毎回書くのは地味に面倒ですし、うっかり忘れて動かなくなることもあるので対策しておくためのコードでした。

:::note warn
2021年11月現在、実はEmotion公式は@emotion/babel-preset-css-propではなく@emotion/babel-pluginを推奨しているのですがStorybookで上手く動いてくれませんでした。そのためこの記事では@emotion/babel-preset-css-propを使用しています。
:::

## Emotionを使う

ここまで来たら各コンポーネントのスタイル指定をCSS Modulesの記法からEmotion式に変えるだけでOKです。

こんな記事を書いておいて何ですが、実はEmotionを使うにあたりUXPin自体の設定変更は必要ありません笑
ただ、自分が導入しようと思ったときは「実質セット」であるStorybook側の設定に色々悩まされたのでこのタイトルで記事を投稿しています。

## お片付け

前回導入したCSS Modules用の記述がいくらか残っていますので綺麗にします

```diff_javascript:uxpin.webpack.config.js
  ...
  module: {
    rules: [
      {
        loader: ['babel-loader', 'ts-loader'],
        test: /\.tsx$/,
        exclude: /node_modules/
      },
-     {
-       test: /\.css$/,
-       use: [
-         {
-           loader: 'style-loader',
-         },
-         {
-           loader: 'css-loader',
-           options: {
-             importLoaders: 2,
-             modules: true
-           },
-         },
-       ],
-     },
    ]
  }
  ...
```

`diff_javascript:.storybook/main.js
  ...
  addons: [
    "@storybook/addon-links",
    "@storybook/addon-essentials",
-   "storybook-css-modules-preset",
  ],
  ...


```terminal:ターミナル
yarn remove @types/css-modules css-loader storybook-css-modules-preset style-loader
```

あとは`module.css`ファイルを消せばOKです。