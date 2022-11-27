<!--
title:   @rollup/plugin-typescriptの8.2.5から8.3.0でincludeとexcludeのパス指定が変わった
tags:    @rollup-plugin-typescript,TypeScript,rollup.js
id:      97da9f6ffbf5ee3fa033
private: false
-->
## この記事の概要

タイトル通りですが、[@rollup/plugin-typescript](https://github.com/rollup/plugins/tree/master/packages/typescript)の8.2.5から8.3.0で`include`と`exclude`のパス指定が少し変わっていたので備忘録がてら記事にまとめました。

## 前提

React + TypeScriptでコンポーネントライブラリを作っている中で発見しました。

Storybookを導入しているのですが`*.stories.tsx`の型定義はビルド内容には含めたくありません。
そのため`rollup.config.js`内で`tsconfig`をオーバーライドしていました。

```jsonc:tsconfig.json（抜粋）
{
  "compilerOptions": {
    // オプション色々
    "rootDir": "src"
  },
  "include": ["src/**/*"]
}
```

```javascript:rollup.config.js（抜粋）
export default {
  input: "src/index.ts",
  output: {
    // オプション色々
  }
  plugins: [
    // プラグイン色々
    typescript({ exclude: ["src/**/*.stories.tsx"] })
  ]
}
```

### 8.2.5での挙動

元のコード | ビルドしたコード
--- | ---
![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5f868375-612e-8986-6081-45508e3ab90b.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bf2295bb-a93e-b2f7-cd33-5a8b62378475.png)

期待している通りになっています

### 8.3.0での挙動

コードは何も変えていないのに`stories.d.ts`が生成されてしまいました。

元のコード | ビルドしたコード
--- | ---
![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5f868375-612e-8986-6081-45508e3ab90b.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c6156576-2da2-09f9-dc38-15aad724fb63.png)

## 原因

8.3.0での変更内容は`filterRoot`というオプションの追加で、こちらのデフォルト設定によるものでした。

ざっくり説明すると、`include`と`exclude`のパターン指定において、ルートになるディレクトリを自分で指定できるようになる、というものです。
デフォルトでは`tsconfig.json`の`rootDir`を指定されていました。

つまり、もともと`rollup.config.js`に書いていた`exclude: ["src/**/*.stories.tsx"]`は、デフォルトの設定と相まって`src/src/**/*.stories.tsx`を指定しているのに等しくなっていたようです。

## 解決策

`exclude: ["**/*.stories.tsx"]`と指定したら無事期待通りの動きになりました。