<!--
title:   Macのデスクトップをすっきりさせるテクニック5選
tags:    Alfred,Automator,Bartender,Mac,dock
id:      5dc3ef4eeea118a1aad2
private: false
-->
## これは何

- **デスクトップの乱れは心の乱れ**
- Macのデスクトップをできるだけ綺麗に整理する方法をまとめた記事です

![何もないデスクトップ](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/2fb063a9-93c0-d3f0-5299-9443229c01ef.png)

## 1. `Spotlight Search` or `Alfred`を起点にする

機能は色々ありますが、ランチャーとしての使用だけに絞った話で問題ありません。
ランチャーの使用が直接的にデスクトップをすっきりさせるわけではありませんが、これらを起点に全ての操作を実行するとデスクトップをすっきりさせやすいです。

`Spotlight Search`の場合はMacのデフォルトの機能なので`Command + Space`で起動できます。

Alfredは公式サイトからインストーラーをダウンロードするか

https://www.alfredapp.com/

Homebrewからインストールするかです。

```shell
brew install --cask alfred
```

筆者はクリップボード拡張や1Passwordとの連携も含めてAlfredの方が好きです。

## 2. Dockを非表示にする

| Dockあり | Dockなし |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bde324ab-bfbf-5de2-2dd5-f3a009ad784d.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/2fb063a9-93c0-d3f0-5299-9443229c01ef.png) | 

Dockは「Macらしさ」の要素でもありますが、ディスプレイの中で面積をとっているのも事実です。
そのため、利便性を重視してずっと非表示にしています。

「不便じゃない？」と聞かれることもあるのですが、ランチャーからのアプリ起動を前提にすれば特に問題は無いのでないでしょうか。[^1]

[^1]: 現在起動中のアプリが一覧できるのは良い機能だと思いますが、筆者の場合は30分〜1時間も触っていないアプリは落としてしまうので、余計に重要度が低いです。

ただし「完全に非表示」にはできないため、以下のステップを踏みます。

1. `System Preferences > Dock & Menu Bar`より`Automatically hide and show the Dock`にチェックを入れる
    - ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b3975578-8c6a-d075-c945-b64e83a96996.png)
1. 以下のコマンドを叩き、非表示から表示になるまでのdelayの時間を長くする
1. 擬似的にずっと非表示になる

```shell
defaults write com.apple.dock "autohide-delay" -float "100" && killall Dock
```

「試しにやってみたけど、やっぱりDockは必要だな……」というときは以下のコマンドで初期設定に戻せます。

```shell
defaults delete com.apple.dock "autohide-delay" && killall Dock
```

ちなみに初期設定のdelayは`0.5`なので以下のコマンドも同じ意味を持ちます。

```shell
defaults write com.apple.dock "autohide-delay" -float "0.5" && killall Dock
```

## 3. BartenderでMenu Barを整理する

| アイコンが多いMenu Bar | アイコンが少ないMenu Bar |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a488b5a0-7432-588d-bd9b-17f1477dbe45.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bc1f1880-7c9d-3dbf-a077-6acabe2b05df.png) |

色んなアプリを増やすとMenu Barにアイコンが増えますよね？
アイコンが増えるとごちゃごちゃしてきて嫌になるので、整理しちゃいましょう。

ただし、設定からオンオフできるアプリも多いですが、オフにしたらしたでアクセスしづらい機能がある場合も……。

そんな悩みがあっても、Bartenderを入れると簡単に整理できますし、必要なときはアクセスも容易です。

こちらも公式サイトからダウンロードするか

https://www.macbartender.com/Bartender4/

Homebrewからインストールするかです。（ちなみに有料で、15$です。）

```shell
brew install --cask bartender
```

以下のように設定画面から、アイコンの表示の種別を設定できます。

- 常に表示するアイコン
    - バッテリーやWi-Fiのステータスは流石に常時見えていて欲しいので表示しています
- 普段は隠しておくアイコン
    - それ以外の常駐アプリのアイコンは隠してあります
    - 「…」のようなアイコンを押すと、隠しているアイコンが出現します
- ずっと隠しておくアイコン
    - これは私は設定していません

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/cbc4b5bc-92d2-4414-bb34-8954dbce0fcb.png)

注意点として、日付とControl Centerのアイコンは非表示できません。

日付はいつも見るので良いのですがControl Centerはほぼ使ったことがなく……正直言って非表示にしたいので、もしやり方をご存じの方がいれば是非教えてください。

## 4. Automatorでデスクトップから自動でファイルを移動する

せっかくDockやMenu Barを綺麗にしても、デスクトップにたくさんのファイルが散らかっていては意味がありません。
ですが、「毎日綺麗にしよう」と思っても人間の意志の強さなんてたかがしれています。いつの間にか散らかるものです。

というわけでシステムの力に頼りましょう。

まずはAutomatorを使ってデスクトップのファイルを退避するアプリを作ります。

先に完成系をお見せするとこんな感じ。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b3e85cd7-1df9-889d-0a75-495ded34ff24.png)

### 起動、ファイル作成

まず、Automatorで新規ファイルを作成したら`Application`を選びます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4a3de1f6-db52-456b-7523-fbf95fb21ca1.png)

### デスクトップのアイテムを取得して、変数に格納

1. `Find Finder Items`で、Search対象を`Desktop`にする
1. `Set Value of Variables`で格納、名前は適当に（今回は`Desktop Items`としています）

### 新しいフォルダを作成して、変数に格納

1. New Folderを選択して、日付のフォーマットとフォルダを作成する場所を選ぶ
    - その際、右クリックをしてから`Ignore Input`を選ばないと上手く動いてくれません
    - ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/8c8bc27a-be18-2778-13e3-f9c413332c8b.png)
1. こちらも変数に格納、名前は適当に（今回は`New Folder`としています）

### `Desktop Items`を`New Folder`へ移動

1. `Get Value of Variable`で`Desktop Items`を選択
1. `Move Finder Items`で`To`に`New Folder`を選択

この状態でどこかのフォルダに作ったアプリを保存して、起動するとデスクトップにあるデータを設定したフォルダへ移動してくれます。
そしてもう一手間かけて自動化しましょう。

### cronで定期実行

ターミナルを開き、以下のコマンドを叩きます。

```shell
crontab -e
```

起動したら、任意の時間と保存したアプリの指定をします。
私は`Applications`に`DesktopToTemp.app`という名前で保存して、毎日23:59に起動しようと思ったので、こうなりました。

```
59 23 * * * open /Applications/DesktopToTemp.app
```

これで、デスクトップにファイルが溜まりまくる事態を回避できました。

## 5. 壁紙を単色にする

これは完全に好みの問題ですが……。
`System Preferences > Desktop & Screen Saver`に進みと壁紙が変えられるのはみなさんご存じかと思いますが、デフォルトの写真の他に「Colors」という項目があります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/088b65f9-ad4a-956d-d016-58254c9f6e5a.png)

気に入った色がなければ`Custom Color...`からカラーピッカーを起動して選べます。

## まとめ

1. `Spotlight Search` or `Alfred`を起点にする
1. Dockを非表示にする
1. BartenderでMenu Barを整理する
1. Automatorでデスクトップから自動でファイルを移動する
1. 壁紙を単色にする

こんなにこだわって何があるんだと言われれば、**何もありません**。
ただなんとなく気分が良くなるだけの代物ですが、似たような嗜好の方がいたら仲良くなれる気がしています。