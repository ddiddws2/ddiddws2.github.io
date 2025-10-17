---
title: Rainy75でマウスをスムーズに動かすために悪戦苦闘
description: QMKでファームウェアを構築していく
categories: [KEYBOARD,QMK,VIA]
image: /assets/img/2025-10-17-via-qmk/via_qmk.png
tags: [rainy75, via, qmk,keyboard]     # TAG names should always be lowercase
mermaid: true
---

<br>

### ‍️⌨️ Rainy75は厳密にはQMK対応していない。
QMK/Via対応を謳うにはライセンスの規約上、改変したコードはパブリックに公開する必要があるが、
githubレポジトリへの登録がないため、ライセンス違反になっている。(後継のCrush80,Zen65も同様)
<br>
<br>

[githubから一部抜粋](https://docs.qmk.fm/license_violations#offending-vendors)
<br>

|Vendor|Reason|
|---|---|
|WOBKEY|Selling tri-mode boards based on QMK without sources, <br> attempted upstreaming crippled firmware without wireless.| 

<br>

#### 方針
>**やりたいこと**<br>
Rainy75でマウスポインターをスムーズに動かして、マウスレスで作業したい。
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[Rainy75のマウスキー設定] --> B[QMK/Via で設定];
B --> C[Via で設定];
B --> D[QMK で設定];
C --  マウスキーのスムーズさの設定は直接はできない。--> F{❌};
D --  WOBKEYのファームウェアをゲットする --> E[公式サイトには、exeのものしかない。];
E --  類似レイアウトのキーボードを使ってやってみる --> G[採用];
```
{: .prompt-info }

<br>
<hr size="3" color="#e2e2e2">
     
        
#### QMK設定方針
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[QMKでファームウェア作成] --> B[1.QMKの作業環境をインストール];
B --> C[2.サンプルを使ってファームウェアを作成];
C --> D[3.ファームウェアをRainy75に焼き付ける];
```

<br>
<hr size="7" color="#e2e2e2">
     
#### QMK作業詳細
##### 1.QMKの作業環境をインストール 
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[1.QMKの作業環境をインストール] --> B[QMKのインストール];
B -- brew install qmk/qmk/qmk --> C[QMKのセットアップ];
C -- qmk setup　--> D[テストしてみる]
D -- qmk compile -kb blaster75 -km default --> E[生成される内容を確認]
F@{ shape: comment, label: "brewは事前にインストールしておく" }
```




### 📚 参考
#### Github
- [QMK](https://github.com/qmk)
- [Via](https://github.com/the-via)
