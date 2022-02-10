<!--
title:   divやspanでボタンっぽい要素を作ると大変なことになるからbuttonを使おう
tags:    HTML,JavaScript,accessibility,アクセシビリティ
id:      7d06d4f2d861b022767a
private: false
-->
[Ateam Brides Inc. Advent Calendar 2021](https://qiita.com/advent-calendar/2021/ateam-brides)の10日目はデザイナーの綿貫（@xrxoxcxox）がお送りします！

## この記事の概要

ボタンっぽい見た目の要素をクリックしたら検索フォームが動くとか自分のプロフィールを更新するとか、そういうインターフェースはたくさんありますよね。

ときどき`div`や`span`要素に`addEventListener('click', function)`をつけて動かしているのを見かけるのですが、結局改善を求められるはずでしかも工数がかかります。

というわけでシンプルに`button`を使いましょう、と啓蒙する記事です。

## 伝えたいこと

ボタンっぽい役割の要素であれば`button`タグを使いましょう！

この1点のみです！

## `div`や`span`でボタンっぽい要素を作るとどうなるの？

- タブキーでフォーカスを当てられない
- `enter`や`space`でボタンのアクションを実行できない
- スクリーンリーダーが適切に読み上げてくれない

というわけで非常に使いづらくなります。
あなたの作っているプロダクトを使ってくれている人から「ボタンが操作できないので修正してもらえませんか？」と意見をいただければラッキーで、上手く使えないからと静かに去ってしまう可能性もあるかもしれません。

特にキーボードのみで操作している人にとってはクリティカルな問題になり得ます。

## `div`や`span`でボタンっぽい要素を作った場合、改善する方法

まずは最初の時点のコードを載せます

```html:index.html
<div id="div" class="button">ボタン風のdiv</div>
```

`javascript:script.js
const div = document.getElementById('div');

const divClick = () => {
  alert('divがクリックされました');
};

div.addEventListener('click', divClick);
`

| クリックだけならbuttonとdivの差は無く見える |
| --- |
| ![default.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/06ab66eb-4ecb-24a8-2d0a-f180c0550f71.gif) |

### `tabindex`をつける

```diff_html:index.html
- <div id="div" class="button">ボタン風のdiv</div>
+ <div id="div" class="button" tabindex="0">ボタン風のdiv</div> 
```

`tabindex`をつけないと、そもそもタブキーでフォーカスを当てることができません。
タブキーでリズミカルに進んでいたのに、アクションを起こすためのボタンでつんのめってしまいます。

| tabindexが無いのでフォーカスがあたらない | tabindexがあるのでフォーカスがあたる |
| --- | --- |
| ![focus-before.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/43533ff7-ad94-3c78-1657-4775d9f8bc22.gif) | ![focus-after.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4e37b74a-8ade-860a-c8d0-2d0e1cbd18a2.gif) |

### `keydown`のeventListenerを登録する

`keydown`がないと、せっかくフォーカスがあたってもキーボードからアクションを起こせません。
`enter`と`space`を登録して、クリックとは別な処理として書く必要が出てきます。

```diff_javascript:script.js
  const div = document.getElementById('div');

  const divClick = () => {
    alert('divがクリックされました');
  };

+ const divKeydown = (e) => {
+   if (e.keyCode === 13 || e.keyCode === 32) {
+     alert('divに対してキーが押されました');
+   }
+ };

  div.addEventListener('click', divClick);
+ div.addEventListener('keydown', divKeydown);
```

| keydownが無いのでenterを押しても反応がない | keydownがあるのでenterを押すと反応する |
| --- | --- |
| ![keydown-before.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/8b141230-43f2-8c44-400c-cc583b37b755.gif) | ![keydown-after.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/52508c15-8668-0900-e5b9-3644ae727a21.gif) |

### `button`のroleを追加する

```diff_html:index.html
- <div id="div" class="button" tabindex="0">ボタン風のdiv</div> 
+ <div id="div" class="button" tabindex="0" role="button">ボタン風のdiv</div>  
```

操作するタイミングでは問題がなくなりましたが、まだマークアップとしては`div`でしかないのでスクリーンリーダーが上手く認識してくれません。
しっかり`button`であると明示する必要があります。

※少し見づらいですが、右下に出してあるVoiceOverに表示されている内容が変わっています。

| roleが無いのでdivと認識されたまま | roleがあるのbuttonと捉えられる |
| --- | --- |
| ![role-before.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ef662431-83da-7a11-b5a4-2b0cdd4eb330.gif) | ![role-after.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/565ccd24-4ffe-2a3c-02c8-f4d93e0db059.gif) |

## 上記の内容は`button`タグなら全く実施しなくてOK

たとえば、何かの処理を作る度にキー押下の判定を加えるのは面倒ですよね？（仮にユーティリティな関数として切り出したとしても、毎回呼び出すこと自体が面倒だと思います）

けどこれらは正しいマークアップ、つまり`button`タグを使ってさえおけば何も実施しなくてOKなんです。

使わない手はありませんね。

## まとめ

- ボタンっぽい要素を`div`や`span`で作ってしまうと
    - タブキーでフォーカスを当てられない
    - `enter`や`space`でボタンのアクションを実行できない
    - スクリーンリーダーが適切に読み上げてくれない
- `button`で作れば上記の問題は起きない