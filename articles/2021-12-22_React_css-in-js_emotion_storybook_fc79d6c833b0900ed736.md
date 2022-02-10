<!--
title:   StorybookでEmotionのJSX pragmaを省略する方法
tags:    React,css-in-js,emotion,storybook
id:      fc79d6c833b0900ed736
private: false
-->
## この記事の概要

コンポーネントカタログとしてStorybookを使用しているチームは多いと思います。
そしてCSS in JSのライブラリでEmotionを使用しているチームもまあまあ多いと思います。

この2つを組み合わせるにあたって、通常だと各コンポーネントでJSX pragmaを書かないといけないのですが、それを省略するための方法をまとめました。

| ライブラリ | バージョン |
| --- | --- |
| react | 17.0.2 |
| react-dom | 17.0.2 |
| @emotion/react | 11.7.1 |
| @emotion/babel-preset-css-prop | 11.2.0 |
| @storybook/react | 6.4.9 |

https://storybook.js.org/

https://emotion.sh/docs/introduction

## 伝えたいこと

Emotionを使ったことがある人であれば`/** @jsx jsx */`（または`/** @jsxImportSource @emotion/react */`）の記述をした経験はあると思いますが、毎回書くの結構面倒じゃありませんか？

一度設定してしまえば次からは書かなくて済むので是非設定しましょう！とお伝えしたくこの記事を書きました。

## やり方

:::note warn
完全に新規で構築する段階から説明します。
ある程度構築済みの場合は適宜ステップを飛ばしてください。
:::

### 空のディレクトリの作成

とにもかくにもまずはディレクトリを作成します。

```bash
mkdir your-project-name
cd your-project-name
```

### package.jsonの初期設定

空のpackage.jsonを作りましょう。
私はyarnを使っていますが、npmでももちろんOKです。

```bash
yarn init
```

### Reactのインストール

一旦この2つだけでOKです。

```bash
yarn add react react-dom
```

### Storybookの初期設定

package.jsonを見て良い感じに初期設定をしてくれます。
`Button`, `Header`, `Page`の3コンポーネントと、`Introduction`のmdxが出力されるのを確認してください。

```bash
npx sb init
```

:::note
ちなみにもし先ほどのステップでTypeScriptを入れていると`tsx`でstoriesが出来上がります。
:::

https://storybook.js.org/docs/react/get-started/install

### CSSを消してEmotionに置き換える

ひとまずCSSファイルを消してしまいましょう。

```bash
rm stories/button.css stories/header.css stories/page.css
```

各コンポーネントをアップデートするのですが全て載せると多いので、一例として、置き換えた後の`Button.jsx`のコードを載せます。

```javascript:stories/Button.jsx
/** @jsxImportSource @emotion/react */
import React from "react";
import { css } from "@emotion/react";

export const Button = ({
  primary = false,
  size = "medium",
  backgroundColor,
  label,
  className,
  ...props
}) => {
  const mode = primary ? styles.primary : styles.secondary;
  return (
    <button
      type="button"
      css={[styles.button, styles[size], mode]}
      className={className}
      style={{ backgroundColor }}
      {...props}
    >
      {label}
    </button>
  );
};

const styles = {
  button: css({
    fontFamily: '"Nunito Sans", "Helvetica Neue", Helvetica, Arial, sans-serif',
    fontWeight: 700,
    border: 0,
    borderRadius: "3em",
    cursor: "pointer",
    display: "inline-block",
    lineHeight: 1,
  }),
  primary: css({
    color: "white",
    backgroundColor: "#1ea7fd",
  }),
  secondary: css({
    color: "#333",
    backgroundColor: "transparent",
    boxShadow: "rgba(0, 0, 0, 0.15) 0px 0px 0px 1px inset",
  }),
  small: css({
    fontSize: 12,
    padding: "10px 16px",
  }),
  medium: css({
    fontSize: 14,
    padding: "11px 20px",
  }),
  large: css({
    fontSize: 16,
    padding: "12px 24px",
  }),
};
```

### `@emotion/babel-preset-css-prop`のインストールと適用

Emotionから出ているBabelプラグインである`@emotion/babel-preset-css-prop`をインストールします。

```bash
yarn add @emotion/babel-preset-css-prop
```

StorybookのBabel設定をアップデートする場合`.storybook/main.js`を触ります。

https://storybook.js.org/docs/react/configure/babel

```diff_javascript:.storybook/main.js
  module.exports = {
    stories: [
      "../stories/**/*.stories.mdx",
      "../stories/**/*.stories.@(js|jsx|ts|tsx)",
    ],
    addons: ["@storybook/addon-links", "@storybook/addon-essentials"],
    framework: "@storybook/react",
+   babel: async (options) => ({
+     ...options,
+     presets: [...options.presets, "@emotion/babel-preset-css-prop"],
+   }),
  };
```

<details>
<summary>**@emotion/babel-preset-css-propをインストールしないとどうなるの？**</summary>
<div>

以下にスクリーンショットとコピペしたテキスト、両方載せてみました。
要は「css propとして上手く渡せていないですよ」と怒られてしまっているんですね。
全くスタイルが当たらない状態になってしまうので、インストール必須というわけです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/3de1a06c-4f10-0c9b-d8ad-4431e4758c25.png)

> You have tried to stringify object returned from `css` function. It isn't supposed to be used directly (e.g. as value of the `className` prop), but rather handed to emotion so it can handle it (e.g. as value of `css` prop).,You have tried to stringify object returned from `css` function. It isn't supposed to be used directly (e.g. as value of the `className` prop), but rather handed to emotion so it can handle it (e.g. as value of `css` prop).,You have tried to stringify object returned from `css` function. It isn't supposed to be used directly (e.g. as value of the `className` prop), but rather handed to emotion so it can handle it (e.g. as value of `css` prop).

</div>
</details>

### JSX pragmaの削除（完成！）

```diff_javascript:stories/Button.jsx
- /** @jsxImportSource @emotion/react */
  import React from "react";
  import { css } from "@emotion/react";
```

## まとめ

- `@emotion/babel-preset-css-prop`をインストールし`.storybook/main.js`に設定を追記する
- JSX Pragmaを削除する