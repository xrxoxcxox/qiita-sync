<!--
title:   Next.jsでビルトインサポートされているCSSを比較してみた
tags:    css-in-js,css-modules,next.js,styled-jsx,フロントエンド
id:      e7202a73f30d58b76229
private: false
-->
## これは何

- [Next.js](https://nextjs.org/)では[CSS Modules](https://github.com/css-modules/css-modules)と[styled-jsx](https://github.com/vercel/styled-jsx)がビルトインサポートされており、その2つを比較してみた記事です
- また[フロントエンド強化月間 - 開発する上で知っておくべき知見を共有しよう](https://qiita.com/official-events/b9ad63394fa2635dfca9)イベントへの投稿記事でもあります

https://qiita.com/official-events/b9ad63394fa2635dfca9

## 作った物

| Figma | コーディングした結果 |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ce0bdddf-6b1b-e92c-6ac3-d6f3b2ef8e5a.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c82c387f-a2fd-cb4f-b30d-03446b381b0b.png) |

何かしらを投稿するサイトっぽい見た目で作ってみました。
（検証のために作っただけなのでモバイル対応はしていません。大変な崩れ方をします。）

GitHubで全てのコードを公開しているので良かったら見てください。

https://github.com/xrxoxcxox/qiita-css-in-js-comparison

### 機能比較のための項目

- SCSSの導入
    - CSS自体はビルトインサポートだけどSCSSはサポートされていない
- コンポーネント自体のスタイルと親から渡すスタイルを分ける
    - コンポーネント自体の色や文字の大きさなど
    - 親要素から渡すgridやmarginなど
- propsを渡してコンポーネントの見た目を変える
    - ボタンのコンポーネント
        - 塗りと線のバージョン
        - 大きめサイズと小さめサイズのバージョン
    - 投稿記事のコンポーネント
        - 画像有りと無しのバージョン

## SCSSの導入

ビルトインサポートのスタイリングを比較していますが、なんだかんだSCSS記法は欲しくなると思います。
（疑似要素やメディアクエリなど）

### CSS Modules

まずは`sass`のインストール。

```shell
npm install sass
```

次は`next.config.js`の変更。

```javascript
const path = require('path')

module.exports = {
  sassOptions: {
    includePaths: [path.join(__dirname, 'styles')],
  },
}
```

なお、公式のドキュメントに記載があります。

https://nextjs.org/docs/basic-features/built-in-css-support#sass-support

### styled-jsx

まずは`@styled-jsx/plugin-sass`のインストール。
こちらでも`sass`は必要なんですね。

```shell
npm install sass @styled-jsx/plugin-sass
```

そして`.babelrc`の編集

```json
{
  "presets": [
    [
      "next/babel",
      {
        "styled-jsx": {
          "plugins": ["@styled-jsx/plugin-sass"]
        }
      }
    ]
  ]
}
```

こちらは`Next.js`のドキュメントには記載がありませんが、パッケージのGitHubに詳細が書いてあります。

https://github.com/Thream/styled-jsx-plugin-sass

## コンポーネント自体のスタイルと親から渡すスタイルを分ける

コンポーネントそのもののスタイル（色や文字の大きさやpadding）はコンポーネントに書いて、ページ上でどうレイアウトするかは親から渡した方が良いです。
ボタンのコンポーネントと、それがヘッダーの中にあるときを例にして説明します。

### CSS Modules

`props`として受け取った`className`を`button`の`className`に渡せるようにすることで、marginやgrid-areaの指定などは親から渡せるようになっています。
直接は関係ありませんが、クラス名を複数持たせるには`clsx`など別途ライブラリを使う必要があります。

```jsx:CSSModulesButton.tsx
import { FC, ReactNode } from 'react'
import styles from '../../styles/css-modules-button.module.scss'
import clsx from 'clsx'

type InputProps = JSX.IntrinsicElements['button']
type Props = {
  children: ReactNode
  className?: string
  size: 'm' | 's'
  variant: 'fill' | 'border'
  onClick?: (event: React.MouseEvent<HTMLInputElement>) => void
} & InputProps

export const CssModulesButton: FC<Props> = ({
  children,
  className,
  size,
  variant,
  onClick,
}) => {
  return (
    <button
      onClick={onClick}
      className={clsx(styles.button, styles[variant], styles[size], className)}
    >
      {children}
    </button>
  )
}
```

親要素であるヘッダーは、普通にclassNameを指定しているだけです。

```jsx:CssModulesHeader.tsx
import { FC } from 'react'
import Image from 'next/image'
import { CssModulesButton } from './CssModulesButton'
import styles from '../../styles/css-modules-header.module.scss'

export const CssModulesHeader: FC = () => {
  return (
    <header className={styles.header}>
      <div className={styles.header__inner}>
        <h1 className={styles.header__logo}>
          <Image src="/logotype.svg" width={116} height={24} alt="Logotype" />
        </h1>
        <CssModulesButton
          size="m"
          variant="fill"
          className={styles.header__button}
          onClick={() => alert('新規作成')}
        >
          新規作成
        </CssModulesButton>
        <img
          src="https://source.unsplash.com/random/80x80"
          width="40"
          height="40"
          alt="User Icon"
          className={styles.header__icon}
        />
      </div>
    </header>
  )
}
```

### styled-jsx

ボタンのコンポーネントはCSS Modulesとほとんど同じです。
こちらは複数のクラス名を持つにしても特別な準備は必要ありません。

余談ですが、`<style jsx></style>`の中に直接スタイルを書くと見通しが悪くなりすぎるので外に出すようにしています。

```jsx:StyledJSXButton.tsx
import { FC, ReactNode } from 'react'
import css from 'styled-jsx/css'

type InputProps = JSX.IntrinsicElements['button']
type Props = {
  children: ReactNode
  className?: string
  size: 'm' | 's'
  variant: 'fill' | 'border'
  onClick?: (event: React.MouseEvent<HTMLInputElement>) => void
} & InputProps

export const StyledJSXButton: FC<Props> = ({
  children,
  className,
  size,
  variant,
  onClick,
}) => {
  return (
    <>
      <button
        onClick={onClick}
        className={`button ${variant} ${size} ${className}`}
      >
        {children}
      </button>
      <style jsx>{buttonStyle}</style>
    </>
  )
}

const buttonStyle = css`
  {/* 省略 */}
`
```

スタイルを渡す側の記述は若干面倒です。

```jsx:StyledJSXHeader.tsx
import { FC } from 'react'
import Image from 'next/image'
import { StyledJSXButton } from './StyledJSXButton'
import css from 'styled-jsx/css'

export const StyledJSXHeader: FC = () => {
  return (
    <>
      <header className="header">
        <div className="header__inner">
          <h1 className="header__logo">
            <Image src="/logotype.svg" width={116} height={24} alt="Logotype" />
          </h1>
          <StyledJSXButton
            size="m"
            variant="fill"
            className={`header__button ${className}`}
            onClick={() => alert('新規作成')}
          >
            新規作成
          </StyledJSXButton>
          <img
            src="https://source.unsplash.com/random/80x80"
            width="40"
            height="40"
            alt="User Icon"
            className="header__icon"
          />
        </div>
      </header>
      <style jsx>{headerStyle}</style>
      {styles}
    </>
  )
}

const headerStyle = css`
  {/* 省略 */}
`

const { className, styles } = css.resolve`
  .header__button {
    margin-left: auto;
  }
`
```

`css.resolve`という記法で書く必要があります。
また、スタイルを渡したいコンポーネント（ここでいうと`StyledJSXButton`）のclassNameは以下のように書いて

```jsx
className={`header__button ${className}`}
```

`<style jsx>`とは別に`{styles}`も記載しなければなりません。
`css.resolve`で定義されているのが`className, styles`と名前被り必至なのが微妙に悩ましいです。

## propsを渡してコンポーネントの見た目を変える

またもボタンを例に出しますが、propsを渡して以下を切り替えられるようにします。

- 塗りのボタンと線のボタン
- 大きめサイズと小さいサイズ

### CSS Modules

```jsx:CssModulesButton.tsx
export const CssModulesButton: FC<Props> = ({
  children,
  className,
  size,
  variant,
  onClick,
}) => {
  return (
    <button
      onClick={onClick}
      className={clsx(styles.button, styles[variant], styles[size], className)}
    >
      {children}
    </button>
  )
}
```

`scss:css-modules-button.module.scss
.button {
  border-color: transparent;
  border-radius: 2px;
  border-style: solid;
  border-width: 1px;
  font-weight: bold;
  padding: 7px 15px;
}

.fill {
  background-color: var(--color-primary);
  color: var(--color-light-text-high);
  &:hover {
    background-color: var(--color-primary-dark);
  }
}

.border {
  border-color: var(--color-primary);
  color: var(--color-dark-text-medium);
  &:hover {
    background-color: var(--color-background);
    color: var(--color-dark-text-high);
  }
}

.m {
  font-size: var(--font-size-body1);
}

.s {
  font-size: var(--font-size-body3);
}
`

`.button`のクラスは常についていて、渡ってくるpropsにあわせて`styles[variant], styles[size]`が上手く切り替わって付与されます。

また、CSS Modulesの記事を見ると`styles.className`の記法が紹介されていますが、ドットで繋げても動かないため`styles[className]`と記載する必要があります。
よくよく考えれば分かるのですが、自分はここに苦戦してなかなか実現できませんでした。

### styled-jsx

例が若干悪かったかもしれません……ほぼCSS Modulesと変わらない書き方になりました。

```jsx:StyledJSXButton.tsx
export const StyledJSXButton: FC<Props> = ({
  children,
  className,
  size,
  variant,
  onClick,
}) => {
  return (
    <>
      <button
        onClick={onClick}
        className={`button ${variant} ${size} ${className}`}
      >
        {children}
      </button>
      <style jsx>{buttonStyle}</style>
    </>
  )
}

const buttonStyle = css`
  .button {
    border-color: transparent;
    border-radius: 2px;
    border-style: solid;
    border-width: 1px;
    font-weight: bold;
    padding: 7px 15px;
  }

  .fill {
    background-color: var(--color-primary);
    color: var(--color-light-text-high);
    &:hover {
      background-color: var(--color-primary-dark);
    }
  }

  .border {
    border-color: var(--color-primary);
    color: var(--color-dark-text-medium);
    &:hover {
      background-color: var(--color-background);
      color: var(--color-dark-text-high);
    }
  }

  .m {
    font-size: var(--font-size-body1);
  }

  .s {
    font-size: var(--font-size-body3);
  }
`
```

本家のドキュメントにあるDynamic stylesも紹介します。

```jsx:公式にあるコード
const Button = (props) => (
  <button>
     { props.children }
     <style jsx>{`
        button {
          padding: ${ 'large' in props ? '50' : '20' }px;
          background: ${props.theme.background};
          color: #999;
          display: inline-block;
          font-size: 1em;
        }
     `}</style>
  </button>
)
```

今回の例では塗りのボタンと線のボタンを切り替えるのが`props ? foo : bar`だとやたら記述量が多くなったので却って原始的な作りを実施しました。

## まとめ

- SCSSの導入とpropsにあわせてスタイルを変更するのはどちらもそんなに変わらない
- 親からスタイルを受け渡すのはstyled-jsxだと割と面倒
    - また`css.resolve`で定義されているのが`className, styles`なのが名前被りして悩むタイミングがある
- 個人的にはこの2つならCSS Modulesを選ぶ