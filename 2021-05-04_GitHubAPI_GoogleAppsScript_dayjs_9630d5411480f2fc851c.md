<!--
title:   今日対応したIssueやプルリクを一覧するためにGASを書いた
tags:    GitHubAPI,GoogleAppsScript,dayjs
id:      9630d5411480f2fc851c
private: false
-->
## これは何

- 今日対応したIssueやプルリクを一覧したい、という欲求が自分の中にあったので、GASで実現するまでの流れをまとめた記事です
    - 書き方を少し変えれば「今週対応したIssue」などもすぐに作れるはず

## 完成物

ブックマークレットからMarkdown形式で出力できるので、各種メモツールでまとめるのに親和性が高いと思っています

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d4c58e2f-635b-62e5-4485-3a997b13314a.gif)

```javascript
const doGet = () => {
  const today = dayjs.dayjs().format('YYYY-MM-DD');

  const url = `https://api.github.com/issues?state=all&since=${today}`;
  const token = PropertiesService.getScriptProperties().getProperty('GITHUB_TOKEN');
  const options = {
    headers: {
      Authorization: `token ${token}`,
    },
  };

  const response = UrlFetchApp.fetch(url, options);
  const responseToJson = JSON.parse(response);

  const issuesList = (issues) => {
    let result = `## 対応したIssue、Pull Request\n`;
    for (let i in issues) {
      result += `- ${issues[i].closed_at && 'Closed'} [${issues[i].title}](${issues[i].html_url})\n`;
    }
    return result;
  };

  const output = ContentService.createTextOutput(issuesList(responseToJson));

  return output;
};
```

## 全体の流れ

1. GitHubでPersonal access tokensを発行
1. GASで新規プロジェクトを作成→コードを書く準備
1. GASからGitHub APIを叩く→書式を整える
1. デプロイ

## GitHubでPersonal access tokensを発行

まずはGitHubにて`Settings > Developer settings > Personal access tokens`と進んでください。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/00464f25-f30e-7ea7-7c6b-b1c1dfcee701.png)

ページの右上あたりにある`Generate new token`を押して次に進みます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/1029ca4c-24b7-e924-a16d-25c8188c4a62.png)

スコープは`repo`を選択して、`Generate token`を押すとトークンが発行されます。

## GASで新規プロジェクトを作成→コードを書く準備

### 新規プロジェクトを作成

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/3491097a-a866-dc16-dbc6-ac2bcf8f938a.png)

[Apps Scriptのホーム画面](https://script.google.com/home)へアクセスし、`新しいプロジェクト`を押して新規作成します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/33714b39-850b-c97a-24e3-4296c66efb37.png)

するとこのような画面が開くのですが、実はこの状態だとこの後の作業が少しやりづらいです。
なので`以前のエディタを使用`を押して、最初だけ古いバージョンのUIを使いましょう。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/bfa42aa9-32ba-fa3a-cf4f-2642e77a09f1.png)

`以前のエディタを使用`を押すとこのアンケートが開くので、私は`Missing script property editing`に入れておきました。
まさにこの`プロパティ`を編集する機能が新しいUIから消えてしまったので、ここから意見を表明しておきます。

### GitHubのPersonal access tokensを登録

トークンをコードの中に直接書くのはよろしくありません。
そのため`プロパティ`という機能を使って外からは見えないようにしましょう。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/4efdc3ef-3b6d-ddb5-4574-f509dcba5697.png)

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/3ed5d541-5de1-3310-8e0d-fdea0b8d0657.png)

旧版のエディタで`ファイル > プロジェクトのプロパティ > スクリプトのプロパティ`を開いて`行を追加`を選択。
プロパティ名は自由につけて大丈夫ですがここでは`GITHUB_TOKEN`と名付け、値には先ほど生成したトークンを入れます。

保存したら残りの作業は新しいエディタでも問題ありません。
私は「テンプレートリテラルが便利だから」程度の理由で新しいエディタに戻ってコードを書き始めましたが、慣れているならこのまま旧エディタで進めても良いと思います。

### タイムゾーンの変更

GASはプロジェクト作成時点でタイムゾーンが`Asia/Tokyo`に設定されているのですが、初期設定では不可視です。
GitHub APIからのレスポンスは`UTC+0000`なのに`new Date()`で得られる時間が`UTC+0900`になって不整合がおき、気持ち悪いので揃えました。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/33e31a97-f2c9-542a-2279-53920f9aa024.png)

まずはサイドバーの`プロジェクトの設定`を選びます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/e0267ba9-0b90-f05d-b367-9c6456cda1dd.png)

`「appsscript.json」マニフェスト ファイルをエディタで表示する`にチェックを入れてエディタへ戻ります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ff34e7dd-4c67-ebd3-fb8f-363c792ac80d.png)

するとこのように`appsscript.json`というファイルが増えて、タイムゾーン設定がされているのがわかります。
この`Asia/Tokyo`を`Etc/UTC`へ変更しておきましょう。

### Day.jsの導入

そんなに大したことはしていないのでライブラリを使う必然性は薄いのですが、一応入れています。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/b374b612-d89c-29a8-796d-9c3b60705c31.png)

サイドバーの`ライブラリ`にある`+`ボタンをクリックして

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/dd7f4124-d5c6-e415-7fb5-653fa0728b40.png)

出てきたウィンドウの`スクリプトID`に`1ShsRhHc8tgPy5wGOzUvgEhOedJUQD53m-gd8lG2MOgs-dXC_aCZn9lFB`と入力して検索をします。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/8b5bde54-d550-7e23-9844-e2e1d3c3500c.png)

おそらくこんな感じで画面が切り替わるので、`追加`を押せばOK。
サイドバーの`ライブラリ`の下に`dayjs`が増えたと思います。

あとは普段Day.jsを使用する書き方の前に`dayjs.`をつけて書けばGAS上でも動くようになります。

```javascript:例
// 普段の書き方
const now = dayjs();

// GASでの書き方
const now = dayjs.dayjs();
```

### 関数名の変更

プロジェクトを作成した時点だと、コードはこのようになっていると思います。

```javascript
function myFunction() {
  
}
```

今回はブックマークレットとして動かすつもりなので、関数名を`doGet`に変更します。（`doGet`以外の名前だと動きません）
あとついでにアロー関数にしました。

```javascript
const doGet = () => {
  
}
```

## GASからGitHub APIを叩く→書式を整える

### とりあえずAPIを叩いてレスポンスを得る

```javascript
const doGet = () => {
  const url = `https://api.github.com/issues`;
  const token = PropertiesService.getScriptProperties().getProperty('GITHUB_TOKEN');
  const options = {
    headers: {
      Authorization: `token ${token}`,
    },
  };
  const response = UrlFetchApp.fetch(url, options);
  const responseToJson = JSON.parse(response);
}
```

不意に`const token = PropertiesService.getScriptProperties().getProperty('GITHUB_TOKEN');`というコードが出現しました。
`PropertiesService.getScriptProperties().getProperty('GITHUB_TOKEN')`という部分が、前のセクションで登録した`プロパティ`から値を呼び出すための書式です。

### 自分の欲しい条件にあわせてエンドポイント`を変更する

現段階では、自分がアサインされているIssueを全部取得するようになっています。

私の場合は以下の条件を考えていました。

- 今日対応したものだけを取得したい
- クローズできたものも、できなかったものも、並列で並べたい

そのため、コードはこのように変化します。
先ほど準備した`Day.js`はここで使用しました。

```diff_javascript
  const doGet = () => {
+   const today = dayjs.dayjs().format('YYYY-MM-DD');
-   const url = `https://api.github.com/issues`;
+   const url = `https://api.github.com/issues?state=all&since=${today}`;
    const token = PropertiesService.getScriptProperties().getProperty('GITHUB_TOKEN');
    const options = {
      headers: {
        Authorization: `token ${token}`,
      },
    };
    const response = UrlFetchApp.fetch(url, options);
    const responseToJson = JSON.parse(response);
  }
```

`state`には`open`, `close`, `all`があり、`since`はISO8601で記載された日時以降にアップデートがあったIssueだけに絞ってくれます。

もちろんこれ以外にも色々な書き方ができるので、気になった方は[公式のAPIのドキュメント](https://docs.github.com/en/rest/reference/issues)を読んでみてください。

### 書式を整えて画面に表示する

普段の振り返りの習慣を含めて、以下のようにイメージしていました。

- Markdownで、タイトルがリンクになっているリストとして作成したい
- クローズしたものとそうでないものを見分けたい
- セクションの見出しをつけたい

結果、このようなコードを書きました。

```diff_javascript
  const doGet = () => {
    const today = dayjs.dayjs().format('YYYY-MM-DD');
    const url = `https://api.github.com/issues?state=all&since=${today}`;
    const token = PropertiesService.getScriptProperties().getProperty('GITHUB_TOKEN');
    const options = {
      headers: {
        Authorization: `token ${token}`,
      },
    };
    const response = UrlFetchApp.fetch(url, options);
    const responseToJson = JSON.parse(response);
+   const issuesList = (issues) => {
+     let result = `## 対応したIssue、Pull Request\n`;
+     for (let i in issues) {
+       result += `- ${issues[i].closed_at && 'Closed'} [${issues[i].title}](${issues[i].html_url})\n`;
+     }
+     return result;
+   };
+   const output = ContentService.createTextOutput(issuesList(responseToJson));
+   return output;
  }
```

`ContentService.createTextOutput()`は、簡単に言うならアクセスがあった際にテキストとして画面に表示するものを設定するためのコードです。
これによって、ブックマークレットを起動したら画面に文字が表示されるようにできます。

## デプロイ

いよいよ、デプロイするだけです。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c815143e-c2f9-459f-b269-10fe5185bdd3.png)

画面右上にある`デプロイ`ボタンを押して、`新しいデプロイ`を選択。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5139e2cf-a64e-c206-c65d-5d02ffb38243.png)

デプロイの種類から`ウェブアプリ`を選んで、デプロイボタンを押せばOKです。
表示されたURLをコピーしてブックマークに登録しておけば……

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/d4c58e2f-635b-62e5-4485-3a997b13314a.gif)

最初に貼ったgifのように、今日やったIssueやプルリクをまとめて表示することができます。