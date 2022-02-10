<!--
title:   0から作るUXPin Merge + TypeScript + Storybookの環境 その2
tags:    React,TypeScript,UXPin,UXPin_Merge,storybook
id:      73e1dff5eea8d1aa4599
private: false
-->
## これは何

- [この記事](2021-11-09_React_TypeScript_UXPin_UXPin_Merge_storybook_d75d9a00c8102216e0ae.md)の続きで、[UXPin Merge](https://www.uxpin.com/merge)をTypeScriptとともに導入するための記事です。
    - [こちらのPull Requestに今回の記事で変更した部分が全て載っています](https://github.com/xrxoxcxox/qiita-uxpin-with-typescript/pull/1)
- 前回はStorybookのイニシャライズで自動生成されたコードをそのまま使っていましたが、今回はそれらをCSS Modules化します

https://www.uxpin.com/merge

https://github.com/xrxoxcxox/qiita-uxpin-with-typescript

## CSS Modulesを使う準備

### ライブラリのインストール

まずはこちらのコマンド。

```terminal:ターミナル
yarn add -D @types/css-modules storybook-css-modules-preset
```

https://www.npmjs.com/package/@types/css-modules

https://www.npmjs.com/package/storybook-css-modules-preset

### @types/css-modulesの説明

TypeScriptの環境でCSS Modulesを使うと、以下のようなエラーが出ます。

```
Cannot find module './foo.module.css' or its corresponding type declarations.
```

これを解消するためにインストールしています。

中身は実質これだけなので、自分で`d.ts`をどこかに用意しても良いんですが……笑

```typescript:node_modules/@types/css-modules/index.d.ts
interface CSSModule {
    /**
     * Returns the specific selector from imported stylesheet as string.
     */
    [key: string]: string;
}
```

### Storybookの準備の続き

`storybook-css-modules-preset`をインストールしただけでは駄目なので、`.storybook/main.js`を以下のように編集します。

```diff_javascript:.storybook/main.js
  module.exports = {
    "stories": [
      "../stories/**/*.stories.mdx",
      "../stories/**/*.stories.@(js|jsx|ts|tsx)"
    ],
    "addons": [
      "@storybook/addon-links",
-     "@storybook/addon-essentials"
+     "@storybook/addon-essentials",
+     "storybook-css-modules-preset"
    ]
  }
```

ライブラリを使わず`main.js`の中に以下のような記述をしても良いのですが、使う方が簡単なので今回は採用しています。

```javascript:.storybook/main.js
webpackFinal: (config) => {
  // webpackの設定
}
```

https://storybook.js.org/docs/react/configure/webpack

### UXPinの準備

`uxpin.webpack.config.js`を編集します。

```diff_javascript:uxpin.webpack.config.js
  {
    test: /\.css$/,
    use: [
      {
        loader: 'style-loader',
      },
      {
        loader: 'css-loader',
        options: {
-         importLoaders: 2
+         importLoaders: 2,
+         modules: true
        },
      },
    ],
  },
```

`css-loader`の`modules`をtrueにするだけでOK。

## CSS Modulesの適用

あとはひたすら同じ作業なので、[Button.tsxを例にして説明します。](https://github.com/xrxoxcxox/qiita-uxpin-with-typescript/blob/fbccb75c4801c562de9d0418843558743db913ae/stories/Button.tsx)

CSSのimportをCSS Modulesの作法にならって変更。

```diff_typescript:Button.tsx
- import './button.css';
+ import styles from './button.module.css';
```

classNameが単なる文字列として指定されている箇所をCSS Modulesの作法にならって変更。

```diff_typescript:Button.tsx
- const mode = primary ? 'storybook-button--primary' : 'storybook-button--secondary';
+ const mode = primary ? styles['storybook-button--primary'] : styles['storybook-button--secondary'];
```

ちなみに一般的には`styles.foo`の書式だと思いますが、今回のようにクラス名がケバブケースになっている場合は`styles.['foo-bar']`とすることで回避可能です。

| OK | NG |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/16754001-223e-164c-de9a-3547d8d0a6d3.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1b6710ab-7a85-6592-a490-af46c98c4fc6.png) |

そもそもハイフンで繋がなければ良いじゃん……というのはごもっともですが、ときには使えるテクニックかもしれません。

あとはひたすら同じ作業を繰り返していけば完成です！

## ちなみに

前回の記事では`uxpin.config.js`からしれっと`Page.tsx`を抜いていました。

```javascript:前回の記事で載せていたuxpin.config.jsの内容
module.exports = {
  components: {
    categories: [
      {
        name: 'General',
        include: [
          'stories/Button.tsx',
          'stories/Header.tsx'
          // StorybookのイニシャライズでPage.tsxも生成されているけど登録していない
        ]
      }
    ],
    webpackConfig: 'uxpin.webpack.config.js'
  },
  name: 'UXPin Merge + TypeScript + Storybook'
}
```

じつは`page.css`の中では`h2`や`p`などタグに対して直接スタイルを指定していたんですね。
そのためUXPinのGUIの画面のスタイルが崩れてしまいます。

その説明を前回の記事の時点でするのは話がややこしくなるな……と思って省略していました。

今回CSS Modulesを適用したので`Page.tsx`を登録しても問題なく動きます。