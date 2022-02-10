<!--
title:   CSS in JS時代のCSS設計
tags:    CSS,CSS設計,React,emotion,styled-components
id:      fc8542cadc40ef2505eb
private: false
-->
[Ateam Lifestyle Advent Calendar 2019](https://qiita.com/advent-calendar/2019/ateam-lifestyle)の1日目は
株式会社エイチームライフスタイル クリエイティブ戦略部 シニアデザイナーの綿貫が担当します！

先陣切って投稿しますので、みなさんどうぞ読んでください！

## はじめに

CSS in JSもそこまで珍しくなくなってきたと思いますが、CSS in JSでのCSS設計の話はなかなか見かけません。
ないなら自分で書こう！と思い立ったので書きます。

## そもそもCSS設計が必要な理由

CSSを書くに当たって、人間が悩むのは主に「**名前空間が存在していない**」からだと思います。
**1つ1つのパーツ**を**個別に**スタイリングをするだけならさほど難しくもありませんよね？

ところが、サイト全体を通してスタイリングするとなると

* 自分が触っている箇所に
    * 予期せぬスタイルが当たっている
    * 当たるはずのスタイルが当たらない
* 自分が触っていない箇所に
    * 予期せぬスタイルが当たっている

などに悩まされるかと思います。

大抵の場合は**カスケーディング**や**セレクタの詳細度**が絡み合い事故を起こすのですが、**名前空間を持っていれば**防げたであろうケースも多いのです。

こういったつらさを解消するためにCSS設計が必要であり、色々な種類のものが考案されてきました。
この記事では詳しくは触れませんが、一例を挙げますので気になる方は読んでみてください。

* [SMACSS](http://smacss.com/ja)
* [BEM](https://en.bem.info/)
* [FLOCSS](https://github.com/hiloki/flocss)

## CSS in JSについて

ここまで散々CSS in JSという言葉を使ってきましたが、これは**ある特定の技術**を指すものではありません。
言葉通り**「JavaScriptの中でCSSを書く」という概念**です。

実際にコードを書く際にはライブラリを選ぶところから始まります。

有名どころだと以下のもの。

* [CSS Modules](https://github.com/css-modules/css-modules)（厳密に言うと違うけどCSS in JSの文脈で出てくることが多い）
* [styled components](https://www.styled-components.com)
* [Emotion](https://emotion.sh/)

CSS Modulesは学習コストが一番低く、styled componentsは普及している範囲が一番広く、Emotionは最近追い上げて来ている&個人的に好きです。

どれも共通しているのは、**擬似的にローカルスコープを生成してセレクタを閉じ込められる**点。
これが出来るようになったおかげで命名に悩む時間は大幅に減りました。

では名前空間の問題が解消した上で、それでもなお、人間が設計しないといけないことは何でしょうか？

## CSS in JSでの設計とこれまでの設計との違い

大きく分類すれば

* グローバルに使うものとそうでないものを明らかにする
* propsによるスタイルの変化を考慮する
* どの機能・オプションを使うのかを明示する

ことに分かれると思います。

### グローバルに使うものとそうでないものを明らかにする

UIをコンポーネントとして実装する際のメリットは**それぞれが独立している**ことです。
スタイリングにおいても大きなメリットですが、そうは言ってもグローバルに共通化したいものもあります。

代表例はカラーパレット。
特にブランドカラーを定義している際は、毎回手打ちで色を指定するのは非現実的です。

あるいはreset CSSの類。
各コンポーネントにいちいち記述するのは絶対に嫌です。

かといってなんでもかんでも共通化していては、せっかくのローカルスコープが意味をなさなくなってしまうのが悩みどころ。
チームやプロダクトによりますが、どこまでを共通化してどこからは各コンポーネントに閉じ込めるかをしっかり話し合うのが大事だと思います。

範囲さえ決めることが出来れば、先に挙げたライブラリならどれでもglobalなスタイルも定義できます。
また、定数や関数を他で定義しておいて読み込めばいつでも使いまわせます。

### propsによるスタイルの変化を考慮する

CSS in JSで楽になることの1つとして、propsを渡してスタイルを変更できることが挙げられます。
選ぶライブラリによって実装方法は変わりますが、例えばstyled componentsなら以下のようにバリエーションを生成できます。

1. `Input`コンポーネントに`inputColor`が指定されていたらcolorがその値になる
2. 指定されていなかったらフォールバックとして`palevioletred`が適用される

```jsx
// 公式ドキュメントよりコードを抜粋
const Input = styled.input`
  padding: 0.5em;
  margin: 0.5em;
  color: ${props => props.inputColor || "palevioletred"};
  background: papayawhip;
  border: none;
  border-radius: 3px;
`;

render(
  <div>
    <Input defaultValue="@probablyup" type="text" />
    <Input defaultValue="@geelen" type="text" inputColor="rebeccapurple" />
  </div>
);
```

しかし、なんでもかんでもpropsで渡せるようにしてしまっては記述が汚くなります。
やろうと思えば……

```jsx
// 先ほどのコードを改変
const Input = styled.input`
  padding: ${props => props.inputPadding || 0.5}em;
  margin: ${props => props.inputMargin || 0.5}em;
  color: ${props => props.inputColor || "palevioletred"};
  background: ${props => props.inputBackground || "papayawhip"};
  border: none;
  border-radius: ${props => props.inputBorderRadius || 3}px;
`;

render(
  <div>
    <Input defaultValue="@probablyup" type="text" />
    <Input
      defaultValue="@geelen"
      type="text"
      inputPadding="2"
      inputMargin="1"
      inputColor="rebeccapurple"
      inputBackground="red"
      inputBorderRadius="5"
    />
  </div>
);
```

かなり嫌なコードですよね？
全てのカスタムを許せば良いってものじゃありませんし、`Input`はもはやコンポーネントとして成立しているのか怪しいです。

こういった場合は以下のような書き方にすることもできます。

```jsx
// 更に改変
const Input = styled.input`
  padding: 0.5em;
  margin: 0.5em;
  color: palevioletred;
  background: papayawhip;
  border: none;
  border-radius: 3px;
  ${props => props.primary && css`
    padding: 1em;
    margin: 1em;
    color: rebeccapurple;
    background: red;
    border-radius: 5px;
  `}
`;

render(
  <div>
    <Input defaultValue="@probablyup" type="text" />
    <Input defaultValue="@geelen" type="text" primary />
  </div>
);
```

これなら自由すぎるカスタム性はなくなり、`Input`の書き方もスッキリしました。

ただし、これだけ変わるなら別コンポーネントとして定義した方が良いのでは？とか、
今後`secondary`や`tertiary`が出てきたらどうなるのか？とか
propsの渡し方はboolとenumのどちらの方が良いのか？とか
疑問が次々湧きますよね。

色々なやり方でpropsをの受け渡しとスタイルの変更ができてしまうため、**何を許容して何を縛るかの設計**が大事になるかと思います。

### どの機能・オプションを使うのかを明示する

上の話とも少し似ています。
今CSS in JSのライブラリはお互いがしのぎを削っているからか、各種オプションが非常に豊富です。

* オブジェクト記法でも書けるし、テンプレートリテラルでも書ける
* component化もできるし、インラインで直接も書ける
* jsx内にも書けるし、外にconstとしても書ける

などなど……。
アレコレできるようになった反面、縛りは入れないと非常に読みづらくなります。

例えばEmotionは最後発なのでかなり色々な書き方ができるのですが、以下を見てください。

```jsx

/** @jsx jsx */
import { css, jsx } from '@emotion/core'

const text = css`
  font-size: 20px;
  padding: 10px;
`

const P = props => (
  <p
    css={{
      margin: 0,
      fontSize: 12,
      lineHeight: '1.5',
      fontFamily: 'Sans-Serif',
      color: 'black'
    }}
    {...props}
  />
)

render(
  <div
    css={{
      backgroundColor: 'hotpink',
      '&:hover': {
        color: 'lightgreen'
      }
    }}
  >
    <P css={text}>This has a hotpink background.</P>
  </div>
)
```

最終的にどんなスタイルになるのか全然わかりませんよね？
自分にも分かりません。

書いている本人ですらシミュレートするのに一苦労なのですから、後から読んだ人には分かるはずもありません。
なので初めにチームでどの機能・オプションを使うのかをしっかり決めておくことが大事です。

## これまでの設計から変わらないこと

既存のCSS設計（あるいはコーディングルール）から変わらないことも多くあります。

まずは、セレクタのスコープが絞られるようになったからと言って、**要素セレクタや過度な入れ子**はよろしくないということです。

Shadow DOMが実現出来ているわけではありませんので、要素セレクタでスタイルを宣言すると大抵何かと衝突します。
入れ子については記述が複雑になる他、パフォーマンスの観点でも良くありません。

あとは、最終的にアウトプットされるのはあくまでCSSです。
marginの相殺なんかはローカルスコープがあろうが発生してしまうもの。
そのためまだまだ最終的なCSSをイメージしながら書く必要はあります。

## 具体的なルール

ここまでは概念的な話がほとんどでした。
詳しく書きたいところなのですが、色々なライブラリが存在するので「これだ！」というものは無いと思います。

そんな中でもEmotionを使った詳細な書き方は[こちらの記事](2019-10-30_CSS_React_css-in-js_emotion_5a520eea36232d0337bc.md)で紹介しているので良ければ一緒に見てください。

## まとめ

CSS in JSにおけるCSS設計の概念をまとめてみました。
まだベストプラクティスが出揃っていない分野だとは思うので、これから使う人にとって少しでも参考になれば幸いです。

[Ateam Lifestyle Advent Calendar 2019](https://qiita.com/advent-calendar/2019/ateam-lifestyle) の2日目は、@tatsumin0206がお送りします！！どんなネタを用意してくるのか楽しみです！！