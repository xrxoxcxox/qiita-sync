<!--
title:   Reactでアンマウント時のアニメーションをつけるのにReact Transition Groupを使ったら簡単だった
tags:    React,cssアニメーション,emotion,react-transition-group,フロントエンド
id:      fecc35fe93b5c570169a
private: false
-->
## これは何

- Reactでライブラリに頼らずにunmount時のアニメーションをつけようと思うと意外と大変
- React Transition Groupを使うと楽
- 上記をまとめた記事です
- なお、この記事の内容は[こちらのリポジトリ](https://github.com/xrxoxcxox/qiita-react-transition-group)で公開しており、[GitHub Pages](https://xrxoxcxox.github.io/qiita-react-transition-group/)で実際の挙動を見ることもできます
- 「フロントエンド強化月間 - 開発する上で知っておくべき知見を共有しよう」イベントへの投稿記事でもあります。

https://qiita.com/official-events/b9ad63394fa2635dfca9

## 環境など

| パッケージ | バージョン |
| --- | --- |
| react | 17.0.2 |
| @emotion/react | 11.4.0 |
| react-transition-group | 4.4.1 |

## 当初遭遇した事象、状況

- Reactを使ってコードを書いている
- ボタンをクリックしたら要素が出たり消えたりするアニメーションを作りたかった
- 出現するときのアニメーションは問題ない
- 消えるときに瞬間的に消えてしまう

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0092e6a3-dc12-7092-613f-ab02228f7fa8.gif)

## 書いていたコード

若干長いですが、途中で出る用と消える用のキーフレームを指定してアニメーションをさせようとしていました。

```jsx
import { useState } from "react";
import "./App.css";
import { css, keyframes } from "@emotion/react";

function App() {
  const [open, setOpen] = useState(false);
  const toggleOpen = () => setOpen((currentState) => !currentState);
  return (
    <div className="App">
      <p css={title}>CSS Only</p>
      {open && (
        <div css={window} className={open ? "open" : "close"}>
          <img
            src="https://picsum.photos/360/240"
            alt=""
            css={image}
            width="360"
            height="240"
          />
        </div>
      )}
      <button css={button} onClick={toggleOpen}>
        {open ? "Close" : "Open"}
      </button>
    </div>
  );
}

const title = css`
  align-self: flex-end;
  color: #fff;
  font-size: 40px;
  font-weight: bold;
`;

const windowOpenKeyframes = keyframes`
  from {
    opacity: 0;
    transform: translateY(50%);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
`;

const windowCloseKeyframes = keyframes`
  from {
    opacity: 1;
    transform: translateY(0);
  }
  to {
    opacity: 0;
    transform: translateY(-20%);
  }
`;

const window = css`
  align-self: center;
  background-color: #fff;
  border-radius: 8px;
  color: #212121;
  font-size: 40px;
  justify-self: center;
  opacity: 0;
  padding: 20px;
  transform: translateY(0);
  transition-duration: 300ms;
  transition-property: opacity, transform;
  transition-timing-function: cubic-bezier(0, 0, 0.2, 1);
  width: 400px;
  &.open {
    animation: ${windowOpenKeyframes} ease 300ms both;
  }
  &.close {
    animation: ${windowCloseKeyframes} ease 300ms both;
  }
`;

const image = css`
  background-color: #eee;
  border-radius: 4px;
  display: block;
`;

const button = css`
  align-self: flex-start;
  background-color: #61dafb;
  border: none;
  border-radius: 8px;
  color: #212121;
  cursor: pointer;
  font-size: 20px;
  font-weight: bold;
  justify-self: center;
  padding: 20px 40px;
  text-decoration: none;
  width: 200px;
`;

export default App;
```

## 原因

分かってしまえば当たり前の話なのですが、Reactで要素がアンマウントされるときは一瞬で消えてしまいます。
`open`のstateがfalseになったら、`close`のクラスが付与されるより先に画面から消えてしまうため、アニメーションがおこるはずもありません。

## React Transition Groupの利用

http://reactcommunity.org/react-transition-group/

`open`のstate以外にもう1つstateを用意して、`open`がfalseになったらCSSのクラスを付与して、アニメーションが終わったらそのstateもfalseにして、両方falseになったら消える……
などと書けばライブラリを使わなくてもできるような気がしますが[^1]、ライブラリを使えば割と簡単に実装できました。

[^1]: もっと良いやり方があるかもしれません。もしあったら是非教えてください :pray: 

React Transition Groupには`<Transition>`と`<CSSTransition>`があります。
`<Transition>`はコンポーネントの中で更にstateを渡さないといけないので、今回は`<CSSTransition>`を選びました。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/87b8eae7-7e4f-dcf4-bb87-d7b33da7e2d2.gif)

```jsx
import { useState } from "react";
import "./App.css";
import { css, keyframes } from "@emotion/react";
import { CSSTransition } from "react-transition-group";

function App() {
  const [reactTransitionGroupOpen, setReactTransitionGroupOpen] =
    useState(false);
  const toggleReactTransitionGroupOpen = () =>
    setReactTransitionGroupOpen((currentState) => !currentState);
  return (
    <div className="App">
      <p css={title}>React Transition Group</p>
      <CSSTransition
        in={reactTransitionGroupOpen}
        timeout={200}
        classNames="react-transition-group"
        unmountOnExit
      >
        <div css={reactTransitionGroupWindow}>
          <img
            src="https://picsum.photos/360/240"
            alt=""
            css={image}
            width="360"
            height="240"
          />
        </div>
      </CSSTransition>
      <button css={button} onClick={toggleReactTransitionGroupOpen}>
        {reactTransitionGroupOpen ? "Close" : "Open"}
      </button>
    </div>
  );
}

const title = css`
  align-self: flex-end;
  color: #fff;
  font-size: 40px;
  font-weight: bold;
`;

const reactTransitionGroupWindow = css`
  align-self: center;
  background-color: #fff;
  border-radius: 8px;
  color: #212121;
  font-size: 40px;
  justify-self: center;
  padding: 20px;
  width: 400px;
  &.react-transition-group-enter {
    opacity: 0;
    transform: translateY(50%);
  }
  &.react-transition-group-enter-active {
    opacity: 1;
    transform: translateY(0);
    transition-duration: 300ms;
    transition-property: opacity, transform;
    transition-timing-function: cubic-bezier(0, 0, 0.2, 1);
  }
  &.react-transition-group-exit {
    opacity: 1;
    transform: translateY(0);
  }
  &.react-transition-group-exit-active {
    opacity: 0;
    transform: translateY(-20%);
    transition-duration: 150ms;
    transition-property: opacity, transform;
    transition-timing-function: cubic-bezier(0.4, 0, 1, 1);
  }
`;

const image = css`
  background-color: #eee;
  border-radius: 4px;
  display: block;
`;

const button = css`
  align-self: flex-start;
  background-color: #61dafb;
  border: none;
  border-radius: 8px;
  color: #212121;
  cursor: pointer;
  font-size: 20px;
  font-weight: bold;
  justify-self: center;
  padding: 20px 40px;
  text-decoration: none;
  width: 200px;
`;

export default App;
```

ポイントは`<CSSTransition>`に渡した`className`は1つ下の要素に受け継がれつつ、状態に合わせて色々なサフィックスがつくことです。

```jsx
const someComponent = () => {
  return (
    <CSSTransition className="fade">
      <div>I'm component</div>
    </CSSTransition>
  )
}
```

上記のようにした場合`div`に対して、状態に合わせて以下のクラスがついたり消えたりします。

- appear系
    - fade-appear
    - fade-appear-active
    - fade-appear-done
- enter系
    - fade-enter
    - fade-enter-active
    - fade-enter-done
- exit系
    - fade-exit
    - fade-exit-active
    - fade-exit-done

今回の例では以下の4つのものを使いました。

- `className`-enter
    - 今まさにマウントされた！という瞬間につくクラス
- `className`-enter-active
    - マウントされて、出現するアニメーションをしている最中につくクラス
- `className`-exit
    - 今からアンマウントされる！という瞬間につくクラス
- `className`-exit-active
    - アンマウントされるにあたって消えゆくアニメーションをしている最中につくクラス

それ以外にも例えばフェードインで出現し、アニメーションが終わったら更に背景色が変わって欲しい、とあらば`className-enter-done`のクラスに対してCSSを指定します。

## まとめ

- Reactにおいて、他のライブラリに頼らずアンマウント時のアニメーションを書くのは大変
- React Transition Groupを使えばアンマウントが始まる瞬間や消えゆく時間、消えきった瞬間などタイミングを細かく指定してスタイリングできる