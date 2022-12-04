<!--
title:   GitHubでIssueの削除はできなくても、重複としてマークしたり、計画外としてクローズしたりはできる
tags:    GitHub,tips
id:      6d3385ecfec3d2ffd650
private: true
-->
## この記事の概要

GitHubで、組織が所有するリポジトリでは、大抵の人はIssueを削除できません。[^deleting-an-issue]
間違えて作ったIssueや要らなくなったIssueも、Closeするしかないです。

[^deleting-an-issue]: 組織リポジトリの場合、管理者または所有者のアクセス許可を持つアカウントのみがIssueを削除できます。
https://docs.github.com/ja/issues/tracking-your-work-with-issues/deleting-an-issue

しかし、ちゃんと完了したIssueのようにクローズするのも若干気持ち悪いです。
「訳あってクローズしたけど、不要なIssueだった」ことがわかる方法を記事にしました。

## Mark as a duplicate

コメントの本文に`Duplicate of`と記載し、その後にIssueあるいはPull Requestの番号を入力して投稿します。
Pull Requestに`Close {Issue番号}`と書くのと同じ書式です。

これにより「重複」としてマークができます。

https://docs.github.com/ja/issues/tracking-your-work-with-issues/marking-issues-or-pull-requests-as-a-duplicate

## Close as note planned

コメントの`Close issue`の横の下向き矢印をクリックし`Close as note planned`を選択してからクローズします。
クローズされる他、Issue検索の際に`reason:"not planned"`と入れると絞り込みができます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/697351b5-6b27-2057-ac12-a8dbf0e3c073.png)


https://github.blog/changelog/2022-03-10-the-new-github-issues-march-10th-update/#%F0%9F%95%B5%F0%9F%8F%BD%E2%99%80%EF%B8%8F-issue-closed-reasons

## 最後に

これまでは「普通にクローズすると、まるでちゃんと完了したIssueみたいに見えて嫌だなあ」と思い、コメントに理由を書いていました。
（◯◯の理由で実施しないことになった。など。）

あくまでメモとしての記載で対症療法的だったのが、公式にサポートされて非常にありがたいです。

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox

Devトークでのお話してくださる方も募集中です！

https://jobs.qiita.com/employers/qiita-inc/dev_talks/489