<!--
title:   ポートフォリオサイトをNetlifyからVercelへ移行した
tags:    Netlify,Vercel
id:      ad84e4cedde92c737001
private: false
-->
## これは何

- [自分のポートフォリオサイト](https://www.keisukewatanuki.work/)のホスティングをNetlifyからVercelへ移行したので、備忘録がてらまとめた記事

## なぜ乗り換えを検討したのか

- Netlifyが遅い、みたいな記事をいくつか目にした
    - https://scrapbox.io/meganii/Netlifyの表示スピードがいまいちパフォーマンスを発揮できていない件
    - https://blog.anatoo.jp/2020-08-03
    - https://www.yuuniworks.com/blog/2020-09-14-netlify-is-slow/
- なんとなく使ってみたい気持ちが生まれた
    - チュートリアル的に使ったことはあるものの、継続的な運用をするサイトでは使ったことがないので興味があった
- ホスティングサービスを乗り換える経験をしてみたかった
    - 具体的には分からないながら、一度経験をしておくと何かに役立つかもしれないという勘
    - 私はデザイナーなのでホスティングサービス乗り換えの陣頭指揮をとることはよっぽど無い気がするものの……好奇心で動いてしまった

## Vercelへデプロイ

まずは現状のmainブランチをVercelでデプロイしてみました。
独自ドメインを設定しなければランダムなドメインが割り当てられるはずなのでひとまずは様子見。

ログイン後、`New Project`を押して進みます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f07a83b0-a5ce-174d-5937-b44142fc47b4.png)

Importするリポジトリを選択するビューに移動するので、今回は`portfolio`を選びます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c4a9848b-93f7-9877-9777-c507f4a2b3f7.png)

1人で作っているので`Create a Team`はスキップ。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ff18be79-dde9-4721-2124-a27951fbb9e3.png)

環境変数を設定している場合はここで入力します。
自分はFigmaでの活動履歴をグラフ化していて、それを動かすためにいくつかのKeyの登録をしています。

`Deploy`ボタンを押すと……

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/fdd959c8-b6b8-6e1d-fcf7-8c16ac273b42.png)

ビルドが走って……

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/da9a7c64-0d74-1a7f-6da7-7afbef504385.png)

デプロイ完了！
ここまで、ビルド時間を含めても5-6分くらいで非常にスムーズでした。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0821fea7-42bb-df37-64c1-dd4e9fda22d2.png)

## 独自ドメイン設定

`Settings`を開いて、設定したいドメインを入力します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ed5729bd-2ec8-ebf6-b3d9-f04455f8c8e0.png)

リダイレクト設定が出てくるのですが特にこだわりがなかったのでRecommendedを選びました。
`www`にリダイレクトするようです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f1e0d4eb-c83b-518c-c855-92e5f404d97c.png)

まだドメイン側で何も設定していないので案の定失敗します。
A RecordとCNAMEの両方を設定します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b677e7cb-b2cf-98af-7447-2c5856f5c9f9.png)

どこでドメインを取得したかで多少違うのでしょうが、私の場合はムームードメインです。
`ムームーDNS`のページで、先ほど表示されていた値を登録 & Netlifyで設定していたネームサーバをデフォルトのものに戻しました。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/6faf8d87-5736-2880-e67d-195ba508e339.png)

Vercelに戻って`Refresh`ボタンを押すと準備完了しました。
若干のラグはありましたがそれでも数分でした。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/dcf14451-d8ed-b060-0439-c01953e85002.png)

実際のサイトも、最初の数分だけはアクセス不可になってしまいました。
とは言えこの後すぐにアクセス可能になったので個人のポートフォリオとしては何も問題ないでしょう。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c7c86c7c-6a97-9ce6-337f-a115199bf886.png)

## お片付け

Netlifyに登録していたSiteを削除。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a8e02eef-c579-0f0f-b9db-b67f1fe143d4.png)

Gatsbyを使っているサイトなので、Netlifyのプラグインを削除

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/29639aed-2771-0cc5-4d0b-f8820343177b.png)

## パフォーマンス測定

| | Desktop | Mobile |
| --- | --- | --- |
| Netlify | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/543cc645-f34d-0adb-e297-1ba38e90830e.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5b5c2df8-0104-54a8-508a-c3b6bc943351.png) |
| Vercel | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/55236321-f3f4-637b-dce4-582d3f1e37d7.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/260dadb2-dc9b-4e8f-4bc8-0ab7707d6a0a.png) |

- デスクトップはスコア上昇、モバイルは下降
    - デスクトップ：89→99
    - モバイル：73→67
    - え、どういうこと？
        - 有識者の方がいらしたら是非教えてください……
- 体感だと少し速くなったくらい？
    - 明らかな変化までは感じ取れませんでした

## まとめ

- 乗り換え作業自体は調べごとも含めて30分程度で終了
- 若干パフォーマンスは上がったものの過度な期待をするものではない
- 新しく生まれた疑問として、事業運営で乗り換えがあったらダウンさせずに上手くやれるのだろうか？
    - 今回は個人サイトなので一度完全に落として移行作業を進めていたものの、利益を上げているプロダクトの場合こういうのはどうするんだろう