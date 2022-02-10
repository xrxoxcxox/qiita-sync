<!--
title:   tsconfigのincludeやexcludeの初期設定を理解しておらず必要以上にコードを書いていた話
tags:    TypeScript,tsconfig.json
id:      f6166eabc9b44736760b
private: false
-->
## この記事の概要

見よう見まねで`tsconfig.json`の設定をしていたので今まで何回か無駄なコードを書いてしまっていました。
これから新しく`tsconfig.json`を作成する人が少しでも少ないコード量で設定を終えられる助けになれば良いな、と思って書いた記事です。

## include編

### 無駄に書いていたコード

だいたいいつもこんな感じで書いていました。

```json:tsconfig.json
{
  "include": ["src/**/*.ts", "src/**/*.tsx"]
}
```

### これで良かった

拡張子を指定しないで、ひとまとめ。

```json:tsconfig.json
{
  "include": ["src/**/*"]
}
```

### 説明

デフォルトでは`.ts`と`.tsx`と`.d.ts`のみをサポートしています。

`allowJS`を指定すると`.js`と`.jsx`もincludeされますが、いつも`false`にしていたにも関わらず`.ts`や`.tsx`を記載していました。
明示的になるので悪いことも無いと思いますが、知っててやるのと知らないでやるのには大きな隔たりがあるよな……と感じています。

https://www.typescriptlang.org/tsconfig#include

## exclude編

### 無駄に書いていたコード

プロジェクトによって多少違いますが、だいたいこんな感じでした。

```jsonc:tsconfig.json
{
  "compilerOptions": {
    "outDir": "dist",
    // その他色々な設定
  },
  "exclude": ["dist", "node_modules"]
}
```

### これで良かった

記載そのものが不要。

```jsonc:tsconfig.json
{
  "compilerOptions": {
    "outDir": "./dist",
    // その他色々な設定
  },
}
```

### 説明

デフォルトでexcludeされるのは`node_modules`, `bower_components`, `jspm_packages`, そして`outDir`です。

そのため例に挙げたような設定であればまさにデフォルトそのまま。
記載する必要がありません。

こちらも明示的になるので悪いとは思いませんが、デフォルトを知っていればあえて指定することもないのかな？と思います。

https://www.typescriptlang.org/tsconfig#exclude