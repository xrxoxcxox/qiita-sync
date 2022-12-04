<!--
title:   見た目重視でoutline:none;やoutline:0;を指定する必要は本当に無くなった
tags:    AdventCalendar2022,CSS,アクセシビリティ
id:      2f7ef9e55e6ecc7d7095
private: true
-->
## この記事の概要

以前[見た目重視でoutlineを消したいにしても、せめてこうしませんか？](2021-04-17_CSS_Design_HTML_82e083b3f47309873262.md)という記事を書きました。
当時は[what-input](https://github.com/ten1seven/what-input)というライブラリを提案をしていたのですが、最近のブラウザアップデートでいよいよ不要になった感があり、記事にしました。

## これまで起きていたこと

非常にざっくりとまとめると以下のようなことが起きていました。

- buttonやinputなどの要素はフォーカスが当たるとフォーカスリングという輪が表示される
- 上記の要素を**クリックした際**もフォーカスリングが表示されるため、見た目を好ましく思わずに`:focus { outline:none; }`のようなコードを書く
- 結果、クリックした際だけでなく、**キーボード操作でフォーカスを当てた際も**フォーカスリングが表示されなくなってしまう

クリック時、このように枠線がつきます。
色や太さは変えられるものの「そもそも枠がつくこと自体が嫌」と消されていた印象があります。

| リンク | ボタン |
| --- | --- |
| ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/f914b5bd-5483-fdd2-ac71-20955cbaffe0.png) | ![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/517a1586-db17-d5f8-1a6a-9e75f9a1525d.png) |

## `:focus-visible`の挙動がデフォルトになった

`:focus-visible`はタブ移動のときだけoutlineを表示させる擬似クラスです。

これまで使われていた`:focus`はタブでもマウスでもoutlineを表示させていたので`outline:none;`が指定されてしまいがちでした。

今ではユーザーエージェントスタイルシートでは`:focus`ではなく`:focus-visible`が使われています。
これにより、特に何も指定せずとも「マウスではフォーカスリングが表示されず、キーボードでは表示される」状態になっています。

## リリース情報

Chrome

https://chromestatus.com/feature/5658873031557120

Firefox

https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Releases/85#css

Safari

https://webkit.org/blog/12445/new-webkit-features-in-safari-15-4/#css

## 最後に

前の記事を書いたのが2021年4月18日で、Chromeで`:focus-visible`がデフォルトになったのが2021年4月14日のようでした。
タイミングが悪過ぎます。

Safariが対応したのが2022年3月14日でバージョンが15.4なので、まだ`:focus-visible`が完全にデフォルトとも言い切れないのですが、割と時間の問題かと思っています。

いずれにせよ、ブラウザのデフォルトの挙動に任せればアクセシビリティも高く、見た目も良いのは嬉しいです。

---

最後まで読んでくださってありがとうございます！
Twitterでも情報を発信しているので、良かったらフォローお願いします！

https://twitter.com/xrxoxcxox

Devトークでのお話してくださる方も募集中です！

https://jobs.qiita.com/employers/qiita-inc/dev_talks/489