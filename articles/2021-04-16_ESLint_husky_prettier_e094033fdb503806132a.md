<!--
title:   ESLintとPrettierとhuskyを導入する（2021年4月版）
tags:    ESLint,husky,prettier
id:      e094033fdb503806132a
private: false
-->
## これは何

- フォーマッター類の設定方法をまとめた記事です
    - ネットでよく解説されている方法が2018~2019年頃の設定が多い気がしたため、最近の内容にあわせて記事を書きます
    - 将来的にこの記事の内容も役に立たなくなりそうな気がするのでタイトルに2021年と入れておきました
- なぜフォーマッターが必要か？といった内容は扱いません

## 忙しい人向けのまとめ

### ESLint + Prettier

- `eslint-plugin-prettier`は使わない
    - [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)をESLintの`extends`に適用して、Prettierと競合しないようにする
    - lintはlint、formatはformat、それぞれ個別に動かす
- 古い解説記事から設定ファイルをコピペするより`yarn run eslint --init`した方が確実

### husky

- 新しく作るプロジェクトであれば`npx husky-init`すれば解決する
- 昔から存在するプロジェクトであれば[husky-4-to-6](https://github.com/typicode/husky-4-to-6)に沿ってmigrateする
    - もしくは、一度アンインストールしてv6を入れなおす……

## ESLint + Prettier

今までは、これらのプラグインorツールを導入してルールの競合を回避していました。

- [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier)
- [prettier-eslint](https://github.com/prettier/prettier-eslint)

## 2021年4月現在の推奨されている設定

しかし現在ではこれらの方法は明確に非推奨とされています。
（[Prettier公式がプラグイン使用について言及しています](https://prettier.io/docs/en/integrating-with-linters.html)）

ではどうするかというと、以下のような流れです。
（コマンド類は簡略化しています）

1. ESLintのextendsに[eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)を追加して競合を回避
1. `npm run prettier && npm run eslint`といったコマンドをユーザー側で用意して実行

```bash:インストール時
npm install --save-dev eslint-config-prettier
```

```diff_javascript:.eslintrc.js
  extends: [
    ...
+   'prettier',
  ],
// ※他の設定を上書きするために、最後の行に記載する必要がある
```

```bash:コマンドを叩く
npm run prettier --write . && npm run eslint --fix .
```


ツールで自動で連携させるのではなく、ユーザーが自分で設定するような方向性になりました。

### 余談

色々な解説記事でESLintの設定ファイルが公開されていますが、それらをコピペしても微妙に上手く動かないことも多いです。

そもそも設定ファイルを`.eslintrc`にするのか`.eslintrc.js`にするのかや、TypeScriptを使うかどうかなどで記述も変わってくるため、自分のプロジェクトにあった内容に適宜置き換えながらコピペしないといけません。

なんだかんだ煩わしいため、今から新しくESLintを入れるなら`npm install eslint --save-dev`のあとにこのコマンドを実行した方が良いと思います。

```bash:セットアップ用のコマンド
npx eslint --init
```

これを実行するといくつか質問されます。
自分のプロジェクトの内容にあわせてYES/NOなどを選んでいけば、それだけでちゃんと動く設定ファイルが出来上がります。

ちなみに[インストール方法はGetting Startedに書いてあります。](https://eslint.org/docs/user-guide/getting-started)

## husky

今までは`package.json`にこのような記述をして、commit時にhook→コマンド実行をしていたと思います。

```json:package.json
"lint-staged": {
  "*.js": ["eslint", "prettier --write"]
},
"husky": {
  "hooks": {
    "pre-commit": "lint-staged"
  }
}
```

しかし最新バージョンであるv6ではこの書き方は動作しません。

### 新しくインストールするなら

もし新しくプロジェクトを始める段階なら心配ご無用です。

```bash
npx husky-init && npm install
```

このコマンドを実行すれば、以下が一気に行われます。

1. `husky`がインストールされる
1. `package.json`のscriptに`"prepare": "husky install"`が追加される
1. プロジェクトのルートに`.husky`というフォルダが作成される
1. `.husky`の中に`pre-commit`というファイルが作成される
1. `pre-commit`の中に`npm test`と記述される

あとは`npm test`を好きなコマンドに書き換えればOKです。
`lint-staged`と併用するなら以下のようなイメージです。

```json:package.json
"scripts": {
  ...
  "lint-staged": "lint-staged"
},
"lint-staged": {
  "*.js": ["eslint", "prettier --write"]
}
```

```diff_shell:pre-commit
- npm test
+ npm run lint-staged
```

### 既にインストールされていて、アップデートするなら

既にhuskyがインストールされている場合は[husky-4-to-6](https://github.com/typicode/husky-4-to-6)というドキュメントが用意されているので、こちらに沿って進めます。

といってもおそらく大半の場合は、以下のコマンドを叩いて

```bash:husky-4-to-6
npm install husky@6 --save-dev && npx husky-init && npm exec -- github:typicode/husky-4-to-6 --remove-v4-config
```

`package.json`に書かれていた`pre-commit`のコマンドを`.husky/pre-commit`へ移すだけです。

もし`package.json`に書かれているコマンドが**ローカルにインストールしたバイナリを直接呼び出していた場合は**npx経由で呼び出す必要があるので`.husky/pre-commit`の記載が以下のような形式になります。

```shell:.husky/pre-commit
npx --no-install jest
yarn jest
```

ちなみに[huskyを新規にインストールする際のやり方](https://typicode.github.io/husky/#/?id=automatic-recommended)も[huskyをv6へアップデートするやり方](https://typicode.github.io/husky/#/?id=migrate-from-v4-to-v6)も公式に記載があります。