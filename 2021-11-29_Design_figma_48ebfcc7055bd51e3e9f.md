<!--
title:   徹底解剖！FigmaのBranching機能
tags:    Design,figma,デザイン
id:      48ebfcc7055bd51e3e9f
private: false
-->
## これは何

Qiitaのデザイナーをしています。綿貫といいます。
中の人としてもアドベントカレンダーを盛り上げるべくFigma Advent Calendarを作成し、初日の記事を書きました！

https://qiita.com/advent-calendar/2021/figma

今回取り上げるのはOrganizationプラン限定であるBranching機能。
「すぐにプラン変更に踏み切れない、けど機能が気になる……」という方は結構いるのではないでしょうか？

この記事では基本的な使い方と、具体的なユースケースへの検証結果をまとめます。

## Branching機能とは？

Gitを使った開発でのブランチとほぼ同じ概念です。
メインのファイルからブランチを切り、データを変更し、レビューとともにメインのファイルへマージする……というフローがFigmaでも実現可能に。

全体的なイメージを得るには文章を読むより公式のYouTube動画を見る方が早いと思われます笑
（5分弱の動画なのでパパッと見れます）

https://www.youtube.com/watch?v=tbNCGEC2G1E

## 基本的な使い方

### 前提

- ある程度イメージしやすいよう、実際に業務で使っているQiitaのFigmaデータを用いて説明します
- 架空のサイトを作っても良いのですが、情報量が少なくなるなど「業務で使えるレベルではない記事」になってしまうのが嫌だったため、リアルなデータを用います
- ただし、記事内の変更はあくまで説明のために作っているものなので実際のQiitaに反映する予定はありません

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9c96825d-e2e6-7217-86bc-408ba8b43055.png)

### Branchの作成

ヘッダーにあるファイル名横の下向きキャレットをクリック。
メニューが開くので`Create branch...`を選択します。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/38a39f5e-af8a-dd88-5cad-43a9e3da72e1.png">

名前を入力するためのモーダルウィンドウが出現。
ここでは`first-branch`として`Create`を押して次へ進みます。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5c4cdae4-8c6b-d633-5e78-6a4a2b88c9cd.png">

今作ったブランチが新しいタブとして開かれます。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/460ddd0c-d5ee-48ea-1acb-dd735bf54c3e.png">

普段ファイル名が表示されている領域には元ファイルの名前と今のブランチの名前がセットで表示されるように。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c4b5ce88-df8d-8fbb-6cfb-b007755f39df.png">

また、元ファイルに戻ってみるとファイル名の横に`1 branch`の文字が。
これは`このファイルから切られたブランチの数`を表しています。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5ad72f40-7f2b-1327-940c-5375705f5a2a.png">

### Branchの編集とマージ

変化が分かりやすいように、ヘッダーの色を緑から白に変えてみましょう。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/394dc168-5b4c-3dff-1fae-b6b98ceeb4e2.png)

ブランチ名横の下向きキャレットをクリック。
`Review and merge changes...`を選びます。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/952f913c-006a-f64b-1520-66060e2e9ad7.png">

マージのためのウィンドウが出現。
`Reviewers`の横にあるプラスボタンを押すとレビュワーを選べます。

自分のチームのEditorをサジェストしてくれるので、大抵はそこから選ぶだけで事足りるでしょう。
今回はテスト用に作ったチームなので、協力を依頼した[出口くん](https://qiita.com/degudegu2510)だけが表示されていますが、本来であれば複数名表示されます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/cd48e64c-35e1-390d-6412-155ce85ee81b.png)

レビュワーとして追加すると`Requset review`がアクティブになるので、クリック。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5e94fd5f-e7af-98aa-4dbf-d6f89e17bcd6.png)

レビュー依頼があると、Slackやメールで通知が来ます。
（スクリーンショットを撮る都合で先ほどとはレビュワーとレビューイが入れ替わっています、ご了承ください :bow: ）

通知が来たらファイルにアクセスしてレビューをしましょう。

| Slack | メール |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/19c8fff5-c11d-3c92-7a47-f10111b6805f.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9edeb532-3cc3-4315-7baa-084b66761b29.png) |

データに何か問題があれば通常のようにコメント機能を使ってやりとり。
問題が無ければブランチ名横の下向きキャレットからメニューを開いて`Approve`。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/523f1137-af8b-9d09-031e-2461e111f59d.png)

ちなみにレビュワー、レビューイどちらでも`Merge branch`を実行できます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4ffa7de2-033a-024c-6a4b-e956967422bc.png)

`Merge branch`を押すとMainファイルにリダイレクト（？）された後、画面下部に`Branch merged`のメッセージとともにマージされたデータが表示されます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/940693d5-0314-1410-eeb8-87982ab8f2c6.png)

以上が基本的な使い方でした。
難しい操作もなく、かなり分かりやすいのではないでしょうか？

## 疑問が出そうな点

### メインファイルとブランチがコンフリクトしたらどうなる？

フィードのナビゲーションの色をそれぞれのファイルで変えてみました。

| Main | Branch |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4983d891-6b84-70c2-549c-0bf92886f7ce.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4586ef5e-b07b-22fa-2931-7268d601d966.png) |

この状態でBranch側からマージ操作をしてみると、`Branch review`ウィンドウの上部に`コンフリクトしているから解消してください`という旨のメッセージ。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/390b0c29-824e-5dea-090f-4fad0aa56ffb.png)

`Resolve conflicts`を押すとこのような画面が出ます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/828733d9-4235-49b7-7e94-1327470071ed.png)

……画面サイズが大きいのもあり、ちょっと見づらいですね。
コンフリクト解消のためのインターフェースでは、今のところ単にFrameを並べるだけのようです。

ビジュアルリグレッションテストのツールのように色をつけたりはしてくれると嬉しいですが、今後に期待でしょうか。

そして`Keep`するデータを選んで`Confirm picks`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0bad735c-f78f-fd66-7eb9-e3b9faa04f50.png)

とは言え、ボタンを押しても今はこれでおしまい。
このあと無事にマージができるようになるだけで、まだマージはされません。

もう一度マージ操作のウィンドウを表示するとコンフリクトのメッセージが消えているのが分かります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5d8f84ad-64f0-52e5-4b41-374fe34d9262.png)

この後の操作は変わりません。

また、MainからBranch1とBranch2を同時に切って、先にBranch1をマージしたためBranch2とコンフリクトした……といった場合も同様の流れを辿ります。

### 複数のブランチは切れる？

切れます。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9b165ad9-61b3-42f0-0609-c44d7ec2f5c5.png">

今存在するブランチの数がファイル名の横に表示されるのと、この`x branches`をクリックすると一覧が閲覧可能です。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9b98d35a-3489-dfa4-7268-8394bbb6fdba.png">

### マージしたブランチはどうなる？

先ほどのウィンドウの`Archived`タブに行くと存在を確認できます。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/587ac7cf-e044-2309-747a-ec110da338c5.png">

マージしてしまうと再編集はできず、View onlyと化していました。
操作感としてはView onlyのときと同様で、InspectやExportは生きています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9ab7ccc4-0693-1890-6572-af63233e5769.png)

### マージせずにアーカイブしたい場合は？

先ほどのウィンドウで、`...`から`Archive`を選ぶと実行可能。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bb98f4ae-ec60-29e4-5ce8-ba1e66607ac0.png">

マージしたのかアーカイブしたのかは、名前の横に表示されているので区別できます。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/58224a5f-e917-9ccf-9e7c-d6e1a32596dd.png">

### レビュー無しでマージはできる？

できます。

ウィンドウ右下にある`Merge without review`を押せば自分ひとりでマージ可能。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b88c3e70-8b55-297d-0c8c-2fb16cc1954e.png)

「デザイナーが自分ひとりしかいない」といったチームも少なくないと思いますが一安心ですね。

### Mergeしたブランチは元に戻せる？

厳密には戻せないけど、見かけ上戻せる……といったところでしょうか。

どういうことかと言うと、マージの際このように2つのVersion historyが作られます。
そのため`Before merge`のHistoryをRestoreすることで`元のMainファイル`を手に入れるのは可能です。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/05c11907-05e9-5532-9290-75d3d580732f.png)

ただしRestoreしてもマージ済みのBranchはLockedのままで、帰ってはきません。
`Duplicate as new file`すれば一応は大丈夫ですが……皆が不安に思うであろう「簡単に戻せるか否か」へは「一応戻せるけど慎重に」くらいの回答になってしまいます。

### Mainの変更をBranchに適用したい場合は？

Gitでmainをrebaseするイメージで、今のブランチの変更も残したいしMainの最新も取り込みたいし……そういう場面はあると思います。

近い機能はこちらの`Update from main...`なのですが、思っているようにはいかない場面も多いでしょう。

<img alt="" width="400" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/aff3de88-20e0-ceb3-bdbb-e2d1f68cd9c0.png">

先ほども少し説明しましたが比較がFrameごとなので、以下のような取り込み方はできません。
実態としてはどちらかの変更だけが活かせることとなります。

| ブランチを切った時点でのMain | 編集したMain | 編集したBranch | 編集したMainを取り込んだBranch（理想の姿） |
| --- | --- | --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/cbca1eea-b10f-5734-df2f-872b8d4b065a.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/83dee744-5a23-14f8-2d71-bca3df963752.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1bae8169-cbc5-b963-d32d-2605a65510f5.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0bd2a509-c4cc-fa9e-7c21-307740b1fa09.png) |

テキストで補足すると

- ブランチを切った時点でのMain 
    - 今のままの見た目
- 編集したMain
    - フィードナビゲーションの見た目だけ編集
- 編集したBranch
    - フィードとサイドバーの見た目を編集
- 編集したMainを取り込んだBranch（理想の姿）
    - Mainのナビゲーションの変更と、Branchでのフィードとサイドバーの変更、両方が適用されている

ただあくまで同一Frameだとこうなるという話で、以下は可能です。

- MainではFrame Aを触っていてBranchではFrame Bを触っていた
- Branch側にFrame Aの変更を取り入れたい

Gitであれば(コンフリクトが起きる可能性はあるものの)柔軟に取り込めるのでちょっと歯がゆいかもしれません。
これも今後に期待です。

### Frameの座標や名前だけを変えた場合は？

名前を変えても同一のFrameだとみなされて、おかしくなってしまうことはありませんでした。

- Main
    - home
    - top
- Branch
    - top
    - top

このような変更をしてもちゃんとhomeとtop（元々homeだった方）が対応していたので優秀ではないでしょうか？

座標だけを変更した場合も変更差分として反映されるので「うっかり大量のFrameを1px動かしてしまった」ような場面では大量の変更が出ます。

このときウィンドウには`Position, rotation, or scale`と表示されているので具体的にどこがどれだけ変わったかはチェックできません。
色や形が変わっていないことは担保できますが……若干不安が残ります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1c45b43e-6637-81ce-772e-2a32b9fe7a46.png)

### Branchにつけたコメントはどうなる？

Mainのファイルから閲覧する術はなさそうでした。
しかしArchivedなBranchを辿ればコメントが見れます。

個人的には良い塩梅かなと思っていて、何故かというと以前同じファイルにコメントし続けた結果「コメントの647番見てください！」「どれ！？！？！」となってしまったからです笑

MainにはMain用のコメント、BranchにはそのBranchでの開発固有のコメント、と分けて考えておくのが吉だと思います。

## まとめ

- 初めて使う人でもそこまで迷わない作りになっている
    - Gitを使ったことがある人であればなお理解しやすいフロー
- ある程度はビジュアルで差分も出してくれるものの、細かなパーツ単位でのマージはできない
- 正直な話「これからに期待」な箇所も多い
    - とは言えちゃんと理解していれば便利になるのも間違いなさそう