---
title: Liquidテンプレートの使い方
description: Shopifyが開発しているテンプレート言語
categories: [BLOG]
image: /assets/img/2025-10-13-shopifyliquid/liquid.png
tags: [blog, markdown, liquid]     # TAG names should always be lowercase
mermaid: true
---
<style type="text/css">
/* 強調表示を蛍光ペン風に */
article strong{
margin:0 0.1em;
padding:0.1em 0.2em;
background:#fcfc60 !important;
background:linear-gradient(to bottom, transparent 60%, rgb(88, 194, 183) 60%) !important;
}
/* bタグは太字 */
article b{
font-weight:bold !important;
}
</style>


<br>
<!-- no robot! -->
<meta name="robots" content="noindex">



## ‍️ Shopify Liquidとは
Shopifyが開発しているRubベースのテンプレート言語。<br><br>

### 使い方 基本の3つ
####  **Objects**
波括弧二つで囲む \{\{\}\}  
格納された変数を展開するのに使う。

```
\{\{ page.title \}\}  
↓  
{{ page.title }}
```

<br>
<br>

#### **Tags**
波括弧の中に％を入れて囲む  \{\% page.title \%\}  
イテレーション回したり、変数のアサインとかに使えるらしい。

```
\{\% if user \%\}
  Hello \{\{ user.name \}\}!
\{\% endif \%\}
```
<br>
<br>

#### **Filters**
波括弧の中で｜を使う  \{\{ "myhome/town/here" | append: ".html" \}\}  
変数のフィルターしたりする。複数のフィルターを適用可能。

```
\{\{ "myhome/town/here" | append: ".html" \}\}
↓
{{ "myhome/town/here" | append: ".html" }}
```
<br>
<br>

### 基礎知識
#### オペレータ
  
|オペレータ|意味|
|---|---|
|==	|equals|
|!=	|does not equal|
|>	|greater than|
|<	|less than|
|>=	|greater than or equal to|
|<=	|less than or equal to|
|or	|logical or|
|and	|logical and|
|contains	|checks for the presence of a substring inside a string|

<br>

#### 型

|型|意味|
|---|---|
|String	|文字列|
|Number|数字型|
|Boolean|真理値|
|Nil|NULL|
|Array|配列|

>  Jekyllのテーマで使ってるLiquidは最新版ではない可能性あり
{: .prompt-warning }

<br>

### Tagで使えるコンディション

|コントロールフロー|
|---|
|if|
|unless|
|else/if|
|case/when|

<br>

### Tagで使えるイテレーション

|Iteration|
|---|
|for|
|else|
|break|
|continue|
|limit|
|offset|
|range|
|reversed|
|forloop|
|cycle|
|tablerow|
|tablerowloop|

<br>

### Tagで使えるテンプレート

|Template|
|---|
|comment|
|raw|
|echo|
|render|

### Tagで使える変数

|Variable|
|---|
|assign|
|capture|
|increment|



<br>
<br>

### 📚 参考
#### 公式リファレンス
- [Liquid公式ブログ](https://shopify.github.io/liquid/)

