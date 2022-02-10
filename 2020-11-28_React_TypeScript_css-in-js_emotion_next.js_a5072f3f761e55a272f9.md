<!--
title:   Next.js v10でTypeScriptを使いつつEmotion v11を使おうとしたら5時間も解決しなかった話
tags:    React,TypeScript,css-in-js,emotion,next.js
id:      a5072f3f761e55a272f9
private: false
-->
## これは何

- タイトルにあるように、5時間もハマってしまいました
- 一応解決出来たので備忘録として残しています

### 使用しているパッケージの情報

| 名前 | バージョン |
|---|---|
|next|10.0.1|
|react|17.0.1|
|typescript|4.0.5|
|@emotion/react|11.1.1|
|@emotion/babel-plugin|11.0.0|


## JSX Pragmaが動かない

### 事象

- [インストールの記事](https://emotion.sh/docs/install)などには`/** @jsx jsx */`という記述が紹介されている
- しかしそのままだとコンソールエラーが出た

### 解決策

- `/** @jsxImportSource @emotion/react */`という記述にする
    - [CSS Propの詳細な説明のページ](https://emotion.sh/docs/css-prop#jsx-pragma)に記載されている
    - Reactのバージョンが新しくて（v17）JSXのランタイムの方式が違っていたため、従来の記述からは変える必要があった

## Babelの設定が上手く働かない

### 前提

- 新しいJSX Pragmaが動くようにはなったものの、毎回書くのは面倒
- Babelの設定をしてJSX Pragmaの記載無しで動くようにしたい

### 事象

`@emotion/babel-preset-css-prop`を使ってもJSX Pragma無しだと動かなかった

### 解決策

- `@emotion/babel-preset-css-prop`の代わりに`@emotion/babel-plugin`を入れつつ、`.babelrc`を以下のようにしたら動いた
    - babelrcの中身は[公式のこのページより抜粋](https://emotion.sh/docs/css-prop#babel-preset)

```json:.babelrc
{
  "presets": [
    [
      "next/babel",
      {
        "preset-react": {
          "runtime": "automatic",
          "importSource": "@emotion/react"
        }
      }
    ]
  ],
  "plugins": ["@emotion/babel-plugin"]
}
```

## 動くは動くけど、型定義がなくて怒られる

### 事象

- スタイル自体は当てられるけどエディタ上でエラーが出ている
    - インストールしただけだとEmotionの型定義ファイルが無かった

### 解決策

- `next-env.d.ts`に以下の一行を足す
    - [Emotion v11についてのページ](https://emotion.sh/docs/emotion-11)に記載されている

```ts:next-env.d.ts
/// <reference types="@emotion/react/types/css-prop" />
```

## まとめ

- 過去にEmotionを適用したリポジトリを見ながら、ただ真似るだけで設定していたのが良くなかった
- 公式ドキュメントをしっかり読むのはやっぱり大事