<!--
title:   0から作るUXPin Merge + TypeScript + Storybookの環境
tags:    React,TypeScript,UXPin,UXPin_Merge,storybook
id:      d75d9a00c8102216e0ae
private: false
-->
## これは何

- デザインツールである[UXPin Merge](https://www.uxpin.com/merge)を導入するにあたり、TypeScriptでの環境構築の仕方をまとめた記事です
    - 公式ドキュメントには[通常のJavaScriptでの導入の仕方](https://www.uxpin.com/docs/merge/integrating-your-own-components/)しか記載がなかったので書いてみました

https://www.uxpin.com/merge

また、今回の記事内のコードは以下のリポジトリに全て載せています。

https://github.com/xrxoxcxox/qiita-uxpin-with-typescript

## 前提

UXPinを導入する場所は、アプリケーションのコードが全て入っているリポジトリよりも、UIライブラリやデザインシステムといった単位で管理しているリポジトリの方が良いでしょう。

UXPin専用のコードを書かないといけない箇所もあるので、ライブラリを開発するためのリポジトリ内で管理した方が取り回しがしやすそうです。

というわけで、ライブラリとして開発するため実質的にStorybookはセット。
そのためこの記事ではStorybookの導入まで説明します。

## 0. 初期化

:::note warn
この記事ではyarnを使っていますが、npmを使っている方は適宜読み替えてください。
:::

ターミナルでコマンドを叩きます。

```terminal:ターミナル
yarn init -y
```

このような内容の`package.json`が生成されました。

```json:package.json
{
  "name": "uxpin-with-typescript",
  "version": "1.0.0",
  "main": "index.js",
  "author": "Keisuke Watanuki",
  "license": "MIT"
}
```

## 1. React + TypeScriptの準備

ターミナルでコマンドを叩きます。

```terminal:ターミナル
yarn add -D react @types/react react-dom @types/react-dom typescript
```

`diff_json:package.json
  {
    "name": "uxpin-with-typescript",
    "version": "1.0.0",
    "main": "index.js",
    "author": "Keisuke Watanuki",
-   "license": "MIT"
+   "license": "MIT",
+   "devDependencies": {
+     "@types/react": "^17.0.34",
+     "@types/react-dom": "^17.0.11",
+     "react": "^17.0.2",
+     "react-dom": "^17.0.2",
+     "typescript": "^4.4.4"
+   }
  }
`

更にコマンドを叩きます。

```terminal:ターミナル
npx tsc --init
```

`tsconfig.json`が生成されるので適宜設定して使います。
今はコメントアウトを消すなど、最低限にとどめておきました。

```json:tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true,
    "jsx": "react",
  }
}
```

https://www.typescriptlang.org/tsconfig

## 2. Storybookの準備

ターミナルでコマンドを叩きます。

```terminal:ターミナル
npx sb init
```

プロジェクトのルートに`.storybook`と`stories`というフォルダが追加されたり、`package.json`にdependenciesやscriptsが追加されたり、TypeScript + React用のコードが自動で追加されます。

:::note
コンポーネント類が「stories」というディレクトリに格納されていて、実際の運用では「components」などにリネームすると思われますが、話を簡単にするためにこのまま進めます。
:::

この段階で以下のコマンドを叩くと、localhost:6006でStorybookが起動します。

```terminal:ターミナル
yarn storybook
```

https://storybook.js.org/docs/react/get-started/install

## 3. UXPin Merge CLIの準備

ターミナルでコマンドを叩きます。

```terminal:ターミナル
yarn add -D @uxpin/merge-cli
```

`package.json`にscriptsを追加しておきましょう

```diff_json:package.json
  {
    "name": "uxpin-with-typescript",
    ...
    "scripts": {
      "storybook": "start-storybook -p 6006",
-     "build-storybook": "build-storybook"
+     "build-storybook": "build-storybook",
+     "uxpin": "uxpin-merge --disable-tunneling"
    }
  }
```

`terminal:ターミナル
yarn add -D babel-loader ts-loader@^8.2.0 style-loader@^2.0.0 css-loader@^5.2.7
`

:::note alert
@uxpin/merge-cliの2.7.9においてはloaderのバージョンが最新だと動きませんでした。根本解決ではありませんがこれらのバージョンを指定してインストールすると動作することは確認したため、ひとまずこちらをお試しください。
:::

追加でコマンドを叩きます。

```terminal:ターミナル
touch uxpin.config.js uxpin.webpack.config.js
```

それぞれのファイルには以下を記載します。

```javascript:uxpin.config.js
module.exports = {
  components: {
    categories: [
      {
        name: 'General',
        include: [
          'stories/Button.tsx',
          'stories/Header.tsx'
        ]
      }
    ],
    webpackConfig: 'uxpin.webpack.config.js'
  },
  name: 'UXPin Merge + TypeScript + Storybook'
}
```

:::note warn
StorybookのイニシャライズでPage.tsxというファイルも生成されていますが、ここでは登録していません。説明すると少し長くなってしまうので次回の記事で紹介します。
:::

```javascript:uxpin.webpack.config.js
const path = require('path')

module.exports = {
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'bundle.js',
    publicPath: '/'
  },
  resolve: {
    modules: [__dirname, 'node_modules'],
    extensions: ['*', '.tsx']
  },
  devtool: 'source-map',
  module: {
    rules: [
      {
        loader: ['babel-loader', 'ts-loader'],
        test: /\.tsx$/,
        exclude: /node_modules/
      },
      {
        test: /\.css$/,
        use: [
          {
            loader: 'style-loader',
          },
          {
            loader: 'css-loader',
            options: {
              importLoaders: 2
            },
          },
        ],
      },
    ]
  }
}
```

ここまで来たら、ターミナルで以下のコマンドを叩くとUXPinのexperimental modeが起動します。

```terminal:ターミナル
yarn uxpin
```

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1192a92e-03da-5cb6-19fc-ef01a07551d6.png)

ターミナル上に表示されたURLにアクセスすればexperimental modeで挙動を試せます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5b1ad599-2839-11a6-e75f-b1984ba9f877.png)

## 次回

この記事で作った環境にCSS Modulesを適用します。

https://qiita.com/xrxoxcxox/items/73e1dff5eea8d1aa4599