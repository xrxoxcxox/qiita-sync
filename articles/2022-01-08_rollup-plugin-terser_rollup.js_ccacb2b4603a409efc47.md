<!--
title:   rollup-plugin-terserの有無を比べてみる
tags:    rollup-plugin-terser,rollup.js
id:      ccacb2b4603a409efc47
private: false
-->
## この記事の概要

Rollupの調べごとをしていたら「rollup-plugin-terserを使うとminifyできる」と見つけて試しに使ってみました。
せっかくなのでどんな風に圧縮されるのかを記事にしてみます。

## 圧縮する対象

Storybookのイニシャライズをしたときに生成されるコンポーネントをEmotionで書き換えただけのコンポーネントです。

```typescript:Button.tsx
import { css } from '@emotion/react'

export interface ButtonProps {
  primary?: boolean
  backgroundColor?: string
  size?: 'small' | 'medium' | 'large'
  label: string
  className?: string
  onClick?: () => void
}

export const Button = ({
  primary = false,
  size = 'medium',
  backgroundColor,
  label,
  className,
  ...props
}: ButtonProps): JSX.Element => {
  const mode = primary ? styles.primary : styles.secondary
  return (
    <button
      type='button'
      css={[styles.button, styles[size], mode, className]}
      style={{ backgroundColor }}
      {...props}
    >
      {label}
    </button>
  )
}

const styles = {
  button: css`
    border: 0;
    border-radius: 3em;
    cursor: pointer;
    display: inline-block;
    font-family: 'Nunito Sans', 'Helvetica Neue', Helvetica, Arial, sans-serif;
    font-weight: 700;
    line-height: 1;
  `,
  primary: css`
    background-color: #1ea7fd;
    color: white;
  `,
  secondary: css`
    background-color: transparent;
    box-shadow: rgb(0 0 0 / 15%) 0 0 0 1px inset;
    color: #333;
  `,
  small: css`
    font-size: 12px;
    padding: 10px 16px;
  `,
  medium: css`
    font-size: 14px;
    padding: 11px 20px;
  `,
  large: css`
    font-size: 16px;
    padding: 12px 24px;
  `,
}
```

## rollup.config.jsの設定

出力されるコード内容に関係ある`output`の設定だけを抜粋します

```javascript:rollup.config.js
output: {
  format: "es",
  exports: "named",
  sourcemap: true,
  preserveModules: true,
}
```

## rollup-plugin-terserなし

```javascript
import { __rest } from 'tslib';
import { jsx } from '@emotion/react/jsx-runtime';
import { css } from '@emotion/react';

const Button = (_a) => {
    var { primary = false, size = 'medium', backgroundColor, label, className } = _a, props = __rest(_a, ["primary", "size", "backgroundColor", "label", "className"]);
    const mode = primary ? styles.primary : styles.secondary;
    return (jsx("button", Object.assign({ type: 'button', css: [styles.button, styles[size], mode, className], style: { backgroundColor } }, props, { children: label }), void 0));
};
const styles = {
    button: css `
    border: 0;
    border-radius: 3em;
    cursor: pointer;
    display: inline-block;
    font-family: 'Nunito Sans', 'Helvetica Neue', Helvetica, Arial, sans-serif;
    font-weight: 700;
    line-height: 1;
  `,
    primary: css `
    background-color: #1ea7fd;
    color: white;
  `,
    secondary: css `
    background-color: transparent;
    box-shadow: rgb(0 0 0 / 15%) 0 0 0 1px inset;
    color: #333;
  `,
    small: css `
    font-size: 12px;
    padding: 10px 16px;
  `,
    medium: css `
    font-size: 14px;
    padding: 11px 20px;
  `,
    large: css `
    font-size: 16px;
    padding: 12px 24px;
  `,
};

export { Button };
//# sourceMappingURL=Button.js.map
```

## rollup-plugin-terserあり

```javascript
import{__rest as o}from"tslib";import{jsx as r}from"@emotion/react/jsx-runtime";import{css as e}from"@emotion/react";const i=e=>{var{primary:i=!1,size:a="medium",backgroundColor:n,label:s,className:l}=e,p=o(e,["primary","size","backgroundColor","label","className"]);const c=i?t.primary:t.secondary;return r("button",Object.assign({type:"button",css:[t.button,t[a],c,l],style:{backgroundColor:n}},p,{children:s}),void 0)},t={button:e`
    border: 0;
    border-radius: 3em;
    cursor: pointer;
    display: inline-block;
    font-family: 'Nunito Sans', 'Helvetica Neue', Helvetica, Arial, sans-serif;
    font-weight: 700;
    line-height: 1;
  `,primary:e`
    background-color: #1ea7fd;
    color: white;
  `,secondary:e`
    background-color: transparent;
    box-shadow: rgb(0 0 0 / 15%) 0 0 0 1px inset;
    color: #333;
  `,small:e`
    font-size: 12px;
    padding: 10px 16px;
  `,medium:e`
    font-size: 14px;
    padding: 11px 20px;
  `,large:e`
    font-size: 16px;
    padding: 12px 24px;
  `};export{i as Button};
//# sourceMappingURL=Button.js.map
```

## 両者の違い

単に改行を削るだけでなく、変数の名前を1文字に置き換える処理も走っているようです。
ただ、スタイル関連の部分は改行が残っていますね。
String記法からObject記法に置き換えた上でもう一度ビルドしてみます。

## Object記法に置き換え＆rollup-plugin-terserあり

```javascript
import{__rest as o}from"tslib";import{jsx as r}from"@emotion/react/jsx-runtime";import{css as e}from"@emotion/react";const i=e=>{var{primary:i=!1,size:a="medium",backgroundColor:n,label:l,className:s}=e,d=o(e,["primary","size","backgroundColor","label","className"]);const c=i?t.primary:t.secondary;return r("button",Object.assign({type:"button",css:[t.button,t[a],c,s],style:{backgroundColor:n}},d,{children:l}),void 0)},t={button:e({border:0,borderRadius:"3em",cursor:"pointer",display:"inline-block",fontFamily:'"Nunito Sans", "Helvetica Neue", Helvetica, Arial, sans-serif',fontWeight:700,lineHeight:1}),primary:e({backgroundColor:"#1ea7fd",color:"white"}),secondary:e({backgroundColor:"transparent",boxShadow:"rgb(0 0 0 / 15%) 0 0 0 1px inset",color:"#333"}),small:e({fontSize:12,padding:"10px 16px"}),medium:e({fontSize:14,padding:"11px 20px"}),large:e({fontSize:16,padding:"12px 24px"})};export{i as Button};
//# sourceMappingURL=Button.js.map
```

全ての改行がなくなりました。
テンプレートリテラルの中の改行はそのまま残す仕様のようです。