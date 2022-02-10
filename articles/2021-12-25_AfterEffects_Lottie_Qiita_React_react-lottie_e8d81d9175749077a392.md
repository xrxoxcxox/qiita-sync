<!--
title:   QiitaのLGTMボタンにアニメーションがつくまでの流れ
tags:    AfterEffects,Lottie,Qiita,React,react-lottie
id:      e8d81d9175749077a392
private: false
-->
[Qiita株式会社 Advent Calendar 2021](https://qiita.com/advent-calendar/2021/qiita)（2）の12日目の担当は、CX向上グループの@xrxoxcxoxです！

https://qiita.com/advent-calendar/2021/qiita

## この記事の概要

https://twitter.com/Qiita/status/1471694203587821570

最近、LGTMボタンにアニメーションをつけました。
こちらをどのように実装していったのかを簡単に説明します。

## 全体の流れ

1. After Effectsでアニメーションを作り、Bodymovinでjson書き出し
1. Lottie(react-lottie)でReact上で動かす
1. 実際に触ってみて調整

### After Effectsでアニメーションを作り、Bodymovinでjson書き出し

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/78784e36-3276-fca0-6b2e-236bf92cb5df.png)

もととなるデータを作っていきましょう。

最終出力がsvgアニメーションになるのもあって、After Effectsの全ての機能が使えるわけではありません。
以下のことに気をつけながらアニメーションを考えます。

- シェイプレイヤーかパスデータだけで作る
- 複雑なマスクを使わない
- 基本、エフェクトは使えないものと考える

一旦仕上がったものはこんな感じ。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/18f240d3-652b-1d14-5c64-a7f4421f713e.gif)

Bodymovinのインストール方法や詳しい使い方は省いてしまいましたが、@naomunaomuさんの以下の記事が分かりやすいのでおすすめです。

https://qiita.com/naomunaomu/items/0038f8c23fd5175f1205

### Lottie(react-lottie)でReact上で動かす

コード上で動かすにあたり、公式から出ているライブラリは`lottie-web`です。

https://github.com/airbnb/lottie-web

QiitaのフロントはReactなのですが、上記のライブラリはReactには最適化されていません。
そのため、react-lottieを代わりに使いました。

https://github.com/chenqingspring/react-lottie

:::note warn
react-lottieは更新がしばらくないのが気になりますが、今回はスター数の多さで選びました。
もしご自分でも試してみようと思った場合は、他のライブラリも含めて検討してみてください。
:::

#### ハマった点

単に動かすだけであれば`Lottie`コンポーネントにアニメーションデータのjsonを渡すだけで良いのですが、Qiitaの場合はページに訪れた際の状態で分岐が発生します。

1. まだLGTMしていない記事の場合
    1. ボタンは未LGTM状態（0フレーム目）
    1. クリックするとアニメーションしてLGTM済みの状態（最終フレーム）に遷移する
    1. もう1度クリックすると未LGTM状態（0フレーム目）に戻る
    1. 2と3の繰り返し 
1. 既にLGTMしている記事の場合
    1. ボタンはLGTM済みの状態（最終フレーム）
    1. クリックすると未LGTM状態（0フレーム目）に戻る
    1. もう1度クリックするとアニメーションしてLGTM済みの状態（最終フレーム）に遷移する
    1. 2と3の繰り返し

何を当たり前のことを言っているんだと思われるかもしれません。

しかし、Lottieには「任意のフレームからスタートする」機能がないのです。

そのため2の「既にLGTMしている記事」に訪れた場合に「LGTM済みの状態（最終フレーム）」で提供できません。
色々工夫してみたものの、ページに訪れる度にアニメーションしてしまうとか、変な動きになってしまいました。

ちなみにだいぶ古いですがこのようなIssueも立っていて、ハック的な方法が示されています笑[^1]

[^1]: 更にこの方法は`lottie-web`ならできそうなものの`react-lottie`ではできないっぽかったです……。

https://github.com/airbnb/lottie-web/issues/847

#### 解決した方法

まずは以下の2つの要素を用意しました。

- LGTM済みの見た目の静的なsvg
- アニメーションするsvg（= Lottie）

そして2つを出し分けます。
実際のコードだと色々な要素が含まれているので、簡略化して以下に載せます。

```typescript:おおまかなイメージ
interface Props {
  isLiked: boolean
}

const LgtmButton = ({ isLiked }: Props) => {
  const [isAnimationWorked, setIsAnimationWorked] = useState(false)
  return (
    <button onClick={() => {setIsAnimationWorked(true)}>
      {!isAnimationWorked && isLiked ? (
        <LgtmCompleted />
      ) : (
        <Lottie
          isStopped={!isLiked}
          // その他、オプションの指定色々
        />
      )}
    </button>
  )
}
```

- `isLiked`はグローバルに管理、`isAnimationWorked`はローカルに管理
- ページに訪れた時点では`isAnimationWorked`は絶対にfalseなため、LGTM済みかどうかで分岐する
    - LGTM済みであれば`<LgtmCompleted />`（LGTM済みの見た目の静的なsvg）が表示される
    - 未LGTMであれば再生前の`<Lottie />`が表示される
- ボタンをクリックした時点で`isAnimationWorked`がtrueになる
    - LGTM済みでボタンをクリックした場合は`isLiked`がfalseになるため再生前の`<Lottie />`に切り替わる
    - 未LGTMでボタンをクリックした場合は`isLiked`がtrueになるためLottieの`isStopped`がfalseとなり、動き出す

かなり苦労しましたが、こういった方法で実現できました。

### 実際に触ってみて調整

さきほどAfter Effectsで作成したときと、実際に押せるボタンとして実装したときとではアニメーションの印象が若干違いました。

そのため、一度実装した後にタイミングやデュレーションなど少しずつ調整……。
数フレームずつ速くしたり遅くしたりを繰り返してみつつ、なんとか完成にこぎつけました。

## まとめ

- After Effectsで作ったアニメーションを簡単にjsonに書き出して使えるけど、エフェクトをつけるなど凝ったことはできないので注意
- Lottieは最終フレームでフリーズさせる機能がないので条件分岐で実装
- 最後は目で見て、実際に触って確認。