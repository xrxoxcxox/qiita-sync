<!--
title:   Figmaのコメント一覧をさっと取得したいなあ……
tags:    Design,figma,デザイン
id:      d7d0e0f19fabbd0463d5
private: false
-->
## これは何

Qiitaのデザイナーをしています。綿貫といいます。
中の人としてもアドベントカレンダーを盛り上げるべくFigma Advent Calendarを作成し、3日目の記事を書きました！

https://qiita.com/advent-calendar/2021/figma

今回はFigmaのコメント機能についてとりあげます。
なお、ほぼネタみたいな記事なので実用性はそんなにありません。

## コメントがたくさんつくと確認が大変

Figmaのコメント機能はコミュニケーションをとる上で非常に便利ですが、あまりにもたくさんのコメントがつくと追うのが大変です。
特に複数のPageにまたがっているとあっちを開いてこっちを開いて……。

単にコメントを一覧したいだけなのになあってときは若干不便です。

## ターミナルで全件取得してみましょう

https://www.figma.com/developers/api#comments

Figma APIを見るとcommentsのエンドポイントがありますので、利用しましょう。

### access tokenの取得

`Settings > Account > Personal access tokens`で取得できます。
適当な名前をテキストフィールドに入れてエンターを押せば発行されるのでどこかにメモしておいてください。

### file keyの取得

コメントを取得したいFileを開いて`Share > Copy link`。
ブラウザで開くとURLの形式が`https://www.figma.com/file/:file_key/:title?node-id=XXXX`といった具合になっているので`file_key`をメモしておいてください。

### コマンドを叩く

後は簡単です

```shell:ターミナル
curl -H 'X-FIGMA-TOKEN: YOUR_TOKEN' 'https://api.figma.com/v1/files/YOUR_FILE_KEY/comments' | python3 -c 'import sys,json;print(json.dumps(json.loads(sys.stdin.read()),indent=4,ensure_ascii=False))' | grep "message"
```

結果はこんな感じで見れます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/3add5168-a7b3-8e81-97d7-001a165d5381.png)

なお、詳細に見たい場合は`| grep "message"`を削るだけでOKです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/926e9791-9e59-163e-91c8-dad856b20227.png)

整形のコードはこちらのブログを参考にさせていただきました :pray:

https://kotapontan.hatenablog.com/entry/2021/01/04/curl%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%81%AE%E7%B5%90%E6%9E%9C%E3%82%92%E6%95%B4%E5%BD%A2%E3%81%99%E3%82%8B

## まとめ

- Figma APIでコメントを全件取得できる
- ただただ全てのコメントを見たい場面があれば使えるテクニック
- ネタです