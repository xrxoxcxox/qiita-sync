<!--
title:   自分流: Emotion(CSS in JS)でのスタイルの書き方
tags:    CSS,CSS設計,React,css-in-js,emotion
id:      5a520eea36232d0337bc
private: false
-->
## この記事について

### 内容を一言でまとめると

* CSS in JSのライブラリであるEmotionを使ってどのようにスタイルを書いているか
    * [自分のポートフォリオサイト](https://www.keisukewatanuki.work/)はこの記事に書いたようにスタイルを当てています

### 何故書いたか

* Emotionの機能紹介のような記事は多いけど、実際のプロダクトに当てはめて書き方を紹介している例が少ないなあと思ったから
* 自分なりに考えて一旦書き方が定まってきたから
* 今のやり方を公開すればこれを叩き台にしてもっと良いやり方が生まれていく可能性もあるから

### こんな人が読んでくれたら嬉しい

* CSS in JSのライブラリは一杯あるけど、それぞれ具体的なメリットデメリットがわからん！と困っている人
* npm trendsでstyled-componentを抜いたし、気にはなっているけど若干尻込みしている人
* 実際にEmotionを使っているけど書き方が定まらない人

## 目次

* css Propを使用する
* reset cssなど全体に渡るものは`Global`に指定しておく
* 色、フォントサイズ、グリッドのサイズは定数で指定する
* アニメーションは極力`keyframes`を利用してCSSアニメーションで実装
* ケーススタディ
* まとめ

## css Propを使用する
* css Propの中でもString Stylesの書き方で書く
    * CSSのプロパティとして調べるにしても、過去のアセットをコピペするにしてもObject Stylesだとしんどい
* styled componentsは使用しない（emotion/styled自体インストールしていない）
    * スタイリングのためだけのコンポーネントが増えすぎる
    * ファイルとして分けたコンポーネントと、コンポーネントの中でローカルに（？）増えていくコンポーネントが対応しなくて管理がつらい

## 基本的には全ての要素に`css={foo}`と指定する

* 以下のように、全てのエレメントに`css`を指定

```jsx:OK例
const container = css`
  background-color: #eee;
`

const text = css`
  color: hotpink;
`

render(
  <div css={container}>
    <p css={text}>this is hotpink</p>
  </div>
)
```

* 逆にNGなのは以下
    * 入れ子でタグを直接指定してスタイリングするとスコープが閉じなくなる
    * 例のコードだと`children`やComponent内で呼び出したComponentに`<p>`が存在しているとそれも`hotpink`になってしまう  
    （ならないようにも書けるけど頭の中で完璧にシミュレートするのは難しいので初めから採用しない）

```jsx:NG例
const container = css`
  background-color: #eee;
  p {
    color: hotpink;
  }
`

render(
  <div css={container}>
    <p>this is hotpink</p>
  </div>
)
```

* 例外→`ol, ul > li`や`dl > dt, dd`や`table > th, td`
    * `li`や`dt`、`td`1つ1つ全部にcss Propを渡すと余計にコーディングが大変になるため

```jsx:例外
const foo = css`
  background-color: #eee;
  li {
    padding: 5px 0;
  }
`

render(
  <ul css={foo}>
    <li>なんとか</li> 
    <li>かんとか</li>
    <li>うんたら</li>
    {/* li1つ1つに全部css={bar}とか書くのは変更が発生したときに漏れを起こしやすい */}
  </ul>
)
```

## reset cssなど全体に渡るものは`Global`に指定しておく

* Emotionに組み込まれている`Global`コンポーネントを使うと文字通りグローバルにスタイルを適用できる
* reset cssなどは初めにこの中で設定しておきトップレベルで呼ぶようにしておくと楽
* 注意点
    * `@emotion/core`から`Global`をimportする必要があることと
    * `css`を使っていた箇所を`styles`に変える必要があること
* 具体例

```jsx:Global
import { Global, css } from '@emotion/core'
{/* Globalを使用する際はimportの必要あり、通常はcssだけをimportすれば良いので記述を省いていた */}

const global = css`
  * {
    margin: 0;
    padding: 0;
  }
`

render(
  <Global styles={global}/>
)
```

## 色、フォントサイズ、グリッドのサイズは定数で指定する
* %指定がしたいとき、`before`や`after`で図形を描画するとき、`position`で要素を配置するときは直接値を書いても良い
    * CSSで矢印を描く……などのときは流石に定数では対応しきれないこともある
* 以下は自分のポートフォリオサイトで使っている定数の例
    * このように指定しておけばエディタが補完してくれる
    * 複数人で開発する際の「微妙に違うブルーが50種類くらい存在する」みたいな事象を回避できる

```jsx:Color.js
export default {
  White: '#fff',
  Gray20: 'f2f2f2',
  Gray50: '#dfe0e0',
  Gray80: '#c8c9ca',
  Gray100: '#abacae',
  Gray300: '#8b8e90',
  Gray400: '#6c6f72',
  Gray600: '#4d5154',
  Gray700: '#2f3438',
  Gray800: '#161c20',
  Black: '#040b0e',
  Blue: '#0074af',
}
```

## アニメーションは極力`keyframes`を利用してCSSアニメーションで実装
* アニメーション用のライブラリを使った方が短期的には楽かもしれないけど、ライブラリの選定自体が難しかったり後から使いづらくなると修正が難しくなったりするので自分で書くのを前提とする
* JSを使ってアニメーションをするとレンダリングブロックの原因にもなる
* 以下に示す例では[react-intersection-observer](https://www.npmjs.com/package/react-intersection-observer)を使用
    * これ自体の説明は省略、要はタイミング制御用にJSでクラスを付け外しするにとどめるという話

```jsx:アニメーションの例
const text = css`
  color: hotpink;
  opacity: 0;
  transform: translateY(-10px);
`

const fadeIn = keyframes`
  100% {
    opacity: 1
    transform: translateY(0);
  }
`

const animation = css`
  animation: ${fadeIn} 1s ease-out both;
`

const Component = () => {
  const [ref, inView] = useInView({
    rootMargin: '10px',
    threshold: 0,
  })
 
  return (
    <p ref={ref} css={[text, inView && animation]}>
      animation
    </p>
  )
}
```

## ケーススタディ

以下は実際に迷ったケースの紹介と採用した書き方の紹介

### コンポーネントが入れ子になる際のスタイリング

* 前提
    * 便宜上呼び出す側を「親コンポーネント」、呼び出される側を「子コンポーネント」と呼ぶ
    * 複数の子コンポーネントが存在して、それぞれ処理は違えどスタイルは同じとする
* 親コンポーネント内では以下のようにcss Propをexportする

```jsx:Parent
export const styles = css`
  background-color: #eee;
  padding: 10px;
  width: 100%;
`
```

* 子コンポーネントでは以下のようにcss Propをimportして、jsx内ではimportしたcss Propを割り当てるのみ
  * なお、親Componentからexportされたcss Prop以外をimportしてはしっちゃかめっちゃかになるのでNG

```jsx:Child
import { styles } from './Parent'

render(
  <div css={styles}>
    {/* 子コンポーネントの中身を書く */}
  </div>
)
```

### 基本スタイルは一緒でバージョン違いのComponentがある場合

* 前提
    * primaryなボタンとsecondaryなボタンがある
    * 文字サイズや角丸は一緒だけど文字色と背景色とボタンの幅が違う
* scssでいう`@extend`が出来るのでそれを使う
    * 先に定義したスタイルを`${}`で囲って呼び出す、そのあとに上書きしたいプロパティを記述する

```jsx
const button = css`
  border-radius: 4px;
  padding: 20px;
  font-size: 16px;
  font-weight: bold;
`

const primaryButton = css`
  ${button} {/* 基本のボタンのスタイルを引き継いでいる */}
  color: white;
  background-color: hotpink;
  width: 100%;
`

const secondaryButton = css`
  ${button} {/* 基本のボタンのスタイルを引き継いでいる */}
  color: black;
  background-color: gray;
  width: 200px;
`

const PrimaryButton = () => {
  <button css={primaryButton}>primaryボタン</button>
  {/* 文字色が白、背景色がピンク、幅が100%のボタン */}
}

const secondaryButton = () => {
  <button css={secondaryButton}>secondaryボタン</button>
  {/* 文字色が黒、背景色がグレー、幅が200pxのボタン */}
}
```

### Componentにはマージンを付けたくないが、実際の配置ではマージンが必要な場合

* Attaching Propsを使って処理する
    * 通常、UIコンポーネントそのものにマージンをつけるのは良くない
    * しかし毎回コンポーネントを`div`で囲ってレイアウト用のスタイルを書くのもマークアップが汚くなるのでこういうやり方
    
```jsx:コンポーネント自体
{/* SomeComponentそのもののスタイル */}
const child = css`
  background-color: gray;
  border-radius: 4px;
  padding: 20px;
`

const ChildComponent = props => (
  <div css={child} {...props}>
    {/* ここにComponentの中身が書いてある */}
  </div>
)
```

`jsx:コンポーネントを呼び出す側
{/* SomeComponentをレイアウトするためのスタイル */}
const layout = css`
  margin-top: 20px;
`

render(
  <SomeComponent css={layout}/>
  {/* SomeComponentで指定しているcss={child}に加えてcss={layout}の内容も当たっている
      = margin-top:20pxが適用されている */}
)
`

## まとめ

初めにも書いた通り、まだまだ良い書き方はたくさんあると思います。
しかし、自分自身が最初はかなり苦労しながら書き方の方向性を模索していたので、誰かにとっての手助けになれば嬉しいです。