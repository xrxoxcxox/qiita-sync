<!--
title:   ベクターグラフィックソフトから書き出されるSVGのコードを比較してみた
tags:    AffinityDesigner,XD,figma,illustrator,sketch
id:      6f54e765e3f801dc5cc4
private: false
-->
## これは何

- SVGはベクター形式の画像を表現するためのマークアップ言語です
- 実際は1からコードを書いてグラフィックを作ることはなく、ベクターグラフィックのソフトから出力したものをそのまま使うことがほとんどです
- そんなSVG、ソフトによって書き出されるコードに違いはあるのかを調べてみました

## 検証ソフト一覧

| ソフト名 | バージョン |
| --- | --- |
| Illustrator | 25.3.1 |
| XD | 42.0.22.11 |
| Sketch | 74.1 |
| Figma | 99.0 |
| Affinity Designer | 1.9.3 |

この検証のためだけに一度全てのソフトをアップデートしたので、2021/8/1現在での最新バージョン同士の比較のはずです。

## 比較する画像

こんな画像をそれぞれのソフトで作成してみます。

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/214677/9c0cb795-f088-7b5c-8f3f-1ff5dfc7b7ad.png)

Qiitaにアップする都合上PNGに変換していますが、まぎれもなくベクターで作成しています。
四角、円、多角形、線と、SVGの基本的な要素をひとしきり確認できるように作成しました。

## Illustrator

Illustratorは別名で保存したときとスクリーン用に書き出ししたときで結構コードが変わっていたので両方を掲載します。

### aiファイルを別名で保存して生成されたコード

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- Generator: Adobe Illustrator 25.3.1, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
<svg version="1.1" id="レイヤー_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px"
	 y="0px" viewBox="0 0 200 200" style="enable-background:new 0 0 200 200;" xml:space="preserve">
<style type="text/css">
	.st0{fill:#FF0000;}
	.st1{fill:#00FF00;}
	.st2{fill:#0000FF;}
	.st3{fill:none;stroke:#000000;stroke-width:2;stroke-miterlimit:10;}
	.st4{fill:none;stroke:#CCCCCC;stroke-miterlimit:10;}
</style>
<g>
	<rect class="st0" width="100" height="100"/>
</g>
<circle class="st1" cx="125" cy="125" r="75"/>
<g>
	<polygon class="st2" points="166,155 80,155 123,80 	"/>
</g>
<line class="st3" x1="20" y1="200" x2="20" y2="80"/>
<path class="st4" d="M30,50c27.6,0,50,22.4,50,50s-22.4,50-50,50"/>
</svg>

```

- xml宣言がされている
- `Illustratorによって生成されたファイルである`という旨のコメントが入っている
- `xmlns`だけでなく`xmlns:xlink`の指定もされている
    - これがあると無いとでどう違うのか正直分かっていません
    - どなたかご存じの方がいたら是非教えてください
- レイヤー名がidとして指定されている
- 色や線の太さなどが`style`の中でまとめて宣言されている
- `path`での座標指定が相対指定

### スクリーン用に書き出し

```xml
<svg id="レイヤー_1" data-name="レイヤー 1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 200"><defs><style>.cls-1{fill:red;}.cls-2{fill:lime;}.cls-3{fill:blue;}.cls-4,.cls-5{fill:none;stroke-miterlimit:10;}.cls-4{stroke:#000;stroke-width:2px;}.cls-5{stroke:#ccc;}</style></defs><rect class="cls-1" width="100" height="100"/><circle class="cls-2" cx="125" cy="125" r="75"/><polygon class="cls-3" points="166 155 80 155 123 80 166 155"/><line class="cls-4" x1="20" y1="200" x2="20" y2="80"/><path class="cls-5" d="M30,50a50,50,0,0,1,0,100"/></svg>
```

- minifyされている
    - しかしその割にはレイヤー名がidとして指定されている
- 半円のパスが`c`ではなく`a`を使っている
    - `c`がベジェ曲線の指定
    - `a`は円弧のときだけ使える指定
    - 別名保存時は`c`が使われていた

## XD

XDもexport時設定でminifyするかしないかを選べたので両方試してみました

### minifyなし

```xml
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="200" height="200" viewBox="0 0 200 200">
  <defs>
    <clipPath id="clip-Artboard">
      <rect width="200" height="200"/>
    </clipPath>
  </defs>
  <g id="Artboard" clip-path="url(#clip-Artboard)">
    <rect width="200" height="200" fill="#fff"/>
    <rect id="Rectangle_1" data-name="Rectangle 1" width="100" height="100" fill="red"/>
    <circle id="Ellipse_1" data-name="Ellipse 1" cx="75" cy="75" r="75" transform="translate(50 50)" fill="lime"/>
    <path id="Polygon_1" data-name="Polygon 1" d="M43,0,86,75H0Z" transform="translate(80 80)" fill="blue"/>
    <line id="Line_1" data-name="Line 1" y2="120" transform="translate(20 80)" fill="none" stroke="#000" stroke-width="2"/>
    <path id="Path_1" data-name="Path 1" d="M30,50a50,50,0,0,1,0,100" fill="none" stroke="#ccc" stroke-miterlimit="10" stroke-width="1"/>
  </g>
</svg>

```

- clipPathが指定されている
    - width, height, viewBoxあたりで幅と高さを指定している以上、あってもなくても描画は変わらない気がするものの、何か意図がある？
- アートボード自体の色（`#fff`）を出力している
    - 他のソフトではアートボードごと書き出してもアートボードの色は無視していた
- 全ての要素に`id`と`data-name`が付与されている
    - `id`はともかく`data-name`はなぜ必要なのか
- 座標指定 + transformで位置を制御している
    - 直接描画に影響することはないと思うものの、なんだか怖い指定の仕方ではある

### minifyあり

```xml
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="200" height="200" viewBox="0 0 200 200"><defs><clipPath id="b"><rect width="200" height="200"/></clipPath></defs><g id="a" clip-path="url(#b)"><rect width="200" height="200" fill="#fff"/><rect width="100" height="100" fill="red"/><circle cx="75" cy="75" r="75" transform="translate(50 50)" fill="lime"/><path d="M43,0,86,75H0Z" transform="translate(80 80)" fill="blue"/><line y2="120" transform="translate(20 80)" fill="none" stroke="#000" stroke-width="2"/><path d="M30,50a50,50,0,0,1,0,100" fill="none" stroke="#ccc" stroke-miterlimit="10" stroke-width="1"/></g></svg>
```

- minifyされている
    - その割に相変わらず`clipPath`や`transform`が使われている
- idが`a`とか`b`とかに置き換えられている
    - サンプルで作った画像がかなり簡易なものなので、このまま`c`, `d`...と増えていくのか、`z`以降はどうなるのかなどは謎

## Sketch

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg width="200px" height="200px" viewBox="0 0 200 200" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
    <title>Artboard</title>
    <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
        <g id="Artboard">
            <rect id="Rectangle" fill="#FF0000" x="0" y="0" width="100" height="100"></rect>
            <circle id="Oval" fill="#00FF00" cx="125" cy="125" r="75"></circle>
            <polygon id="Triangle" fill="#0000FF" points="123 80 166 155 80 155"></polygon>
            <line x1="20" y1="80" x2="20" y2="200" id="Line" stroke="#000000" stroke-width="2" stroke-linecap="square"></line>
            <path d="M30,150 C57.6142375,150 80,127.614237 80,100 C80,72.3857625 57.6142375,50 30,50" id="Path" stroke="#CCCCCC"></path>
        </g>
    </g>
</svg>
```

- xml宣言がある
- `xmlns`だけでなく`xmlns:xlink`の指定もされている
- `title`が指定されている
    - アートボード名がtitleになっている様子
- ページとアートボード、両方の単位で`g`が生成されている

## Figma

```xml
<svg width="200" height="200" viewBox="0 0 200 200" fill="none" xmlns="http://www.w3.org/2000/svg">
<rect width="100" height="100" fill="#FF0000"/>
<circle cx="125" cy="125" r="75" fill="#00FF00"/>
<path d="M123 80L166 155H80L123 80Z" fill="#0000FF"/>
<line x1="21" y1="80" x2="21" y2="200" stroke="black" stroke-width="2"/>
<path d="M30 150C57.6142 150 80 127.614 80 100C80 72.3858 57.6142 50 30 50" stroke="#CCCCCC"/>
</svg>

```

- インデントがない
- `g`でまとまっていない
- 多角形を`polygon`でなく`path`で描画している
- かなりさっぱりしていて、個人的には1番好きなコード

## Affinity Designer

エクスポート設定が4種類あったので、それぞれを比較してみました。

### for export

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg width="100%" height="100%" viewBox="0 0 200 200" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve" xmlns:serif="http://www.serif.com/" style="fill-rule:evenodd;clip-rule:evenodd;stroke-linejoin:round;stroke-miterlimit:1.5;">
    <rect id="Artboard" x="0" y="0" width="200" height="200" style="fill:none;"/>
    <clipPath id="_clip1">
        <rect id="Artboard1" serif:id="Artboard" x="0" y="0" width="200" height="200"/>
    </clipPath>
    <g clip-path="url(#_clip1)">
        <g transform="matrix(1.13106,0,0,1.13106,-42.9822,-36.1368)">
            <rect x="38.002" y="31.95" width="88.413" height="88.413" style="fill:rgb(255,0,0);"/>
        </g>
        <g transform="matrix(1.65719,0,0,1.65719,-55.9655,-45.3328)">
            <circle cx="109.2" cy="102.784" r="45.257" style="fill:rgb(0,255,0);"/>
        </g>
        <g transform="matrix(1.60005,0,0,1.39539,-43.8979,-59.5392)">
            <path d="M104.308,100L131.182,153.748L77.434,153.748L104.308,100Z" style="fill:rgb(0,0,255);"/>
        </g>
        <g transform="matrix(1,0,0,1.07417,-3.46616,-14.8334)">
            <path d="M23.466,88.286L23.466,200" style="fill:none;stroke:black;stroke-width:1.93px;"/>
        </g>
        <g transform="matrix(1.10479,0,0,1.10479,-90.6436,-13.5552)">
            <path d="M109.2,57.527C134.178,57.527 154.457,77.806 154.457,102.784C154.457,127.762 134.178,148.041 109.2,148.041" style="fill:none;stroke:rgb(204,204,204);stroke-width:0.91px;stroke-linecap:round;"/>
        </g>
    </g>
</svg>

```

- 非推奨である`DOCTYPE`宣言がされている
- 各要素が全て`g`で括られている
- 幅や高さ、座標指定がなんだかおかしなことになっている
    - 要素を括っている`g`に`transform`がついていて、それで調整している
    - 少し怖い設定である

### small size

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?><!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd"><svg width="100%" height="100%" viewBox="0 0 200 200" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve" xmlns:serif="http://www.serif.com/" style="fill-rule:evenodd;clip-rule:evenodd;stroke-linejoin:round;stroke-miterlimit:1.5;"><rect id="Artboard" x="0" y="0" width="200" height="200" style="fill:none;"/><clipPath id="_clip1"><rect id="Artboard1" serif:id="Artboard" x="0" y="0" width="200" height="200"/></clipPath><g clip-path="url(#_clip1)"><rect x="0" y="0" width="100" height="100" style="fill:#f00;"/><circle cx="125" cy="125" r="75" style="fill:#0f0;"/><path d="M123,80l43,75l-86,-0l43,-75Z" style="fill:#00f;"/><path d="M20,80l0,120" style="fill:none;stroke:#000;stroke-width:2px;"/><path d="M30,50c27.596,0 50,22.404 50,50c0,27.596 -22.404,50 -50,50" style="fill:none;stroke:#ccc;stroke-width:1px;stroke-linecap:round;"/></g></svg>
```

- 非推奨である`DOCTYPE`宣言がされている
- 各要素を囲う`g`要素が消えている
    - おかげで（？）座標やサイズも自然になっている
- 多角形を`polygon`でなく`path`で描画している

### high quality

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?><!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd"><svg width="100%" height="100%" viewBox="0 0 834 834" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve" xmlns:serif="http://www.serif.com/" style="fill-rule:evenodd;clip-rule:evenodd;stroke-linejoin:round;stroke-miterlimit:1.5;"><rect id="Artboard" x="0" y="0" width="833.333" height="833.333" style="fill:none;"/><clipPath id="_clip1"><rect id="Artboard1" serif:id="Artboard" x="0" y="0" width="833.333" height="833.333"/></clipPath><g clip-path="url(#_clip1)"><rect x="0" y="0" width="416.667" height="416.667" style="fill:#f00;"/><circle cx="520.833" cy="520.833" r="312.5" style="fill:#0f0;"/><path d="M512.5,333.333l179.167,312.5l-358.334,0l179.167,-312.5Z" style="fill:#00f;"/><path d="M83.333,333.333l0,500" style="fill:none;stroke:#000;stroke-width:8.33px;"/><path d="M125,208.333c114.982,0 208.333,93.351 208.333,208.334c0,114.982 -93.351,208.333 -208.333,208.333" style="fill:none;stroke:#ccc;stroke-width:4.17px;stroke-linecap:round;"/></g></svg>
```

- 非推奨である`DOCTYPE`宣言がされている
- どういうわけか、`viewBox`の値が`834`になっている
    - もともとは`200`で作っていて、他のデータは全て`200`のまま
    - すべてのサイズが約4.17倍になっているもよう
    - 理由がよく分からないので、分かる方がいたら是非教えてください

### flatten

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg width="100%" height="100%" viewBox="0 0 200 200" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve" xmlns:serif="http://www.serif.com/" style="fill-rule:evenodd;clip-rule:evenodd;stroke-linejoin:round;stroke-miterlimit:1.5;">
    <use xlink:href="#_Image1" x="0" y="0" width="200px" height="200px"/>
    <defs>
        <image id="_Image1" width="200px" height="200px" xlink:href="data:image/png;base64,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"/>
    </defs>
</svg>

```

- 非推奨である`DOCTYPE`宣言がされている
- base64に変換されている
    - 初めて見た形式
- `use`が使用されている

## まとめ

- ちょっとした内容の書き出しでも、ソフトによって結構差がある
- 微妙に意図しない表示になったり、拡大したらわずかな隙間が空くんじゃ無いか……？と思えるものもあった
    - `transform`が多用されているデータが怪しく感じている
- 1番すっきりしているのはFigma
- 1番内容が多いのはAffinity Designerのflatten
    - なんと今回の画像で容量が8KB