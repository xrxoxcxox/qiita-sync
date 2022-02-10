<!--
title:   [LTスライド&原稿]実装まで視野に入れるならAtomic Designを辞めましょう
tags:    AtomicDesign,Design,LT,デザイン
id:      d14fa682bf54069bfb70
private: false
-->
この記事は2020年10月3日に[WCAN 2020 Design Special 〜デザイン力の基本とブランドづくりの一歩〜](https://wcan.jp/event/wcan2020_design.html)で行うLTのスライドと原稿です。

投稿時点ではまだイベントは始まっていませんが、先んじて公開します。
以下が実際の内容です。

![Frame 1.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/307cc5d7-99a7-25ad-8984-5118f89df9be.jpeg)

それでは「実装まで視野に入れるならAtomic Designを辞めましょう」と題して私のLTを始めます。

![Frame 2.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/2805c4ce-9d10-55f8-01c9-e700d2dcab0a.jpeg)

みなさんこんにちは。Increments株式会社でデザイナーをしている綿貫佳祐と言います。

![Frame 3.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/90ceb491-2085-5f05-00e4-6259352ffc18.jpeg)

Incremetnsではこれらのサービスを展開しております。

普段の業務からこれら3つ、インターフェースだけでなくフロントエンドまである程度担当していまして、両方の視点から見た話をしていきます。

![Frame 4.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/805bf573-2fb8-0f75-515c-0358eb9a0244.jpeg)

最初にお断りしておきますと、現在Atomic Designを使っている方々を否定するつもりはありません。

![Frame 5.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ae4ca412-04c7-0a8d-fc8c-e07bdfb741da.jpeg)

ただ、主観ですがAtomic Designは「汎用的なデザイン制作の概念」「コンポーネント設計のベストプラクティス」として浸透し過ぎている気がします。

何においても、唯一絶対のものが存在しているよりいくつか流派が存在している方が健全だと思うんですよね。

![Frame 6.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/669766a7-b8e7-c5ca-c449-a8114682c70e.jpeg)

そして自分は疑問を持ったので世間に投げかけるポジションをやってみよう、とLTをしている次第です。

![Frame 7.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5259b3e6-38d9-fea7-31b9-0005ba7d3c99.jpeg)

まずは自分のこれまでの経験から、Atomic Designが向いているケースと向いていないケースについて話します。

![Frame 8.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e9b52d95-1503-2174-2506-64483b99fb82.jpeg)

向いているのはこんなケースではないでしょうか。

職種でチームが分かれているとか、モックアップ作成がゴール。

イメージとしては、Atomic Designを扱うのがデザイナーだけな場合です。

![Frame 9.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/667a9084-5d3a-d56c-2dc6-af9640c98656.jpeg)

逆に、向いていないのはこういうケース。

同じチームにデザイナーエンジニアがいる。あるいはそもそもあまり役割に境界がない。

LTタイトル通りですが、Atomic Designをコードの世界に持ち込むのが向いていない……という話です。

![Frame 10.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/6b1eaa45-4612-3771-b874-5e13ca9be4b2.jpeg)

ではなぜ向いている / いないが生まれるのか？

![Frame 11.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/65773aa5-c363-e541-b99d-576e742587f9.jpeg)

この3つを主な理由として挙げます。

![Frame 12.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0ca8000b-15aa-cec8-d4f2-57e9beb7a3ee.jpeg)

1つ目のコンポーネントの分け方に迷うという話……

![Frame 13.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/fd9d9375-cdc6-9150-3d84-82c040fb22a6.jpeg)

これはみなさんある程度経験していらっしゃいますよね？

![Frame 14.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a8b7e933-3e0b-02cf-4c0e-9c5e3c835aea.jpeg)

Atomic Designにおいて何か定量的な基準があるわけではなく、チームごとに「我々はこういう基準でコンポーネントを分ける」という慣習法を持っているに過ぎないのです。

ですから人数が多くなると意思の統一が難しくなりますし、職種によって視点がどうしても違います。

![Frame 16.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d59e2cf7-b5a4-918c-c1d0-bd3e7005718b.jpeg)

次は、Atomic Designはコード的な概念を含んでいないというそもそもの話です。

あまり知られてないのですがAtomic Designはコードとしての在り方には言及していません。

![Frame 17.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/cce6e6ba-2d97-a3e0-b4d6-653d8bb2e433.jpeg)

そのためディレクトリ構造やデータの受け渡しに適用すると不整合が起きがちですし、Design TokenやUtilityをどう持つべきかを考え始めると必然的にオレオレAtomic Designになります。

![Frame 18.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0c89f619-a68c-704c-9f54-3ff8f2e5bd78.jpeg)

最後に、Atomic DesignでいうPageの概念ってコーディングの際にはほとんど存在していないんですよね。

![Frame 19.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/7af06023-53f1-6e9a-ecce-513d4a84ff53.jpeg)

ここでいうPageとはTemplateに実データを流し込んだ結果みたいなイメージ、ニアリーイコールでレンダリング結果です。

UIモックアップは大抵レンダリング結果の設計図とか予想図みたいな役割なので、Pageの概念は役立つんですが、コードで書く内容ってAtomic DesignにおいてはTemplateに近いです。

![Frame 20.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/52f70994-f201-a9dc-6145-485aff5f3448.jpeg)

ではどうなるかというとやはりコードでは不整合が起きてPageが形骸化する。

5レイヤーしか無い概念のうち1つが形骸化を余儀なくされるんです。

![Frame 21.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/57fd0021-7a6c-879a-2973-e87535354c7c.jpeg)

ここまで色々話してきましたが、悪い点の指摘だけなら誰でもできるので、代替案として私のオススメを紹介します。

![Frame 22.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f58df655-1ff7-c5ff-48d2-1b72f1849566.jpeg)

まずはコンポーネントをサイズ感によって分類しないこと。なんだったら分類を全て取り払っても良いと思います。Material Designはそんな感じですしね。

もし分類するとしてもコンポーネントの種類ごとです。ナビゲーションとかデータ入力とか。ちなみにこれはAnt Designでの考え方です。

![Frame 23.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9a5f6238-03e3-b4f4-c053-f6bacdbabc95.jpeg)

あとはデザインモックアップの作り方として、同一Componentであっても渡されるデータの有無やStateに合わせて作るのは大事だと思います。

AtomからMolecule、MoleculeからOrganismとレイヤーで考えるのではなくてあくまでコード上での振る舞いを重視するのです。

![Frame 24.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c43ab8dc-dc64-b748-5983-732c39a78a7e.jpeg)

何故かというと、Atomを探そうとかMoleculeを探そうっていう思考よりも、ボタンを探そうっていう思考の方が自然だからです。そしてSketchやFigmaのライブラリ機能を上手く使えば粒度や階層もAtomic Designに無理やり当てはめるよりよっぽど柔軟です。

そしてモックアップにAtomic Designを適用しないなら、コードに適用するメリットもあまりありません。シンプルにシステム開発におけるベストプラクティスに沿っていけばOK。という具合です。

私のLTは以上です。ご清聴ありがとうございました。