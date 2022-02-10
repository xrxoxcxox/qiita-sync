<!--
title:   Create React Appでejectせずにjsx pragma無しのEmotionを使う（TypeScript編）
tags:    React,create-react-app,css-in-js,emotion,フロントエンド
id:      17e0762d8e69c1ef208f
private: false
-->
## これは何

- 以下の一連の流れを解決するための記事です
    - CSS in JSの1つであるEmotionは、デフォルトだとjsx pragmaを毎ファイル書かないといけない
        - `/** @jsx jsx */`とか
        - `/** @jsxImportSource @emotion/react */`とか
    - 書かずに済むためにはbabelの設定を変える必要がある
    - Create React Appでアプリケーションを作るとejectしないとbabelの設定が変えられない
    - 困ってしまう
- また、投稿イベント「フロントエンド強化月間 - 開発する上で知っておくべき知見を共有しよう」への記事でもあります

https://qiita.com/official-events/b9ad63394fa2635dfca9

## ゴールのイメージ

デフォルトだと、それぞれのtsxファイルで以下のような記述をしないといけませんが

```tsx
/** @jsxImportSource @emotion/react */
// eslint-disable-next-line @typescript-eslint/no-unused-vars
import { css, jsx } from '@emotion/react'
```

以下のようにスッキリさせることができます。

```tsx
import { css } from '@emotion/react'
```

また、後ほど説明する`@emotion/babel-plugin`を入れられるようになることで、見た目の問題だけでなくminifyやデッドコードの解消もできます。


## ひとまずCreate React App

自分はyarnを使いつつTypeScript編と銘打っている[^1]ので以下のコマンド。

[^1]: TypeScript編があるならVanillaのJavaScript編があるのかと思いきや、ありません。

```shell
yarn create react-app my-app --template typescript
```

現状のビューと、コードは以下のようになっています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/6ef4d752-ebc5-e369-79ed-c252b70fa616.png)

```tsx:App.tsx
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.tsx</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;

```

## Emotionと@emotion/babel-pluginのインストール

作業の順番を分かりやすくするため先にEmotionとbabel-pluginを入れておきますが、まだスタイルは書きません。

```shell
yarn add @emotion/react
yarn add -D @emotion/babel-plugin
```

なお、babelの設定のために`@emotion/babel-preset-css-prop`を紹介している記事もありますが、2021年5月現在は基本的に`@emotion/babel-plugin`を使う方が良いでしょう。

Reactがv17になってからJSXの変換が新しくなり、それに対応しているのは`@emotion/babel-plugin`だからです。

## CRACOのインストールと設定

https://github.com/gsoft-inc/craco

**C**reate **R**eact **A**pp **C**onfiguration **O**verrideの略でCRACOだそうです。
頭の中では「クラコ」と読んでいて、可愛らしい名前だなあと勝手に感じています。

```shell
yarn add @craco/craco
```

インストールしたらプロジェクトのルートフォルダに`craco.config.js`という名前のファイルを作って、以下のように記載します。

```javascript:craco.config.js
module.exports = {
  babel: {
    presets: [
      [
        "@babel/preset-react",
        { runtime: "automatic", importSource: "@emotion/react" },
      ],
    ],
    plugins: ["@emotion/babel-plugin"],
  },
};
```

詳細な書き方の説明は[公式ドキュメント](https://github.com/gsoft-inc/craco/blob/master/packages/craco/README.md#configuration)に記載がある通りですが、`babel:`や`webpack:`の中にオーバーライドしたい設定を書けばOKです。

ちなみに上記のBabelの設定は[Emotion公式のCSS Propの説明](https://emotion.sh/docs/css-prop)に記載がある通りです。

その後、npm-scriptsを変更します。

```diff_json:package.json
  "scripts": {
-   "start": "react-scripts start",
+   "start": "craco start",
-   "build": "react-scripts build",
+   "build": "craco build",
-   "test": "react-scripts test",
+   "test": "craco test",
    "eject": "react-scripts eject"
  },
```

### 余談：設定をオーバーライドするためのパッケージについて

どんなものがあるのかnpm trendsで調べたところ、一番ダウンロードが多いのは`react-app-rewired`でした。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/76114ee8-ce9d-eb13-55bc-84b73f12e9a1.png)

https://www.npmtrends.com/@craco/craco-vs-@rescripts/cli-vs-customize-cra-vs-react-app-rewired-vs-rescripts-vs-custom-react-scripts

しかし`react-app-rewired`はCreate React App（以下CRA）の2系にまでしか対応していないのか、動いてくれませんでした。
`CRACO`はダウンロード数も2位ですし、CRAの4系にも対応しているので最終的にこちらを使っています。

## tsconfig.jsonの変更

TypeScriptでEmotionを扱うために、`tsconfig.json`にも少しだけ変更を加えてあげないといけません。
と言ってもCRAで生成されたものに1行追加するだけ。

```diff_json:tsconfig.json
  {
    "compilerOptions": {
      "target": "es5",
      "lib": [
        "dom",
        "dom.iterable",
        "esnext"
      ],
      "allowJs": true,
      "skipLibCheck": true,
      "esModuleInterop": true,
      "allowSyntheticDefaultImports": true,
      "strict": true,
      "forceConsistentCasingInFileNames": true,
      "noFallthroughCasesInSwitch": true,
      "module": "esnext",
      "moduleResolution": "node",
      "resolveJsonModule": true,
      "isolatedModules": true,
      "noEmit": true,
-     "jsx": "react-jsx"
+     "jsx": "react-jsx",
+     "jsxImportSource": "@emotion/react"
    },
    "include": [
      "src"
    ]
  }

```

## Emotionを用いてスタイルを変更

やっとスタイルを変更します。
今回は例を示すだけなのでごく簡単な内容を。

```diff_tsx:App.tsx
- import React from 'react';
  import logo from './logo.svg';
  import './App.css';
+ import { css } from '@emotion/react'

  function App() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <p>
            Edit <code>src/App.tsx</code> and save to reload.
          </p>
          <a
-           className="App-link"
+           css={appLink}
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            Learn React
          </a>
        </header>
      </div>
    );
  }

+ const appLink = css`
+   background-color: #61dafb;
+   border-radius: 8px;
+   color: #212121;
+   font-weight: bold;
+   padding: 20px 40px;
+   text-decoration: none;
+ `
+
  export default App;

```

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4b08e32d-dac3-7c9f-69cc-a33d879f7a2e.png)

## まとめ

- Emotionと@emotion/babel-pluginをインストールする
- CRACOをインストールする
- `craco.config.js`を作成し、通常`.babelrc`に書く内容を`craco.config.js`内に記載する
- npm-scriptsを`react-scripts`から`craco`へ更新する
- `tsconfig.json`に`"jsxImportSource": "@emotion/react"`を追加する