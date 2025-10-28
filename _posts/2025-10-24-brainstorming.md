---
title: 思考の参考
description: 『思考法図鑑』を読んで、学んだこと
categories: [BLOG]
image: /assets/img/2025-10-24-brainstorming/brain1.png
tags: [blog,brainstorming,critical thinking]     # TAG names should always be lowercase
mermaid: true
---

<br>
<!-- no robot! -->
<meta name="robots" content="noindex">


<!-- border setting -->
<style>
hr {
  padding: 20px 0;
  overflow: visible;
}
.hr1 {
  border-top: 1px dashed #aaa;
}
.hr1::after {
  display: inline-block;
  position: relative;
  top: -35px;
  left: 50px;
  padding: 0 3px;
  background: #eeeeee;
  color: #aaa;
  font-size: 15px;
} 
</style>

### 思考法図鑑の活用レベル
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
LEVEL1[自分の思考の型を意識する] --> LEVEL2[自分の思考を拡張する] --> LEVEL3[自分の考え方を自論化する] 
```
<br>
<hr class="hr1">

## 思考法
### Logical Thinking
結論と根拠のつながりを明確にし、客観的かつ合理的に考えるための思考法。

`考え方`
1. 論点を決める
2. 情報を集める
3. 何が言えるかを考える
4. 論理を構造化する

>
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[論点]
A <--> B[結論]
B <--> C[根拠]
B <--> D[根拠]
B <--> E[根拠]
```
{: .prompt-tip }

<br>
<br>
<hr class="hr1">
<br>

### Critical Thinking
前提は正しいのか、結論と前提は本当に繋がっているのかなど、<br>
客観的かつ批判的な目線でする思考法。

`考え方`
1. 論理を展開する
2. 論点を疑う
3. 結論と根拠のつながりを疑う
4. 前提を疑う

>
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[論点] -- 論点は正しいのか <br> 結論は論点に対応しているのか
 　<--> B[結論]
  B -- 正しい根拠なのか 
    <--> C[根拠]
  B -- 根拠となる情報は正しいのか
    <--> D[根拠]
    B <--> E[根拠]
```
{: .prompt-tip }

<br>
<br>
<hr class="hr1">
<br>


### Deductive Reasoning(演繹法/えんえきほう)
常識的な情報から小前提を導き出してそこから結論にむすびつける考え方。

`考え方`
1. 大前提を把握する
2. 小前提を把握する
3. 結論を導く

>
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[大前提]
  A -- 大前提から導かれる小前提 
 　--> B[小前提]
  B -- 小前提から導かれる結論 
    --> C[結論]
  A --> C
```
{: .prompt-tip }

`補足` **推論とは**  
推論とは、既知の情報から未知の結論を導き出す、思考過程のこと。

<br>
<br>
<hr class="hr1">
<br>


### Inductive Reasoning(帰納法/きのうほう)
物事の共通項から一般的な結論を導く思考法。

`考え方`
1. サンプルを集める
2. 一般化して結論を導き出す

>
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[事象A] 
B[事象B]
C[事象C]
D[結論]
A --> D
B --> D
C --> D
```
{: .prompt-tip }


<br>
<br>
<hr class="hr1">
<br>


### Abductive Reasoning(仮説推論)
事実から仮説を考察する思考法。

`考え方`
1. 事実を確認する
2. 説明仮説を立てる
3. 説明仮説を検証する

>
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[アブダクション <br>仮説の検証] 
B[演繹<br>仮説の具体化]
C[帰納<br>仮説の検証]
A --> B
B --> C
C -- 仮説の強化 
  --> A
```
{: .prompt-tip }


