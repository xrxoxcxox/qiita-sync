<!--
title:   Next.js + Framer MotionでShared Element Transition
tags:    React,SharedElementTransition,animation,next.js,translation
id:      da1076d4d9ace637788e
private: false
-->
## この記事の概要

モバイルアプリを触っているとビューの遷移にShared Element Transitionが使われていることが多いです。
Webだとあまりメジャーではない気がしますが、上手いことやってみたくてチャレンジしました。

ネイティブアプリほどのスムーズさは実現できませんでしたが、まあほどほど……くらいにはなったので1度記事にしてみます。
より上手くできた方がいたら是非教えてください。

## 執筆時の環境

| パッケージ | バージョン |
| --- | --- |
| framer-motion | 6.3.6 |
| next | 12.1.6 |
| react | 18.1.0 |
| react-dom | 18.1.0 |

## 完成形

最初に今回完成したものを載せておきます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/666e779e-5488-4c87-f27e-7a1acec09889.gif)

トップページに記事一覧がサムネイル表示されていて、クリックすると詳細ページへジャンプ。
共通の要素である画像とタイトルがページをまたいで変化する、というイメージです。

左下に映っている`⌘[`は、ブラウザバックしたした瞬間を分かりやすくするために表示したショートカットです。

## 作り方

### アニメーションなしのページを作る

まずはアニメーションのないページを作ります。

以下はトップページです。
スタイルは大したことを書いていないので省略します。

リンクのループを`[...Array(12)].map((_, i)`で済ませたり、画像のファイル名を`0.jpg`,`1.jpg`と雑に命名したりしています。
あくまでアニメーションの検証用で書いたコードなのでここは適宜読み替えてください。

```jsx:index.js
import Image from "next/image";
import Link from "next/link";
import styles from "../styles/Home.module.css";

export default function Home() {
  return (
    <div className={styles.container}>
      <h1 className={styles.topTitle}>Home</h1>
      {[...Array(12)].map((_, i) => (
        <Link href={`/${i}`} key={i}>
          <a className={styles.image}>
            <div>
              <Image src={`/${i}.jpg`} alt="" width={368} height={207} />
            </div>
            <h2 className={styles.imageName}>
              Image {i}
            </h2>
          </a>
        </Link>
      ))}
    </div>
  );
}
```

見た目はこんな感じです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7db958c5-4301-6c71-924b-2b7d424254c8.png)

対して詳細のページは以下です。
こちらもファイル名から酷いものですが、目を瞑ってください。
現状はトランジションするものをイメージして`<Image />`とその後の見出しを同じ見た目にしています。
見出しについては、トップページでは`h2`で詳細ページでは`h1`ですがここは問題ありません。

```jsx:[id].js
import Image from "next/image";
import { useRouter } from "next/router";
import styles from "../styles/Home.module.css";

export default function Id() {
  const {
    query: { id },
  } = useRouter();
  return (
    <div className={styles.container}>
      <div className={styles.centerImage}>
        <Image src={`/${id}.jpg`} alt="" width={752} height={423} />
      </div>
      <h1 className={styles.pageTitle}>
        Image {id}
      </h1>
      <div className={styles.paragraph}>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
          eiusmod tempor incididunt ut labore et dolore magna aliqua.
        </p>
        <p>
          minim veniam, quis nostrud exercitation ullamco laboris nisi ut
          aliquip ex ea commodo consequat.
        </p>
        <p>
          Duis aute irure dolor in reprehenderit in voluptate velit esse cillum
          dolore eu fugiat nulla pariatur.
        </p>
        <p>
          Excepteur sint occaecat cupidatat non proident, sunt in culpa qui
          officia deserunt mollit anim id est laborum.
        </p>
      </div>
    </div>
  );
}
```

見た目はこんな感じです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4a3c18bd-9f14-1c6f-ef4e-3c4b4b313346.png)

### Framer Motionをインストールしてアニメーションをつける

React用アニメーションライブラリの[Framer Motion](https://www.framer.com/motion/)をインストールします。

```sh:ターミナル
npm i framer-motion
```

まず、先ほどのコードのうち動かしたい要素を`motion.ElementName`の書式に変えます。
（対象が`div`であれば`motion.div`という具合です。）

```diff_jsx:index.js
        <h1 className={styles.topTitle}>Home</h1>
        {[...Array(12)].map((_, i) => (
          <Link href={`/${i}`} key={i}>
            <a className={styles.image}>
-             <div>
+             <motion.div>
                <Image src={`/${i}.jpg`} alt="" width={368} height={207} />
-             </div>
+             </motion.div>
-             <h2 className={styles.imageName}>
+             <motion.h2 className={styles.imageName}>
                Image {i}
-             </h2>
+             </motion.h2>
            </a>
          </Link>
        ))}
```

```diff_jsx:[id].js
-       <div className={styles.centerImage}>
+       <motion.div className={styles.centerImage}>
          <Image src={`/${id}.jpg`} alt="" width={752} height={423} />
-       </div>
+       </motion.div>
-       <h1 className={styles.pageTitle}>
+       <motion.h1 className={styles.pageTitle}>
          Image {id}
-       </h1>
+       </motion.h1>
        <div className={styles.paragraph}>
          <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua.
          </p>
          <p>
            minim veniam, quis nostrud exercitation ullamco laboris nisi ut
            aliquip ex ea commodo consequat.
          </p>
          <p>
            Duis aute irure dolor in reprehenderit in voluptate velit esse cillum
            dolore eu fugiat nulla pariatur.
          </p>
          <p>
            Excepteur sint occaecat cupidatat non proident, sunt in culpa qui
            officia deserunt mollit anim id est laborum.
          </p>
        </div>
```

そうしたら、ページをまたいでも画像と見出しが紐付くように`layoutId`を付与します。
これまた命名が雑ですが……雰囲気を感じてください。

どちらのページでも、画像は`image-0` `image-1` `image-2`となり、見出しも同様に`title-0` `title-1` `title-2`となり、紐付きました。

```diff_jsx:index.js
        <h1 className={styles.topTitle}>Home</h1>
        {[...Array(12)].map((_, i) => (
          <Link href={`/${i}`} key={i}>
            <a className={styles.image}>
-             <motion.div>
+             <motion.div layoutId={`image-${i}`}>
                <Image src={`/${i}.jpg`} alt="" width={368} height={207} />
              </motion.div>
-             <motion.h2 className={styles.imageName}>
+             <motion.h2 layoutId={`title-${i}`} className={styles.imageName}>
                Image {i}
              </motion.h2>
            </a>
          </Link>
        ))}
```

```diff_jsx:[id].js
-       <motion.div className={styles.centerImage}>
+       <motion.div layoutId={`image-${id}`} className={styles.centerImage}>
          <Image src={`/${id}.jpg`} alt="" width={752} height={423} />
        </motion.div>
-       <motion.h1 className={styles.pageTitle}>
+       <motion.h1 layoutId={`title-${id}`} className={styles.pageTitle}>
          Image {id}
        </motion.h1>
        <div className={styles.paragraph}>
          <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua.
          </p>
          <p>
            minim veniam, quis nostrud exercitation ullamco laboris nisi ut
            aliquip ex ea commodo consequat.
          </p>
          <p>
            Duis aute irure dolor in reprehenderit in voluptate velit esse cillum
            dolore eu fugiat nulla pariatur.
          </p>
          <p>
            Excepteur sint occaecat cupidatat non proident, sunt in culpa qui
            officia deserunt mollit anim id est laborum.
          </p>
        </div>
```

実はアニメーション自体は以上で完成です。
あとはFramer Motionが良い感じに遷移してくれます。

## スクロール位置

ただ、このままだとスクロール位置がおかしなことになります。
下の方のリンクから詳細ページに遷移してブラウザバックすると、ページのトップに戻ってしまうのでやや違和感……。

というわけで`next.config.js`をいじります。

Create Next Appで生成された状態からの差分を以下に示します。

```diff_javascript:next.config.js
  /** @type {import('next').NextConfig} */
  const nextConfig = {
    reactStrictMode: true,
+   experimental: {
+     scrollRestoration: true
+   }
  }

  module.exports = nextConfig
```

`experimental`ではありますが、`scrollRestoration`をtrueにするとスクロールした位置を保持してくれます。
これで、冒頭の動画とほぼ同じ動きをするコードが完成しました。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/666e779e-5488-4c87-f27e-7a1acec09889.gif)

## 無念な箇所

`next/image`を使うとページが切り替わる際に一瞬ちらつきが発生してしまいました。

[こちらのIssue](https://github.com/vercel/next.js/discussions/20991)を見るにかつては解決していたようなのですが、自分の手元では上手くいかず……。

画像のちらつきがなくなると更にスムーズに見えるとは思うのですが、現段階ではギブアップです。

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox