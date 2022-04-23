<!--
title:   Chromaticでビジュアルリグレッションテスト
tags:    Chromatic,VisualRegressionTest,storybook
id:      1ddfe5f8f4917f9c0999
private: false
-->
## この記事の概要

ビジュアルリグレッションテストを導入しよう！と思い立ったのですが、よく出てくるreg-suitを使った方法は大変そうに感じてしまいました。
画像の格納先やスクリーンショット保存用ツールなど、個別に色々用意する必要があり、そこまで技術力のない自分だけで用意できるかなあ……と一抹の不安。

そんな折、Chromaticを試してみたらかなり楽だったので使ってみることにしました。

https://www.chromatic.com/

というわけで、備忘録をかねてChromaticの導入について記事にします。

## 説明しないこと

- Storybookの導入の仕方
    - Chromaticを使うにあたりStorybookは必須ですが、Storybookの導入は既に済んでいるものとします
- 具体的な設定や運用の話
    - スレッショルドをどれくらいにするか、スナップショットを撮るブラウザはどうするかなど、細かめな設定については触れません
- Bitbucket, GitLabでの連携方法
    - この後すぐに「GitHub, Bitbucket, GitLabと連携できます」と記載していますが、具体的な使用方法はGitHubに絞って説明します

## 導入

まずは[Chromaticのサイト](https://www.chromatic.com/)にアクセスし、右上にある`Sign up`から進みます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/00c5db99-fd1e-3c39-8164-14c39698fcad.png)

GitHub, Bitbucket, GitLabのアカウントと接続できます。
メールでの登録もできますが、Pull Requestと連携してテストを走らせることを考えると上の3つのどれかを選ぶのが良いでしょう。

これ以降はGitHubに絞って説明します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/608d3e4d-373e-8104-bfb7-5c9bfa410dbf.png)

サインアップが完了するとプロジェクトを追加できるようになります。
`Add project`から進みます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/5a719419-2263-2327-80ac-f4b2c523be13.png)

既存のリポジトリと連携するか、新規に作成するかを選べます。
先にStorybook導入まで完了させておいたリポジトリと連携するのが良いと思われます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f225f9b4-c0a8-05e3-625c-875cee120950.png)

次の画面に進むとこのように指示されます。
dependencyとしてChromaticをインストールし、実行します。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/38aed391-4fb6-634d-4c20-f204e6d88870.png)


```zsh
% npx chromatic --project-token=XXXXXXXXXXXX

Chromatic CLI v6.5.3
https://www.chromatic.com/docs/cli

  ✔ Authenticated with Chromatic
    → Using project token 'XXXXXXXXXXXX'
  ✔ Retrieved git information
    → Commit 'COMMIT_HASH' on branch 'BRANCH_NAME'; no ancestor found
  ✔ Collected Storybook metadata
    → Storybook 6.4.9 for React; supported addons found: Actions, Essentials, Links
  ✔ Storybook built in 16 seconds
    → View build log at /${YourRepositories}/build-storybook.log
  ✔ Publish complete in 10 seconds
    → View your Storybook at https://XXXXXXXXXXXX.chromatic.com
  ✔ Started build 1
    → Continue setup at https://www.chromatic.com/setup?appId=XXXXXXXXXXXX
  ✔ Build 1 auto-accepted
    → Tested 1 story across 1 component; captured 1 snapshot in 4 seconds

ℹ Speed up Continuous Integration
Your project is linked to GitHub so Chromatic will report results there.
This means you can pass the --exit-once-uploaded flag to skip waiting for build results.
Read more here: https://www.chromatic.com/docs/cli#chromatic-options

✔ Build passed. Welcome to Chromatic!
We found 1 component with 1 story and took 1 snapshot.
ℹ Please continue setup at https://www.chromatic.com/setup?appId=XXXXXXXXXXXX

⚠ No 'chromatic' script found in your package.json
Would you like me to add it for you? [y/N]y

✔ Added script 'chromatic' to package.json
You can now run it here or in CI with 'npm run chromatic' or 'yarn chromatic'.

ℹ Your project token was added to the script via the --project-token flag.
If you're running Chromatic via continuous integration, we recommend setting
the CHROMATIC_PROJECT_TOKEN environment variable in your CI environment.
You can then remove the --project-token from your package.json script.
```

:::note warn
自分が導入したタイミングではv6.5.3でしたが、2022年4月23日現在の最新版は6.5.4です。
そのため上記のログでもChromaticとStorybookのバージョンが少し古いです。
:::

:::note alert
上記で`Would you like me to add it for you? [y/N]`と聞かれるタイミングがあり、`y`と答えるとnpm-scriptsに`chromatic`のコマンドを追加してくれます。
便利なのですが、この段階ではトークンがそのままpackage.jsonに書かれてしまうので`.env`に移しておく方が良いでしょう。
`CHROMATIC_PROJECT_TOKEN`という名前で登録しておけばOKです。
:::

ひとまず試しに導入した状態なので、Storyは1つしかありませんがパブリッシュ完了です。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/2ef3a246-7f9a-dc93-8bf6-7a378ea0acbe.png)

## GitHub Actionsでの実行

現状では、手動でコマンドを叩いたタイミングのみChromaticが更新されるだけです。
Pull Requestを作成したタイミングでビルドが走り、レビュー項目として組み込まれるように設定します。

公式のやり方と多少変えている箇所もありますが、基本はこちらに書いてある通りです。

https://www.chromatic.com/docs/github-actions

まずはGitHubにsecretsを追加します。
先ほど発行されたトークンを`CHROMATIC_PROJECT_TOKEN`の名前で登録すればOKです。
登録方法は公式ドキュメントが分かりやすいのでリンクを載せておきます。

https://docs.github.com/ja/actions/security-guides/encrypted-secrets

次にworkflowを定義します。
公式ドキュメントだと`on: push`をトリガーにしていましたがそこまでは必要なかったのでPull Request作成などのタイミングにしておきました。
また、`actions/checkout@v2`を使っていて、そこも少し違います。

```yaml:.github/workflows/chromatic.yml
name: Chromatic

on:
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]

jobs:
  chromatic-deployment:
    timeout-minutes: 10
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
        with:
          fetch-depth: 0
      - name: Install dependencies
        run: yarn install
      - name: Publish to Chromatic
        uses: chromaui/action@v1
        with:
          projectToken: ${{ secrets.CHROMATIC_PROJECT_TOKEN }}
```

上記の設定が完了した上でPull Requestを作成すると、このような表示になります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/a76aa437-e162-48dd-01e7-297a0f768299.png)

`UI Review`の項目の`Details`からは以下のような画面に飛びます。
まず一覧があり、コンポーネントのサムネイルを押下すると詳細な変化を示すビューに変わります。

`Review now`からレビュワーをアサインし、意図した通りの変更であればApproveをもらいます。
問題があればコメントをもらいましょう。

| リンク先 | コンポーネントのサムネイルを押下すると |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/0cb7c3bd-3f6b-fc7e-2389-ab70274b2944.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/c0133aab-a013-65a2-a46d-0b82c8df0468.png) |

すべてのチェックが通るとPull Requestもこのようになり、mergeできるようになります。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/ecbe084d-a673-c31a-19ab-ab0279e9f32c.png)

## セキュリティ、公開範囲

業務で使うとなるとセキュリティの設定が気になると思いますが、基本的には連携したリポジトリの権限に沿って利用可否が決まるようです。
また、GitHubのアカウントは無いけど確認はしたい人（プロジェクトのオーナーなど？）がいる場合も、External collaboratorsとして別途追加可能です。

## まとめ

- Chromaticさえ使えばスクリーンショットの撮影、画像の保存、画像の比較を全部やってくれる
- 簡単にCI/CDに組み込める
- 公開範囲は基本リポジトリの権限に準拠しているので、特別意識しなくても良い

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox