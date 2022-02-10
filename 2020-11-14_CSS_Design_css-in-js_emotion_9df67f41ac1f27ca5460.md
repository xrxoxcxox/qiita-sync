<!--
title:   [LTスライド&原稿]CSS in JSで変わること変わらないこと
tags:    CSS,Design,css-in-js,emotion,デザイン
id:      9df67f41ac1f27ca5460
private: false
-->
この記事は2020年11月14日に[WCAN 2020 Frontend Special](https://wcan.jp/event/wcan2020-frontend.html)というイベントで行うLTのスライドと原稿です。

投稿時点ではまだイベントは始まっていませんが、先んじて公開します。
以下が実際の内容です。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/11c6146b-1f22-6657-c0da-414c1407b262.jpeg)

それでは「CSS in JSで変わること変わらないこと」と題して私のLTを始めます。

なお今回のスライドと台本は既にアップしてあります。今日は相当早口で進めてしまいますが、後でまた読んでいただけたら非常に嬉しいです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f00030c5-1143-3c14-20ce-3dbf4eddb3e4.jpeg)

みなさんこんにちは。エイチームグループのIncrements株式会社でデザイナーをしている綿貫佳祐と言います。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bdd879ee-38b4-2fb7-826e-1518d43f9e30.jpeg)

少しだけご紹介しますと、Incrementsではこれらのサービスを展開しています。

- プログラミングに特化した情報共有コミュニティのQiita
- 社内向け情報共有のためのQiita Team
- エンジニアの採用を支援するQiita Jobs

私はデザイナーでありますが、これらのプロダクトのフロントエンドまで、ある程度担当しています。

個人開発でもCSS in JSを使用しており、普段から使用する中で見えてきたCSSとの変わること・変わらないことについてお話します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f5553663-c426-35d2-83a4-726d0dfb0641.jpeg)

まずは変わることです。色々ありますが、絞ってこの3つ。

- スタイルの競合と命名規則
- ロジックによるスタイル分岐のしやすさ
- ライブラリを理解する必要性

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/473030a9-95e4-736b-df31-2aaa2ac3ad09.jpeg)

はじめのスタイルの競合については、CSS in JSが分かりやすく魅力的に映るポイントではないでしょうか？

![06.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/dc804b43-6c2f-b81b-db10-3b243ca163b7.jpeg)

例はEmotionでのコードです。コンポーネントAとBでそれぞれ`someClass`という命名でスタイルを指定しても、ハッシュがついたクラス名に変換されて競合しなくなります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/aa24b896-8386-c973-2992-f42aacce73d4.jpeg)

コンポーネントのコード量とかにもよりますが、クラス名をほとんど考えなくても良くなる場合も結構多いんですね。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ae20577e-70e7-9271-eb03-63f57e885893.jpeg)

今まで人間がBEMなどの規則を用いて解消していたクラス名の衝突は、最早意識しなくて良い存在へと変化します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f9e45dd3-9526-294a-2981-833faf46ac30.jpeg)

次にロジックによるスタイルの分岐ですが、CSSとJSが同じ世界にあることで格段にやりやすくなりました。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/98996cc8-b6fc-7eef-d4f0-ba6cdd8e2742.jpeg)

どういうことか？これまでのCSSだと「ほとんど一緒なんだけどわずかに違う」スタイルを実装する際はそのパターン数と同じだけクラスを作り、modifierとして定義する必要がありました。

この例でいうとerrorとかsuccessのmodifierを定義しています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/436f8340-508c-b5c8-3bb8-ac4072d5ca4b.jpeg)

しかしCSS in JSの場合はpropsがあります。なんでもかんでも制限なく渡せてしまうのは良くないですが、上手に使うと非常に柔軟です。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/8e2fa0a7-e5ce-7264-c764-7780528b4c96.jpeg)

自分のポートフォリオではFigmaの活動をGItHubの芝っぽく表示していますが、これは1つのコンポーネントにpropsを渡すだけで青色の濃淡の調整が出来ています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e6ad746b-0a80-493a-711c-39c40bc6c2d9.jpeg)

事前に網羅的に宣言しないといけなかったのが、変化した内容を受けて対応出来るように変わっています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/739a4866-f7d6-e9d9-fcca-35223811eae1.jpeg)

そして変わることの最後。ライブラリを理解する必要性です。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d82b2d63-0a7e-8af2-0031-87e7e5fe2e55.jpeg)

色々ライブラリがある中で自分はEmotionが好きなのですが、このような書き方が出来ずハマったことがあります。

あるcss propを他のcss propの中でだけオーバーライドするのが出来ません。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/97030c69-be29-915c-be9d-5cd8a3b23dc3.jpeg)

ハックした書き方をすれば一応解消出来るのですが、とにかく生のCSSとは違いライブラリの機能に依存して書き方を考えないといけない場面も出てくる、というお話です。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/fd03ab63-26a2-1916-303c-affbd345d555.jpeg)

次は変わらないこと。こちらも3つに絞りますと

- CSS設計の必要性
- タイプセレクターの扱い
- マークアップとの関わり方

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/48c68bf5-d27d-c0f4-5db3-e520e2329870.jpeg)

まずCSS設計の必要性です。ときどき「CSS in JSになればCSS設計は要らないよね？」と聞かれるんですが、私は必要だと思っています。

と言っても今までの設計内容と同じではありません。自分がよく考えるのは調整系のスタイルの扱い方についてです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0b1c1cb5-e488-22ff-b97e-3ed6867913f7.jpeg)

例えばFLOCSSでいう`Utility`やITCSSでいう`Trumps`のスタイル。どこかでまとめて宣言して各コンポーネントで呼ぶのはアリとするのか？

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/497d6bf7-7a2a-e35b-afcc-9a33ce1f20ab.jpeg)

「毎回同じ記述するのはダルい」と「ユーティリティーを用いたらせっかくコンポーネントに閉じ込めた意味がなくなる」はどちらの主張も分かるので、線引きは難しいです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d41dd569-f25f-1714-655d-2845e64c1e8d.jpeg)

厳密な命名規則を考える……とかからは開放されますが、CSS設計という概念自体は変わらず必要だと思っています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/2c8cf426-102d-8dbf-a39f-01e785c3a703.jpeg)

次にタイプセレクターの話。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7dea8d2e-0afb-762d-e9a0-110c5d305fe3.jpeg)

画像は1番最初に例に出したものと同じです。クラス名にハッシュがついてユニークになったとは言え、結局はグローバルに宣言された存在でしかありません。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e8f0fb14-d6aa-4d75-5ba0-e587b3959524.jpeg)

CSS in JSでもこのようにネストしてタイプセレクターを使えば、あるコンポーネントの内側にある`div`要素は全部`margin-top`がついてしまい、`p`要素は赤くなってしまいます。このあとのスタイリングで不要な打ち消しを余儀なくされるでしょう。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/73a8ef0c-3af3-9aaf-61f2-185e4f92b4f1.jpeg)

Shadow DOMが良い感じに扱えるようになるとこれらも解消出来るはずなんですが……まだしばらくはタイプセレクターでスタイルを宣言するのはやめた方が良さそうです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a53631ed-3037-db3d-6bef-2615d6688fc2.jpeg)

最後にマークアップとの関わり方。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/55f13ed0-e693-c674-4e15-c554a36b30ff.jpeg)

これは一言で言えば「CSS in JSを導入したからってマークアップが適当で良くなることは無い」に集約されます。

spanを使えば良い場所を全部divで書いて`display: inline;`を書くのは手間ですし、変な階層構造のマークアップをすると`position`とか`z-index`で苦しめられがちです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/80a8c9d2-6b72-af36-be76-5abeb530e0d6.jpeg)

マークアップはマークアップでちゃんと書けた上で、CSS in JSはプラスアルファ便利になるツールでしかありません。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/6f76c9a9-3977-12bc-3ac3-9b936f9cce89.jpeg)

かなり詰め込んでしまいましたが、変わること変わらないことそれぞれ3つご紹介しました。

変わることは

- スタイルの競合と命名規則
- ロジックによるスタイル分岐のしやすさ
- ライブラリを理解する必要性

変わらないことは

- CSS設計の必要性
- タイプセレクターの扱い
- マークアップとの関わり方

自分のLTは以上です。ご清聴ありがとうございました。