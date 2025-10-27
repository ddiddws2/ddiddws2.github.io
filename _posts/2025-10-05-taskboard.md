---
title: Task Board
description: 今後やりたいことのタスクボード
categories: [Blog,Task]
image: /assets/img/2025-10-20-taskboard/kanban.png
tags: [task,blog]     # TAG names should always be lowercase
mermaid: true
---

<br>
<!-- no robot! -->
<meta name="robots" content="noindex">

```mermaid
%%{init: {'theme':'forest'}}%%
kanban
  column1[TODO]
    task2[Mouseの無線接続の仕組み勉強]
    task3[Djangoでファイルアップロードできる仕組み作り]
    task5[Githubのgithub actionsの仕組みを勉強]
  column2[DOING]
    task1[SQLチートシートの作成]
    task4[思考法図鑑を使って思考法の勉強]
  column3[DONE]
    task4[Liquidテンプレートの勉強]
```