<!--
title:   Storybookでちょっとしたスタイルを当てたいときはDecoratorsを使う
tags:    React,storybook,tips,新人プログラマ応援
id:      c2dad5f8981124845d72
private: false
-->
## この記事の概要

Storybookを使っている中で「余白を広げたい」「このコンポーネントのときだけ背景色が欲しい」といった場面はあると思います。
今回はReact + Storybookで上記を実現する書き方を記事にしました。

また、新人プログラマ応援イベントに投稿する記事でもあります。

https://qiita.com/official-events/3f21c92121aa125807b4

## Decoratorsとは

公式ドキュメントはこちらです。（左のナビゲーションから、React以外での書き方にも飛べます）

https://storybook.js.org/docs/react/writing-stories/decorators

マークアップを追加したり、`ThemeProvider`などを通じてスタイルを与えたりといった使い方ができます。

:::note warn
理屈の上では複雑なレイアウトやスタイルも当てられますが、まず間違い無くややこしくなります。
あくまで「コンポーネントカタログとして少しだけ見やすくする」ような使い方が良いと思います。
:::

## 使い方

### 個別のコンポーネントにスタイルを当てる

通常のStoryは以下のようなコードです。

```javascript:Button.stories.jsx
    import React from 'react';
    import { Button } from './Button';

    export default {
      title: 'Button',
      component: Button,
    };

    const Template = (args) => <Button {...args} />;

    export const Primary = Template.bind({});
```

Decoratorsを追加すると、Storyをwrapしてスタイルを当てられます。

```diff_javascript:Button.stories.jsx（Decorators追加後）
    import React from 'react';
    import { Button } from './Button';

    export default {
      title: 'Button',
      component: Button,
    };

    const Template = (args) => <Button {...args} />;

    export const Primary = Template.bind({});
+   Primary.decorators = [
+     (Story) => (
+       <div style={{ margin: '3em' }}>
+         <Story />
+       </div>
+     ),
+   ];
```

### Storybook全体にスタイルを当てる

個別の`stories`ではなく、`.storybook/preview.js`を変更します。

[インストール用のドキュメント](https://storybook.js.org/docs/react/get-started/install)に沿って`npx sb init`を実行していたとすると、`preview.js`は以下のようになっていると思います。

```javascript:.storybook/preview.js
  export const parameters = {
    actions: { argTypesRegex: '^on[A-Z].*' },
    controls: {
      matchers: {
        color: /(background|color)$/i,
        date: /Date$/,
      },
    },
  }
```

先ほどとほぼ同じ書き方ですが、このように変わります。

```diff_javascript:.storybook/preview.js
+ import React from 'react';

+ export const decorators = [
+   (Story) => (
+     <div style={{ margin: '3em' }}>
+       <Story />
+     </div>
+   ),
+ ];

  export const parameters = {
    actions: { argTypesRegex: '^on[A-Z].*' },
    controls: {
      matchers: {
        color: /(background|color)$/i,
        date: /Date$/,
      },
    },
  }
```

Storybook内に`reset.css`的な役割のスタイルを定義するのにも使えるでしょう。

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox