<!--
title:   デザイナーが1人でGatsbyを使ってサイトを作ったよ #2
tags:    Design,React,emotion,figma,gatsby
id:      c27b92e763f3833af6dd
private: false
-->
## この記事について

### 内容を一言でまとめると

- [前回の記事](2019-08-26_Design_React_emotion_figma_gatsby_ac5937fb0e54a875787a.md)の続きで、具体的なコンテンツを作成するまでの流れ

### こんな人が読んでくれたら嬉しい

- ポートフォリオサイトを作成しようと思っている方
- かつ、制作物のページ更新は出来るだけ簡単に済ませたい方

## 作ったサイト

- https://www.keisukewatanuki.work/

## 運用の方法

1. 所定のフォルダに、任意の名前のフォルダを作成する
2. そのフォルダ内にMarkdownファイルを保存する
3. フォルダ名をslugとしたページが作成される

## 作成の手順

### プラグインを導入する

| プラグイン名 | 説明 |
|:--|:--|
| [gatsby-transformer-remark](https://www.gatsbyjs.com/plugins/gatsby-transformer-remark/) | MarkdownファイルをHTMLに変換してくれるプラグイン。 |
| [gatsby-remark-images](https://www.gatsbyjs.com/plugins/gatsby-remark-images/) | Markdown内で使った画像も普段のGatbsyのように最適化を施した上でビルド出来るようにしてくれるプラグイン。 |

```shell:shell
yarn add gatsby-source-filesystem gatsby-transformer-remark gatsby-remark-images gatsby-plugin-sharp
```

今回の記事において重要なのは`gatsby-transformer-remark`と`gatsby-remark-images`です。
Gatsbyを使っているなら`gatsby-source-filesystem`と`gatsby-plugin-sharp`は導入済の場合も多いと思いましたが、念のため記載。

```javascript:gastby-config.js
plugins: [
  {
    resolve: `gatsby-source-filesystem`,
    options: {
      name: `markdown-pages`,
      path: `${__dirname}/src/markdown-pages`,
    },
  },
  {
    resolve: `gatsby-transformer-remark`,
    options: {
      plugins: [
        {
          resolve: `gatsby-remark-images`,
          options: {
            maxWidth: 680,
            linkImagesToOriginal: false,
            quality: 80,
            withWebp: true,
          },
        },
      ],
    },
  },
]
```

`resolve: gatsby-source-filesystem`の部分は、Markdownファイル専用のフォルダを作っておいた方が便利なので作成し、filesystemで扱えるようにここで定義しています。

`resolve: gatsby-transformer-remark`は、オプションとして`gatsby-remark-images`を使うので入れ子になっています。
自分は画像をクリックした際にオリジナルへリンクしてもしょうがないなと思ったのでfalseを選んだり、webpが使えるようにしたかったのでtrueを選択したりしています。

### テンプレートを作成する

```javascript:src/templates/template.js
import React from 'react'
import { graphql } from 'gatsby'
import Img from 'gatsby-image'

// 本来このあたりにスタイリングのためのコードがあるものの、省略

export default function Template({ data }) {
  const post = data.markdownRemark
  const featuredImageFluid = post.frontmatter.featuredImage.childImageSharp.fluid
  return (
    <>
      // 本来レイアウトのためのコンポーネントやコードがあるものの、省略
      <Img fluid={featuredImageFluid} alt='' />
      <h1>{post.frontmatter.title}</h1>
      <article dangerouslySetInnerHTML={{ __html: post.html }} />
    </>
  )
}

export const pageQuery = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
        featuredImage {
          childImageSharp {
            fluid(maxWidth: 800) {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`
```

GraphQLでhtmlやfeaturedImageの情報を取得して、returnの中であれこれ展開します。
今回で一番大切になるのは`dangerouslySetInnerHTML={{ __html: post.html }}`の部分。
ここにMarkdown内のコンテンツが注入されます。

### `createPage` APIを利用してページを生成する

```javascript:gatsby-node.js
const path = require(`path`)
exports.createPages = async ({ actions, graphql }) => {
  const { createPage, createRedirect } = actions
  const template = path.resolve(`src/templates/template.js`)
  const result = await graphql(`
    {
      allMarkdownRemark(
        sort: { order: DESC, fields: [frontmatter___date] }
        limit: 1000
      ) {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)
  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: template,
      context: { slug: node.fields.slug },
    })
  })
}

const { createFilePath } = require(`gatsby-source-filesystem`)
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `markdown-pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}
```

[このページ](https://www.gatsbyjs.com/docs/adding-markdown-pages/)で紹介されているやり方だとMarkdown内に毎度slugのために何かしら指定することになり……面倒でした。
というわけで`createNodeField`という関数でもって、`src/markdown-pages`の中にあるフォルダ名をslugに出来るようにしています。

あとは`result.data.allMarkdownRemark.edges.forEach(({ node })`の中でMarkdownからページを作成する運びです。

### Markdownを保存する

ここまで来たら`src/markdown-pages`内にフォルダを作ってMarkdownファイルを保存すればどんどんページが作成されます。
もちろんリリースした後でもMarkdownを編集すればサイトそのものを更新できます。

## まとめ

せっかくサイトを作っても更新が面倒だと滞りがちです。そうなってしまっては勿体ないですよね。
出来るだけ簡単に更新出来るようにして、日々触れるサイトになると良いよねと思います。
ちなみに自分の制作物のページ更新は若干滞っています……。頑張らないとな……。