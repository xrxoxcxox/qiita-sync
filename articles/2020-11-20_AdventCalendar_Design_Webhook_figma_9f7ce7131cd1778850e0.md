<!--
title:   Figma Webhooksを使ってLibrary Publishを検知してDesign Opsを捗らせる方法
tags:    AdventCalendar,Design,Webhook,figma,デザイン
id:      9f7ce7131cd1778850e0
private: false
-->
[Increments × cyma (Ateam Inc.) Advent Calendar 2020](https://qiita.com/advent-calendar/2020/increments-cyma)の5日目はIncrements株式会社の綿貫が担当します！

## この記事で分かること

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c699a841-44ab-86c7-f694-5e05215d5693.jpeg)

1. [Figma Webhooks](https://www.figma.com/developers/api#webhooks_v2)と[Zapier](https://zapier.com/)を組み合わせて
1. FigmaのLibrary Publishを検知して
1. Slackに通知を送る方法

## Library Publishを検知したくなった経緯

FigmaにはTeam Libraryという機能があり、ファイルをまたいでコンポーネントやスタイルを共有できます。
また、一度共有した後にLibraryのデータを変更すると、LibraryをimportしているUIデータは全て変更されます。

これにより防げるのは**ページAは最新コンポーネントが使われているのにページBは古いまま**といった事態。
新しいコンポーネントがちゃんと各ページに反映されているか？の確認作業がなくなります。

とても便利なTeam Libraryですが、**意図せぬ変更を加えてしまった場合**であっても**全てのファイルに反映されてしまう**のがちょっとした困りごと。

正確にはLibraryを変更した後にPublishという工程を挟み、その後Libraryをimportしているファイルからも明示的にアップデートしないと反映はされません。
そのため全く意図しないアップデートを全ファイルに適用してしまう危険は少ないのですが、**変更した覚えのないコンポーネントのアップデート通知が来ている**場面がときどきあります。

もし**意図せぬアップデート**を反映してしまい、想定外のデータの崩れを引き起こしたら……少し怖くなりますよね。

というわけで、「**いつ**」「**誰が**」「**どのデータを**」アップデートしたのかをリアルタイムに検知したい、というモチベーションで作った仕組みを紹介します。

## Zapierの準備

前置きが長くなりましたが、まずはZapierで以下のように進めていきます。

1. Create Zap
1. Trigger
    1. App Event → Webhooks by Zapier
    1. Trigger Event → Catch Hook
    1. `Continue`を押して次へ
1. Set up trigger
    1. Custom Webhook URLをコピー
    1. `Continue`を押して次へ
1. Test trigger
    1. まだテスト出来ないので、一旦この画面のまま放置

この後続きの入力をしていくので、画面は開いたままにしておいてOK。

|Create Zap|App Event|Trigger Event|
|---|---|---|
|![Create Zapの動線](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/8ce6e41b-2e17-28ab-e0d5-7472deee3cc4.jpeg)|![App Event選択時](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/3d9f41e2-2e1b-9879-e5b9-b9268fd9ac14.jpeg)|![Trigger Event選択時](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e0100726-8b71-7375-adba-d3bff991c692.jpeg)|

## Figmaでwebhook作成

次に[Figma APIのWebhooksのセクション](https://www.figma.com/developers/api#webhooks-v2-post-endpoint)にアクセスして、右側にある`Try it out for yourself`に以下の情報を入れます。

|カテゴリー|入力する内容|
|:--|:--|
|event_type|LIBRARY_PUBLISH|
|team_id|あなたの属するTeamのID※1|
|endpoint|先ほどコピーした`Custom Webhook URL`|
|passcode|適当な文字列※2|
|status|一旦入れなくてもOK|
|description|一旦入れなくてOK|

※1 ブラウザ版Figmaで自分のチームのページにアクセスするとURLが以下のような書式になっており、`XXXXXXXXXX`の部分がIDです。
`https://www.figma.com/files/team/XXXXXXXXXX/your_team_name`
※2 エンドポイントに渡されて、Figmaから呼び出されていることを確認するための文字列……とのことでしたが、イマイチどう活かすかが把握できていません……。


また、上記の情報の下に`+ Get personal access token`というリンクがあるのでクリック。（Figmaにログインしていれば勝手にaccess tokenを入れてくれます）

そして`Submit API Request`を押下すると新規Webhookが作成され、生成されたwebhookの内容が表示されることでしょう。

webhookの作成自体はこれで終わりですが、スムーズに進めるためにこのタイミングでFigmaに適当なLibraryを作ってPublishします。
中身はなんでもOKで、四角形1つ置いてコンポーネント化してPublish、程度で問題ありません。

## ZapierとSlackの連携

再びZapierを開き、先ほどの画面の続きから進めます。

`Test trigger`を押下すると表示されるのが、前のステップで作成したFigma webhookから取得した情報。
ここにある`created_components`や`deleted_components`、`description`などがSlackに通知できる情報です。

|Test trigger|
|---|
|![Test triggerの成功時](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c41bc219-e3ce-4614-0e94-7adbd712540a.jpeg)|

表示を確認できたら以下のように進めていきます。

1. Action
   1. App Event → Slack
   1. Action Event → Send Channel Message
   1. `Continue`を押して次へ
1. Choose account
   1. 自分のSlackアカウントを選択
   1. `Continue`を押して次へ
1. Set up action
   1. Channnel → Figmaの通知を流したいチャンネルを選択
   1. Message Text → 任意の書式で記述※3
   1. それ以外の項目 → 一旦全て空欄でも問題ありません

ここまで出来たら後はこのZapをONにするだけで、Library PublishがあるたびにSlackに通知が来ます。

※3 参考までに、自分はMessage Textは以下の書式です。

```
Libraryが更新されました
 *File:*
{{file_name}} https://www.figma.com/file/{{file_key}}

*Description:*
{{description}}

*Created:*
{{created_components}}

*Modified:*
{{modified_components}}

*Deleted:*
{{deleted_components}}

*User:*
{{triggered_by__handle}}
```

## まとめ

人間とはミスをする生き物。しかしこういった仕組みを導入すれば、意図せぬアップデートが起きてもすぐに気づき、対処が出来るはず。

今回はLibrary Publishを取り上げましたが、**それ以外にもwebhookで出来ることはいくつか存在しています。** 色々な活用法がありそうで楽しくなっちゃいますね。

Figmaにはプルリクエストのような仕組みは無く、**レビューを通ったものだけが本番データ**という仕組みには出来ませんが、こうやってユーザー側で拡張しやすい世界を作ってくれているのは素晴らしいと思います。

ただ、ここまで良いことを色々書いてきましたが少し難点も……

- Figma Webhooksがまだbeta版、今後仕様変更があるかもしれない
- このZapを作るにはZapierの有料アカウントが必要
  - Webhooks by Zapierは無料プランでは試すことも出来ず、月払いを選ぶと$24.99かかる

2つ目の値段の話は、Zapierのヘビーユーザーならいざ知らずこの機能のためだけと考えると少し高く思えます。
簡単に設定出来て良い体験だったのでお金を払うのは吝かではありませんが、費用対効果が良いとは言えないでしょう。
ただ、万人にはオススメ出来ないものの、自動化・効率化が好きな人に「面白そう」と思ってもらえたら嬉しいです！

そして今回は手軽さを求めてZapierを選びましたが、AWS Lambdaなどを使えばもっと自由な設定も出来るのかもしれません。
もっと良いやり方があるよ、という方がいらしたら是非コメントください！

## 最後に

[Increments × cyma (Ateam Inc.) Advent Calendar 2020](https://qiita.com/advent-calendar/2020/increments-cyma)の6日目はIncrements株式会社の @okoshi がお送りします！