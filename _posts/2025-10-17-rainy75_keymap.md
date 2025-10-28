---
title: Rainy75でマウスをスムーズに動かすために悪戦苦闘
description: QMKでファームウェアを構築していく
categories: [Keyboard,QMK,VIA]
image: /assets/img/2025-10-17-via-qmk/via_qmk.png
tags: [rainy75, via, qmk,keyboard]     # TAG names should always be lowercase
mermaid: true
---

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



## 結論　 
Tri-modeスイッチのソースコードがないので、ソースコードが手に入るか、<br>
実装方法がわかるまではやらない。有線接続以外使えなくなる可能性があるので。
Bluetoothとか2.4GhzのUSBドングルの仕様はQMKで実装してる例が見つからない。
もしかしたら、 KeyChronとかは独自に実装してるのかもしれないけど、
実装するとしたら、ZMKとか別のファームウェアを使って実装してる例が多い。
なので、中華系のベンダーは複数のファームウェアを組み合わせてるまたは、
独自実装してるのかも。Via実装してるとウケがいいってことで、Via用のJsonだけ
提供して、内部実装は秘匿しとくと。。ま、一般ユーザー的にはそこまで深く触らなくても満足できる
⌨️になってると思う。

<br><br>


#### 方針決め
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
<hr class="hr1">
     
        
#### QMK設定方針
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[QMKでファームウェア作成] --> B[1.QMKの作業環境をインストール];
B --> C[2.サンプルを使ってファームウェアを作成];
C --> D[3.ファームウェアをRainy75に焼き付ける];
```

<br>
<hr class="hr1">
     
### QMK作業詳細
#### 1.QMKの作業環境をインストール 
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[1.QMKの作業環境をインストール] --> B[QMKのインストール];
B -- brew install qmk/qmk/qmk --> C[QMKのセットアップ];
C -- qmk setup　--> D[テストしてみる]
D -- qmk compile -kb blaster75 -km default --> E[生成される内容を確認]
```

>️用意するファイル<br>
- **keyboard.json**     
QMK用のキーボードjsonファイル<br><br>
- **keymap.c**          
QMK用キーボードマッピングファイル<br><br>
- **via.json**    
via用キーボードマッピングファイル<br><br>
- **rules.mk**   
VIA_ENABLE = yes と書いただけのファイル<br><br>
- **config.h**  
QMK用マウススピードの調整とかする設定ファイル
{: .prompt-warning }

<br>
<hr class="hr1">
<br>

#### 2.サンプルを使ってファームウェアを作成 
```mermaid
%%{init: {'theme':'neutral'}}%%
flowchart TB;
A[2.サンプルを使ってファームウェアを作成] --> B[デフォルトキーボードの設定];
B -- qmk config user.keyboard=blaster75 --> C[キーマップの名前をつける]
C -- qmk config user.keymap=username --> D[キーマップ作成]
D -- qmk new-keymap -kb keyboard_name --> E[keymap.cを編集]
```
ここで作業断念！<br>
必要なファイルを揃えて、あとはマイコンにファームウェア焼き付ければOKくらいの段階だった。
ふと、キーマップの配置中に疑問が出た。BluetoothとかUSBデバイスでの通信とか有線以外に切り替える時の
キーはカスタムされてるっぽい。これって具体的に、QMKのconfig.hなりにどう書かれてるんだ？というか
QMK外での実装もされてるかも、だからあえて、ファームウェアの更新とかは、exeファイルだけよこして
内部実装を秘匿しているのか？ 実装方法が分からない箇所を放置してファームウェアを焼き付けちゃうと、
機能しなくなりそうだ。ということで、一旦作業中止。

<br>
<hr class="hr1">

## ‍️補足　
### ⌨️ Rainy75は厳密にはQMK対応していない。
QMK/Via対応を謳うにはライセンスの規約上、改変したコードはパブリックに公開する必要があるが、
githubレポジトリへの登録がないため、ライセンス違反になっている。(後継のCrush80,Zen65も同様)<br>
via用のJSONファイルは提供されているし、ファームウェアの更新が必要な場合は、<br>
exeファイルも公式サイトに置いてあるので、viaをいじって作業する限りは何の問題もない。
<br>
<br>

[githubから一部抜粋](https://docs.qmk.fm/license_violations#offending-vendors)
<br>

|Vendor|Reason|
|---|---|
|WOBKEY|Selling tri-mode boards based on QMK without sources, <br> attempted upstreaming crippled firmware without wireless.| 

<br>

### 📚 参考
#### Github
- [QMK](https://github.com/qmk)
- [Via](https://github.com/the-via)

#### Blog
- [VIAのマクロでマウスキーを使う方法](https://pmortensen.eu/world2/2024/02/26/a-hack-to-use-mouse-actions-in-via-macros/)
- [QMKマクロ応用編](https://getreuer.info/posts/keyboards/macros/index.html)
- [自作キーボードのVIA対応方法](https://note.com/sam1dare/n/n816ce95fb2f2)

#### Others
- [QMK Configurator](https://config.qmk.fm/#/atreus/f103/LAYOUT_pcb_up)
- [QMK Keycodes](https://docs.qmk.fm/keycodes)