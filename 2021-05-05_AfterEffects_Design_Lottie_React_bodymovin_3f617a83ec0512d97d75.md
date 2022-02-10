<!--
title:   After Effectsで作ったアニメーションをreact-lottieで実装する
tags:    AfterEffects,Design,Lottie,React,bodymovin
id:      3f617a83ec0512d97d75
private: false
-->
## これは何

以下の流れをまとめた記事です。

- After Effectsで作ったアニメーションを
- Lottie（Bodymovin）でJSONに書き出して
- React上で`onClick`で動くようにする

完成物そのものは、`button`をクリックするとアニメーションがスタートするだけのシンプルなものです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f79735d8-4607-ec28-8f66-cdef53d410e1.gif)

また、GitHub Pagesで実際に触れるようにしつつ、

https://xrxoxcxox.github.io/qiita-lottie-react/

コードも全て公開しています。

https://github.com/xrxoxcxox/qiita-lottie-react

なお、この記事ではアニメーションの作り方自体の説明はしません。
以前私が書いた記事で初歩の初歩だけ紹介しているので、興味のある方はこちらをどうぞ。

https://qiita.com/xrxoxcxox/items/887de8b56d09b7e7db3c

## 全体の流れ

1. Lottieの紹介
1. After EffectsにBodymovinをインストールする（初回のみ）
1. After EffectsからJSONを書き出す
1. Reactのプロジェクト上で`react-lottie`を用いる
1. 余談：サンプルコードの通りだと上手く動かない箇所
    1. `import * as animationData from './path/to/json'`
    1. `setupTests.js`

## Lottieの紹介

https://airbnb.io/lottie/

Lottieについて、公式のドキュメントより説明を引用します。

> Lottie is a library for Android, iOS, Web, and Windows that parses Adobe After Effects animations exported as json with Bodymovin and renders them natively on mobile and on the web!

今回はWebでの話、特にReactアプリケーションでLottieを使う際の話です。

Webで動画を扱うにあたって、一番ネックになるのはファイルの容量の大きさではないでしょうか。
綺麗な動画を届けようとすればするほど重くなり、ユーザー体験を損ねかねません。

ですが、Lottieを使うとJSONを読み込むだけでアニメーションを実装できます。
実際に画面に描画されるのもsvg要素なので、かなり軽く済むんですね。

## After EffectsにBodymovinをインストールする（初回のみ）

After Effectsで作成したアニメーションを、Lottieで扱えるJSONに変換してくれるのがBodymovinです。
導入の仕方は何パターンかありますが、自分はAdobe Exchangeからインストールしました。

https://exchange.adobe.com/creativecloud.details.12557.html

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/85fb7194-7f63-1063-6be3-c1ba981a369c.png)

アクセスして、画面の右上あたりにある`Free`ボタンを押せばCreative Cloudアプリが立ち上がってインストールが始まると思います。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c8415226-49b4-18bd-af4b-a4c3bb7a5117.png)

ちゃんとインストールできていれば、`ウィンドウ > エクステンション`の中に`Bodymovin`の項目が出現しているはずです。

## After EffectsからJSONを書き出す

アニメーション作成についての説明は飛ばして、書き出し方を説明します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/362ecde5-6d01-f86f-7c42-eb551bdba618.gif)

上記のように、無限にループする円のアニメーションを作成しました。[^1]

[^1]: 例なので非常に簡単なもので完成としていますが実際はここで作り込みます。

次に`ウィンドウ > エクステンション > Bodymovin`を起動。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c1a6e6e1-4a78-dc03-9be6-a7dd7c2f3a6d.png)

このようなウィンドウが開くはずです。
更に`Settings`を選びましょう。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b8b87afe-8585-cb2e-fcb8-97122ae54034.png)

[設定項目の詳細は公式ドキュメントのComposition Settingsの章](https://aiｈrbnb.io/lottie/#/after-effects?id=composition-settings)を読んでいただく方が良いかと思いますが、いつも私がチェックを入れるのは以下の3つです。

- Glyphs
    - 今回は使っていませんが、テキストを使っている際は必須
    - いわゆるアウトライン化をしてくれるオプション
- Standard
    - 実装する際のJSONを書き出してくれるためオプションのため必須
- Demo
    - ローカルで開くだけで動きを確認できるHTMLを吐いてくれる
    - 実際に近い確認ができるのでいつも一緒に書き出している

設定を確認したら`Save`を押し、ひとつ前の画面に戻ります。
`/Users/..`と書いてある箇所をクリックすると保存場所とファイル名を選べるので、設定して`Render`を押しましょう。

データや設定に問題がなければ`Render finished`と画面に表示されるはずです。

ひとつ注意が必要なのが、BodymovinはAfter Effectsの全ての機能に対応しているわけではないこと……。

https://airbnb.io/lottie/#/supported-features

詳しくは上記のページにまとまっていますが、以下の項目に気をつけていればよっぽど大丈夫だと思います。

- シェイプレイヤーかパスデータだけで作る
- 複雑なマスクを使わない
- 基本、エフェクトは使えないものと考える

## Reactのプロジェクト上で`react-lottie`を用いる

ここからは実際のReactアプリケーション上でアニメーションを動かす流れを説明します。
今回は例なので、Create React Appで生成されるページをそのまま使いますがご了承ください。

まずは先ほど書き出したファイルを適当なディレクトリに格納。
自分は`src`直下に`animation.json`という名前で置きました。

そしてやっと`react-lottie`の登場です。

https://github.com/chenqingspring/react-lottie

### インストール

```shell:react-lottieのインストール
$ npm i react-lottie

# or

$ yarn add react-lottie
```

### `App.js`の編集

```jsx:App.js
import { useState } from 'react';
import './App.css';
import Lottie from 'react-lottie';
import animationData from './animation.json';

function App() {
  const [stop, setStop] = useState(true);

  const defaultOptions = {
    loop: false,
    autoplay: false,
    animationData,
    rendererSettings: {
      preserveAspectRatio: 'xMidYMid slice'
    }
  };

  return (
    <div className="App">
      <header className="App-header">
        <Lottie
          options={defaultOptions}
          height={'50vmin'}
          width={'50vmin'}
          isStopped={stop}
          isClickToPauseDisabled={true}
          ariaRole={''}
          eventListeners={[
            {
              eventName: 'complete',
              callback: () => setStop(true),
            },
          ]}
        />
        <button onClick={() => setStop(false)} className='App-button' disabled={!stop}>Start</button>
      </header>
    </div>
  );
}

export default App;
```

いきなり完成形を出してしまいましたが、順に解説します。

#### import

```jsx:App.js
import Lottie from 'react-lottie';
import animationData from './animation.json';
```

`react-lottie`と、書き出して配置したJSONをimportします。

#### options

```jsx:App.js
const defaultOptions = {
  loop: false,
  autoplay: false,
  animationData,
  rendererSettings: {
    preserveAspectRatio: 'xMidYMid slice'
  }
};
```

例ではこのように書きましたが、最小ならこうです。

```jsx:App.js
const defaultOptions = { animationData };
```

JSONさえ読めていれば、ループや自動再生は初期値（両方true）として動いてくれます。
TypeScriptを使う使わないに関わらず、型定義ファイルを見れば分かりやすいでしょう。

https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/react-lottie

#### Lottie component

```jsx:App.js
<Lottie
  options={defaultOptions}
  height='50vmin'
  width='50vmin'
  isStopped={stop}
  isClickToPauseDisabled={true}
  ariaRole=''
  eventListeners={[
    {
      eventName: 'complete',
      callback: () => setStop(true),
    },
  ]}
/>
```

今回自分がオプションを設定した意図は以下のものです。

- SVG要素自体はクリックできないようにしたい
- 別なbutton要素をクリックすることで再生をスタートしたい
- buttonをクリックするたびアニメーションが再生してほしい

デフォルトだとアニメーションするSVG要素自体がクリックでき、かつ`role`に`button`があたっているので`isClickToPauseDisabled`と`ariaRole`を設定したのと、`complete`イベントの発火（＝アニメーション再生終了）時にアニメーションをリセットするためにstateを操作しています。

Lottieでは`stop`は最初のフレームに戻りつつ動作停止。
対して`pause`は一時停止と明確に役割が分かれています。

そのため、アニメーションが終了したら`stop`を指定することで何度も再生できるようにしています。
このあたりは自分の作りたいアニメーションを想定しつつドキュメントとにらめっこ、実際に動かしながら確認するしかないかもしれません。

## 余談：サンプルコードの通りだと上手く動かない箇所

見出しの通りで、サンプルをコピペしたら動かなかった箇所がありました。
対処法含めてここに載せておきます。

### import * as animationData from './path/to/json'

`import * as animationData`を`import animationData`にすれば解決しました。
また、それにあわせて`options`の`animationData: animationData`も`animationData`だけに省略できます。
1つのファイルで1つのアニメーションしか使わないなら、この方が短く書けますね。

https://github.com/chenqingspring/react-lottie/issues/115

こちらのIssueで話されていました。

### setupTests.js

`jest-canvas-mock`をインストールしていない状態でテストを走らせるとコケてしまうみたいです。

```shell:jest-canvas-mockのインストール
$ npm i --save-dev jest-canvas-mock

# or

$ yarn add -D jest-canvas-mock
```

インストールした上で

```diff_javascript:setupTests.js
  import '@testing-library/jest-dom';
+ import 'jest-canvas-mock';
```

`setupTests.js`でimportすればなおりました。

## まとめ

1. After EffectsにBodymovinをインストールする
1. After Effectsでアニメーションを作成する
    1. シェイプレイヤーかパスデータだけで作る
    1. 複雑なマスクを使わない
    1. 基本、エフェクトは使えないものと考える
1. BodymovinでJSONに書き出す
1. `react-lottie`をインストールして、書き出したJSONを読み込んで`Lottie`コンポーネントを配置する
1. 各種オプションを調整する