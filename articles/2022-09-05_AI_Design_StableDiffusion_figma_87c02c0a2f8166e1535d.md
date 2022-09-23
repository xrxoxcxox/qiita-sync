<!--
title:   Figma上でAIによる画像生成ができるプラグイン、Andoが出たので試してみた
tags:    AI,Design,StableDiffusion,figma,デザイン
id:      87c02c0a2f8166e1535d
private: false
-->
## この記事の概要

Figma上でAIが画像生成をしてくれるプラグインが出たので、導入の仕方について記事にしました。

https://ando.studio/

## Andoとは

参考画像とプロンプトを指定するだけで、Figma上で画像を自動で生成してくれます。
プロンプトだけでも生成はしてくれますが、参考画像を指定した方が希望の見た目に近づけやすいです。

Stable Diffusionを用いているようです。

https://twitter.com/RemitNotPaucity/status/1562319004563173376

## 導入

Figmaのコミュニティ上で`Ando - AI Copilot for Designers`と検索すると出てきます。[^1]

[^1]: 2022年9月5日現在、なぜかURLを共有すると404で閲覧できないのですが、直接Figmaから調べるとアクセスできます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5a99c445-12c3-fa77-f505-4e64f46b9976.png)

プラグインを起動するとこのようなウィンドウが立ち上がるので、`Sign in`しましょう。

<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/51466c08-d321-5767-a82f-5e5dcc7058df.png" alt="" width="340">

クリックすると、Andoのサイトに飛ぶので、`Get Started`をクリックしてログイン画面に移ります。

https://ando.studio/

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d319ee3c-32d4-5280-266a-8114bcc26341.png)

普通のGoogleログインです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c51fb1bc-7577-7c43-b910-121ccd600631.png)

ログインが完了するとプランを選ぶ画面に切り替わります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bd2e89d9-a5da-1053-b75e-59c23440fae0.png)

スクリーンショットに書いてある通りですが、プランの違いは以下です。
Proには`All latest features.`と記載がありますから、今後プランによる機能の違いが出てくるのかもしれません。
現時点（バージョン0.0.3)では枚数の違い以外は無さそうです。

プラン名 | Free | Pro
--- | --- | ---
料金 | $0 | $18 / 月
生成可能な画像の枚数 | 42枚 / 月 | 無制限

ログインが完了した上でFigmaに戻ると、プラグインのウィンドウが変化しています。

<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/57e81beb-1a44-b46d-1701-a9fb3204ef5d.png" alt="" width="340">

これで導入は完了です。

## 使い方

サイトトップで見本として使われている`futuristic sneaker with translucent material, studio lighting, product photography`というプロンプトを拝借します。

`Prompt`のテキストエリアに文章を入力するのみでOKです。

参考画像無しで生成したところ、このようになりました。
スクリーンショットには2つの画像を表示していますが、1度に生成されるのは1枚だけです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/fa1009ca-981b-392f-3baf-12ee8067ad1f.png)

次に、参考画像を指定します。
と言っても特にやることは無く、任意のFrameを選択すると自動で`Reference`に指定されます。
指定する対象はパスでも画像でもなんでも大丈夫です。
強いて気を付けることと言えば、deep selectして間違った対象を参考画像に指定しないこと、くらいでしょうか。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/fbd074c2-34a4-6651-6b3a-f659e4cd95dd.png)

黒い絵を指定しているのに白いスニーカーが出現したものですから、驚いて4回も試してしまいました。
テイストも割とバラバラですし、制御するのはなかなか難しいのかもしれません。
プロンプトも公式のものを拝借しているので、そこまでおかしな内容では無いはず......。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/150c506d-f127-f1d6-9832-b689706dafd1.png)

参考画像を指定した場合、`Prompt Weight`から画像とプロンプトどちらに重みをつけるか指定できます。
範囲は0から100で、デフォルトは80です。

ただ、上で記載している通り、参考画像を参考にしている感が薄く、かなりImageに寄せてもこういった出力でした。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e5c257a2-8b18-6e82-a8c2-8ca71d8f8b79.png)

## まとめ

- Googleログインだけで導入できる
- 無料で生成できる枚数が42枚/月と多い
- 狙ったテイストを生成するのは大変そうな印象
- まだバージョンが0.0.3なので、今後機能が増えていきそう？

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox