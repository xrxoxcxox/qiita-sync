<!--
title:   HTMLとCSSを書くMacユーザー向け Visual Studio Codeの効率的な設定と使い方
tags:    AdventCalendar2022,CSS,HTML,VSCode,VisualStudioCode
id:      46fce6bd54092d9be7da
private: true
-->
## これは何

HTMLとCSSを書くMacユーザーのためのVisual Studio Codeの使い方の記事です。

Visual Studio Codeは非常に多機能なため、単に「便利な使い方を記事にしました」だと際限なく書けてしまいます。
そのため、今回はタイトルにあるような属性の人に絞ってまとめました。

## 設定

`command + ,` で設定画面を開き、変更できます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/237e3307-217b-5335-f38e-7770f9c50405.png)

細かい内容ですが、設定の有無によって操作のしやすさも大きく変わります。
初期設定のまま使わずに使いやすいようにカスタムしておきましょう。

### Bracket Pair Colorization

検索窓に`Editor › Bracket Pair Colorization: Enabled`と入力して、出てきたオプションにチェックを入れます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/10246bee-4508-effa-b0a2-a7cc5d64bbee.png)

対応する括弧のペアの色が揃います。
入れ子になった括弧や、遠い位置にある括弧の対応関係が見やすくなります。

### Editor: Drag And Drop

検索窓に`Editor: Drag And Drop`と入力して、出てきたオプションのチェックを外します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/854f4771-8c9d-b513-446a-d85ab5cb1e46.png)

選択した行をドラッグして移動できる機能がデフォルトで有効になっているのですが、事故の原因になりがちなので無効にしましょう。

### Editor: Format On Paste, Format On Save, Format On Type

検索窓に`Editor: Format On Paste`, `Editor: Format On Save`, `Editor: Format On Type`と入力して、出てきたオプションにそれぞれチェックを入れます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1d425d93-15c1-8e32-75c4-529c75774d79.png)

ペースト、保存、入力をした後にコードが整形されます。

[拡張機能のPrettier](#prettier)をインストールしていると、そのルールにあわせて整形されます。

### Files: Insert Final Newline, Trim Final Newlines, Trim Trailing Whitespace

検索窓に`Files: Insert Final Newline`, `Files: Trim Final Newlines`, `Files: Trim Trailing Whitespace`と入力して、出てきたオプションにそれぞれチェックを入れます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e2582168-6159-0061-09a7-5f966c51e933.png)

最終行の空白行や、行末の余分な空白を制御できます。

:::note warn
プロジェクトのPrettierの設定によっては、これらの設定がバッティングしてしまう可能性もあります。実際の挙動を見ながら設定してください。
:::

## 拡張機能

`shift + command + X`で設定画面を開き、インストールやアンインストールができます。

拡張機能は、入れるだけで表示が見やすくなるものも多いのです。
環境構築として最初に入れるのが良いです。

もちろん自分で操作しないといけない拡張機能もありますから、積極的に使う練習をしましょう。

### Auto Rename Tag

https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag

開始タグを編集するだけで、自動で終了タグも編集できます。
階層の深いコードを触っているときは、対象を見失ったり間違えたりすることもあるので、拡張機能で自動化しましょう。

:::note warn
設定の`Editor: Linked Editing`でも同じことができるはずなのですが、あちらは上手く動かないことが多いように感じるので、拡張機能をお勧めしています。
:::

### CSS Peek

https://marketplace.visualstudio.com/items?itemName=pranaygp.vscode-css-peek

HTMLを触っているとき、CSSを開かなくてもクラスで定義されている内容が分かります。
定義されたファイルを探す手間が省けて時短に繋がります。

- 対象のCSSを開いてジャンプする
    - クラス名にカーソルを当てて `F12` を押す
- HTML内にインラインで表示する
    - クラス名にカーソルを当てて `option + F12` を押す
- 小さいウィンドウで表示する
    - `command` を押しながらホバー

:::note warn
ただし、コード量の多いリポジトリだと上手く動いてくれない場合があります。
:::

### Git Graph

https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph

CLIだけでGit操作を完結している場合コミットグラフがみづらいです。
拡張機能を入れることで見やすくなり、コミットごとの差分も確認しやすくなります。

### GitLens

https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens

ある行にカーソルをあわせると、いつ誰がどんなコミットとして変更できたかを確認できます。
先ほどのGit Graphと被る機能も多いですが、変更差分をその場で確認するときはGitLensの方が見やすいでしょう。

### HTML CSS Support

https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css

CSSファイルで定義された内容をもとに、HTML内で補完が効きます。
CSS Peekと同様に、毎回CSSファイルを開く手間が省けます。

### htmltagwrap

https://marketplace.visualstudio.com/items?itemName=bradgashler.htmltagwrap

テキストを選択した状態で `option + W` を押すと、HTMLタグで囲まれます。
デフォルトだと`p`タグで囲まれて、その状態から編集すれば他のタグにも変更できます。

### markuplint

https://marketplace.visualstudio.com/items?itemName=yusukehirao.vscode-markuplint

HTMLは構文に問題があっても問題なく表示できてしまうことが多いです。
一般的な構文のチェックに加え、アクセシビリティの観点でもチェックできます。

日本人の方が作っているのでドキュメントが日本語なのも心強いポイントです。

### Prettier

https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode

コードの整形をするツールとして最も有名なものがPrettierです。
インデントの数やセミコロンの有無など、人間の目視では見逃してしまいかねない部分はツールに任せましょう。

:::note warn
プロジェクトの依存関係にPrettierを追加するか、マシン内でグローバルにPrettierを導入するか、どちらかがお勧めだそうです。
具体的なやり方はこの記事では触れませんが、インストールしておきましょう。
:::

拡張機能をインストールした後、設定画面の検索窓に`Prettier`と入力して、セレクトから`Prettier - Code formatter`を選択します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9cb32602-6e9a-518e-5569-4d878a1891c6.png)

### Stylelint

https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint

Prettierと似たようなツールで、CSSの書き方をチェックできます。
可能であれば整形まで含めてツールに頼りましょう。

:::note warn
こちらもPrettierと同様に、プロジェクトか自分のマシンにインストールしておきましょう。
:::

Stylelintは設定するまでが少し大変なので、参考記事を載せておきます。

https://qiita.com/y-w/items/bd7f11013fe34b69f0df

https://qiita.com/teixan/items/dd612f13ed002528a6dd

### Trailing Spaces

https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces

半角スペースや全角スペース、タブなど空白文字を分かりやすく表示します。
設定の`editor.renderWhitespace`でも似たようなことはできますが、こちらの拡張機能の方がより分かりやすいです。

### vscode-icons

https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons

エクスプローラーファイルやフォルダの先頭にアイコンがつきます。
それだけですが、ファイルの種類を見分けやすくなるので便利です。

他にもアイコンを変更する拡張機能は存在するので、お好みでどうぞ。

## ショートカットキー

キーを叩いたときの挙動を文章と動画で載せています。

抜粋して紹介しているので、もっと知りたい方は以下をどうぞ。

https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf

### 行のコピー

`command + C`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/491be664-be52-1e91-d1f0-522ad87941e3.png)

カーソルのある行を1行まるまるコピーします。
テキストを選択しなくてもコピーができるので早いです。

### 行のカット

`command + X`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5b043abd-281f-4d95-0ba5-41d09d33165c.png)

カーソルのある行を1行まるまるカットします。
カットとして使いたいとき以外にも、行の削除としても便利です。

:::note
行の削除にもショートカットがありますが、<kbd>shift</kbd> + <kbd>command</kbd> + <kbd>K</kbd>で微妙に押しづらいため、カットでの代用を紹介しました。
:::

### 行の移動

`option + ↑` / `option + ↓`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d8e7ddc1-c1e9-cc1a-99cc-c8b3dbedb799.png)

カーソルのあっている行（選択している行）を、上キーなら上へ、下キーなら下へ移動します。
インデントも調整しながら移動できるため作業が早いです。

### 行のコピー

`shift + option + ↑` / `shift + option + ↓`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a78c8cdc-0013-09a4-aa46-01a94505d871.png)

カーソルのあっている行（選択している行）を、上キーなら上へ、下キーなら下へコピーします。

### 行を上/下に追加

`command + return` / `shift + command + return`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/46e2453d-038a-4657-00ae-9e1b52b78ab3.png)

単なる改行ではなく、インデントを維持した状態で上/下に行を追加します。

### 行の先頭/最後へカーソルを移動

`command + ←` / `command + →`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a490fea6-7689-46cb-26d4-2cc6c1cba755.png)

### コマンドパレットの表示

`shift + command + P`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/6a28dd16-bbd0-08bd-d1ff-68364aab869c.png)

コマンドパレットから色々なアクションを実行できます。
例えば`Emmet: Wrap with Abbreviation`と打ち込むと、選択中のテキストを任意のタグで囲めます。

### クイックオープン

`command + P`

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5cc4f043-8366-68c3-5cf6-abd3514594f0.png)

ファイル名を打ち込むと候補が表示され、<kbd>return</kbd>で開くことができます。
名前を覚えていれば、エクスプローラーから開くよりも早いです。

### 対応する括弧へのジャンプ

`shift + command + \`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/92b624b8-154e-e102-7328-b22271820b22.png)

閉じ括弧にいるときに開き括弧にジャンプする、などに使えます。

### カーソルの追加

`option` + クリック

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b33190b6-7fa1-6dbf-c3ba-f8511055b8bc.png)

カーソルを追加した後にテキストを入力したり削除したりすると、すべてのカーソルに反映されます。

---

`option + command + ↑` / `option + command + ↓`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c654e04d-405b-538c-f780-fa09874323df.png)

カーソルのあっている箇所から、上キーなら上へ、下キーなら下へカーソルを追加します。

どちらも、同じテキストを複数の箇所に追加したいときなどに便利です。

### 現在選択しているテキストと同じ内容を選択

`shift + command + L`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/115eef5b-8b2e-e9ec-c8a0-2eb248ba5e30.png)

選択している内容で、ファイル内に同じテキストがあればすべて選択します。
一括で置換するときなどに使えます。

---

`command + D`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a548a1d2-bbe0-1e0e-7d1c-6d5dc4c4f165.png)

選択している内容と同じテキストを、ファイルの下に向かって1つずつ選択を増やしていきます。

---

`command + G` / `shift + command + G`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f3c21d66-c1eb-cf6e-5400-7dc3aa678c18.png)

選択している内容と同じテキストのうち、次のものや前のものにジャンプします。

### 検索

`command + F`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/184b5545-c1ae-fc66-0a57-2a9455912930.png)

ファイル内で文字の検索ができます。

### 置換

`option + command + F`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/620f13f3-c8a2-3cda-b1bc-43c9fce33703.png)

ファイル内で文字の置換ができます。
正規表現も使えます。

### 行へジャンプ

`control + G`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/47a05b10-e91f-8c4a-1d31-a43dcceeb9a5.png)

ショートカットキーを押した後、数値を入力するとその行にジャンプします。

### 開いているファイルを閉じる

`command + W`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9a98cbbe-a4af-5876-4965-b82b3e4cfd55.png)

### 閉じてしまったファイルを再度開く

`shift + command + T`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/161fece0-e341-085b-e2be-b791fc336dbf.png)

### エディタを分割する

`command + \`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d894100a-0335-90d6-abcd-eeec7c082957.png)

左側はHTML、右側はCSS、などのレイアウトにする際に早いです。

### エディタを移動する

`option + command + →` / `option + command + ←`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/2dd67bb6-a90a-c968-f0a0-4c7b7311f243.png)

分割したエディタを移動できます。

### プロジェクト全域を検索する

`shift + command + F`

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/21ba50fc-1f1b-7a8e-e2a8-a227e16b1cce.png)

ファイル内だけでなく、プロジェクト全域から検索ができます。

### ターミナルを開く / 閉じる

``control + ` ``

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/48dc89cc-e00f-a5aa-8391-6ab83afb2344.png)

## Emmet

Emmetとは、HTMLとCSSの入力補助をしてくれる仕組みです。
Visual Studio Codeにはデフォルトで入っているので、導入は不要です。

抜粋して紹介しています。
もっと知りたい方はこちらをどうぞ。

https://docs.emmet.io/cheat-sheet/

### HTML

#### タグを入力する

`div`と入力した後に`tab`を押すと以下のコードになります。

```html
<div></div>
```

#### クラス付きのタグを入力する

`div.some-class`と入力した後に`tab`を押すと以下のコードになります。

```html
<div class="some-class"></div>
```

#### 中にテキストの入ったタグを入力する

`p{テキスト}`と入力した後に`tab`を押すと以下のコードになります。

```html
<p>テキスト</p>
```

#### 入れ子のタグを入力する

`ol>li`と入力した後に`tab`を押すと以下のコードになります。
入れ子は複数階層指定できます。

```html
<ol>
  <li></li>
</ol>
```

#### 複数の入れ子のタグを入力する

`ol>li*3`と入力した後に`tab`を押すと以下のコードになります。

```html
<ol>
  <li></li>
  <li></li>
  <li></li>
</ol>
```

#### 組み合わせて入力する

`ol.list>li.list-item{item$}*3`と入力した後に`tab`を押すと以下のコードになります。

```html
<ol class="list">
  <li class="list-item">item1</li>
  <li class="list-item">item2</li>
  <li class="list-item">item3</li>
</ol>
```

### CSS

#### displayを指定する

| ショートハンド | 展開内容 |
| --- | --- |
| db | display: block; |
| di | display: inline; |
| dib | display: inline-block; |
| df | display: flex; |
| dg | display: grid; |
| dn | display: none; |

#### 幅や高さを指定する

以下の表の`w`を`h`に置き換えれば`width`が`height`になります。

| ショートハンド | 展開内容 |
| --- | --- |
| w100 | wifth: 100px; |
| w100p | wifth: 100%; |
| w100e | wifth: 100em; |
| w100r | wifth: 100rem; |

#### 余白を指定する

以下の表の`m`を`p`に置き換えれば`margin`が`padding`になります。

| ショートハンド | 展開内容 |
| --- | --- |
| m10 | margin: 10px; |
| m10-20 | margin: 10px 20px; |
| m10-20-30 | margin: 10px 20px 30px; |
| m10-20-30-40 | margin: 10px 20px 30px 40px; |
| m10p | margin: 10%; |
| m10e | margin: 10em; |
| m10r | margin: 10rem; |
| mt10 | margin-top: 10px; |
| mr10 | margin-right: 10px; |
| mb10 | margin-bottom: 10px; |
| ml10 | margin-left: 10px; |

#### その他プロパティ色々

| ショートハンド | 展開内容 |
| --- | --- |
| bd | border |
| bg | background |
| bgc | background-color |
| c | color |
| fz | font-size |
| fw | font-weight |
| mah | max-height |
| maw | max-width |
| mih | min-height |
| miw | min-width |

## 番外編：不要な拡張機能

昔は活躍していたけど、現在は標準機能として搭載されているものなどです。
紹介されている記事を見かけることもあると思いますが、導入しなくて大丈夫です。

### Auto Close Tag

https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag

開始タグを打ち込んだ時点で終了タグも生成できる拡張機能です。

標準の機能で実現できるため不要です。

### Bracket Pair Colorizer 2

https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2

対応する括弧のペアの色が揃う拡張機能です。

[設定のBracket Pair Colorization](#bracket-pair-colorization)を有効にすればほぼ同じ機能を使えるため、不要です。

### Japanese Language Pack for Visual Studio Code

https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-ja

Visual Studio Codeを日本語化できる拡張機能です。

日本語化すること自体は問題ありませんが、日頃から英語での表記に慣れておいた方が良いです。
英語で検索した方が結果が多いのと、リリースノートなども英語で書かれているため推奨しません。

### zenkaku

https://marketplace.visualstudio.com/items?itemName=mosapride.zenkaku

全角スペースにハイライトがつく拡張機能です。

設定の`Editor › Unicode Highlight`で、デフォルトで全角スペースにはハイライトがつくため不要です。

## 最後に

こういう、地味な設定のオンオフやショートカットの習熟度が、なんだかんだ仕事の速さを左右すると思っています。
スピーディーにコードを書くためにも、ツールの設定を見直したり、ショートカットが手に馴染むまで練習してみましょう！

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox

Devトークでのお話してくださる方も募集中です！

https://jobs.qiita.com/employers/qiita-inc/dev_talks/489