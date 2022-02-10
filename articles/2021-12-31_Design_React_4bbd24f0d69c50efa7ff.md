<!--
title:   あえてライブラリを使わずにReactでアニメーションを作る
tags:    Design,React,アニメーション,デザイン
id:      4bbd24f0d69c50efa7ff
private: false
-->
## この記事の概要

[Framer Motion](https://www.framer.com/motion/)などを使えば良い感じのアニメーションをすぐ作ることができますが、そんな中であえて何も使わずにやってみた記事です。

## 方針

- Create React Appを使う
- スタイリングやアニメーション用のライブラリは使わない
- 何かしらのstate変更でアニメーションを起こす

## 完成物

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b5869d71-881b-b3fa-5f59-6ed2abaac25c.gif)

## コード

```javascript:App.jsx
import React, { useState } from "react";
import clsx from "clsx";
import "./App.css";

function App() {
  const [fullSize, setFullSize] = useState(false);
  return (
    <main className="main">
      <div
        className={clsx("card", fullSize && "card--fullsize")}
        onClick={() => setFullSize((current) => !current)}
      >
        <img src="https://picsum.photos/500" alt="" className="card__image" />
        <p className={clsx("card__title", fullSize && "card__title--fullsize")}>
          Card title
        </p>
        <p className={clsx("card__lead", fullSize && "card__lead--fullsize")}>
          Lorem ipsum dolor sit amet,
        </p>
        {fullSize && (
          <p className="card__text">
            consectetur adipiscing elit, sed do eiusmod tempor incididunt ut
            labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud
            exercitation ullamco laboris nisi ut aliquip ex ea commodo
            consequat. Duis aute irure dolor in reprehenderit in voluptate velit
            esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat
            cupidatat non proident, sunt in culpa qui officia deserunt mollit
            anim id est laborum.
          </p>
        )}
      </div>
    </main>
  );
}

export default App;
```

`css:App.css
.main {
  display: grid;
  min-height: 100%;
  padding: 16px;
  place-items: center;
}

.card {
  border-radius: 8px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 14%);
  cursor: pointer;
  line-height: 1.5;
  max-width: 480px;
  padding: 16px;
  width: 100%;
}

[class^="card"] {
  transition: all 250ms ease-in-out;
}

.card--fullsize {
  box-shadow: none;
  padding: 0;
}

.card__image {
  aspect-ratio: 2 / 1;
  border-radius: 4px;
  display: block;
  object-fit: cover;
  width: 100%;
}

.card__title {
  font-size: 32px;
  font-weight: 700;
  margin-top: 16px;
}

.card__title--fullsize {
  font-size: 40px;
}

.card__lead {
  font-size: 20px;
}

.card__lead--fullsize {
  font-size: 24px;
}

.card__text {
  margin-top: 16px;
}
`

## やったこと

- stateにあわせてclassNameを追加する
- `[class^="card"]`と前方一致で指定することで都度`transition`を指定しないで済むようにする

## まとめ

- 最終的に昔ながらのアニメーションのさせ方みたいになった
- しかし、stateにあわせてどんなclassNameがつくのかが予め分かっているので同じようなアプローチにしても分かりやすくなっているかもしれない