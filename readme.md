# Minecraft Education Edition
# C言語（c1-byod） + Pythonによるプレイヤ自動操作セット

**本プログラムはC演習で利用したc1-byod環境で動作します．**

最終更新日 2023/1/8

---

## ◇MineCraftの初期設定

**※必ずお読みください．プログラムが正常に動作しません．**  

本プログラムはPythonのDirectInputを利用しカメラを操作します．そのためフルキーボードゲームプレイを`ON`に設定する必要があります．ゲーム画面から

`【Esc → 設定 → キーボード&マウス → フルキーボードゲームプレイをONにする】`

※（MINECRAFTと表示されている）タイトルメニューにある「設定」ボタンからも設定します．

プログラムを起動する際は，上記の方法で視点操作の感度を予めご確認ください．またカメラの速度はスムース回転スピードを設定することで調整することができます．設定値は5程度がよいかと思いますが作成するプログラムによって調整してください．

また，ディスプレイは1980x1080のディスプレイを要し，テキスト，アプリ，その他の項目のサイズを100%にしておく必要があります．

`【デスクトップ右クリック → ディスプレイ設定 → テキスト，アプリ，その他の項目のサイズを変更する → 100%】`

---

## ◇Botプログラムのダウンロード方法，実行・停止方法

### ダウンロード方法

c1-byod上で以下のコマンドを実行するとダウンロードできる．フォルダは好きなところで良いが，ここでは「```~/```」に展開するとする．

``` sh
cd ~/
git clone https://github.com/masaki555/Minecraft_Contest.git
chmod -R 755 Minecraft_Contest
cd Minecraft_Contest/
./setup.sh
chmod -R 755 python
```

上記のコマンドを実行すると```~/Minecraft_Contest/```というディレクトリが作成され，このディレクトリの中でプログラムをCプログラムを作成する事となる．また，```~/Minecraft_Contest/```の中にはtestbot.cというプログラムが置かれており，こちらを参考にプログラムを書いて下さい．ただし，かなり初心者が書いた出来の悪いサンプルプログラムですので，皆さんはこれを賢いBotプログラムに修正するようにしてください．また，以下にMinecraft_Contestフォルダのフォルダ構造を記載しておきます．

``` file
  📁Minecraft_Contest        // 作成したMinecraft_Contestフォルダ
   ┗ 📁python        // Botを動かすために必要なpythonファイル関係
     📃 control.c    // C言語からpythonを呼び出すために必要なライブラリ
     📃 control.h    // control.cファイルのヘッダファイル
     📃 testbot.c    // botプログラムのサンプル．このプログラムを参考にbotプログラムを書く
```

### ライブラリのアップデート

提供するライブラリは現在試行錯誤の段階であり，モジュールがアップデートされる場合があります．その場合は以下のコマンドでモジュールをアップデートすることができます．ダウンロードした個所が「```~/```」であった場合

``` sh
cd ~/
git pull
```

とするとアップデートする事ができます．ただし，提供するプログラムやファイルを編集するとアップデートできない場合があります．

### コンパイル方法

コンパイルは以下のコマンドでコンパイルする．ここではtestbot.cプログラムを自分で作成したプログラムとしてコンパイルし，minebot.exeという実行ファイルを作成する．

```sh
cd ~/
cd Minecraft_Contest
gcc -o minebot.exe testbot.c control.c
```

コンパイルするとminebot.exeという実行ファイルが作成される．またC演習ではccというコマンドで実行していたが，本botライブラリではgccコマンドを利用する事に注意が必要です．

### 実行方法

プログラムを実行する際には以下の手順で実行します．

1. Minecraftを起動しマップを選択する．マップはTeamsで配布するフラット，天候の変化をOFF，またMobはわかないようにしたマップを利用する*．  
2. c1-byodをアクティブ化し，作成したプログラムを実行する  
3. botプログラムを実行する．  

*フラットを「新しく作る」場合には，【新規→「世界のタイプ」で「フラット」を選択】で作成できます．また，【チート→「天候の変化」を「OFF」】に設定しておきましょう．

プログラムを実行すると自動でx20,y20の位置に横幅900，縦幅1080にMinecraftのゲーム画面が自動調整される．本ライブラリは画像処理を用いてゾンビを判定しているため，指定されたデスクトップの場所（x20,y20から横幅900，縦幅1080）でMinecraftを起動させる必要があります．

また，Minecraftを最小化した状態で，Botプログラムを動かすと正常に動作しません．Minecraftは最小化するとリソース節約のため動作停止し，起動するまでに時間がかかるため，Botの初期化処理等が正常に動作しません．

※稀にerror:detectZombieという表示が出ますが，その場合再度実行して頂ければ動作します．

### 停止方法

プログラムを停止する際には「F12」を押す（連打する）と停止させることができます．停止できたらc1-byod上に終了コード送信と表示されます．

※上記でも停止しない場合はc1-byod上で`【Ctrl + C】`で強制終了できますが，ゾンビプロセスが発生する可能性があります．その場合はPCを再起動する等で対応して下さい．

---

## ◇Botプログラムの作成方法

### プログラム作成手順

ダウンロードしたMinecraft_Contestフォルダの中にプログラムを作成する必要があります．Minecraft_Contestフォルダの中にtestbot.cを改変して頂いても結構ですし，新たにファイルを作成して頂いても結構です．その時に[コンパイル方法](#コンパイル方法)でも注意したようにgccコマンドを使う事と，control.cをコマンドの中に入れてコンパイルするようにしてください．

Botプログラムで必ず実行しなければいけないプログラムが以下となります．実行しなければいけないプログラム（関数）のはコメントを参考にしてもらえれば良いかと思います．また，Botプログラムを書く個所は以下のプログラムのwhile文のコメントの範囲となります．

```C
#include <stdio.h>
#include <unistd.h>
#include "control.h"

int main(int argc, char *argv[]){
    init();          //Minecraftのゲームコントロール関数．ウィンドウサイズを設定する等を行う．
    setTime();       //Minecraft上で時間を夜にしてくれる．
    exePython();     //画像処理プログラムを実行する関数．実行結果はdetectZombie，detectZombie2，detectMobs関数で取得できる．
    setSurvival();   //サバイバルモードにする．
    while(rk){       //無限loopする．rkはF12キーを押すと0となり，プログラムが停止します．
        /*ここからBotプログラムを書く*/

        /*ここまでBotプログラムを書く*/
        sleep(0.1);
    }
    setCreative();  //クリエイティブモードにする．
    setMorning();   //朝にする．
}
```

### ライブラリの関数一覧

関数一覧．

| 戻値 関数名(引数) | 説明 |
| :- | - |
| void init(void) | Botプログラム初期設定関数．Minecraftのウィンドウ位置，サイズを強制的にx20，y20の横幅920，縦幅1080のする．また，マウスの初期位置を記憶する．またプレイヤーの移動を監視するプログラムを起動する． |
| void exePython(void) | Botプログラム初期設定関数．画像処理プログラムが実行される．|
| void setTime(void) | Minecraftの時間を夜に設定してくれる． |
| void setMorning(void) | Minecraftの時間を朝に設定してくれる． |
| void setSurvival(void) | Minecraftの設定をサバイバルモードにしてくれる． |
| void setCreative(void) | Minecraftの設定をクリエイティブモードにしてくれる． |
| int detectZombie1(void) | 画像処理の結果を取得する．戻り値はint型で，7bitの2進数結果を10進数に変換した0から127までのint型が返却される．詳細は後述． |
| int detectZombie2(void) | 画像処理の結果を取得する．戻り値はint型で，15bitの2進数結果を10進数に変換した0から4924までのint型が返却される．詳細は後述． |
| int detectZombie3(void) | 画面を6分割して左から順に検出した場所を1にする．戻り値はint型で，例えば100001だと左端と右端に検出された状態．詳細は後述． |
| int detectMobs(void) | 画面を６分割して検出．戻り値はint型で，数字が１の時はゾンビ，２の時はクリーパーのように出力する．たとえば200001だと画面左端にクリーパーがいて右端にゾンビがいる状態．詳細は後述． |
| void attackLeft(void) | 左クリック．0.01秒間隔で入力されるが，実際にはもう少し遅い． |
| void attackRight(void) | 右クリック．1.50秒間入力される． |
| void moveForward(double time) | 前進する．timeで指定した時間（単位は秒,ミリ秒も指定できる）動く． |
| void moveLeft(double time) | 左に動く．timeで指定した時間（単位は秒,ミリ秒も指定できる）動く． |
| void moveRight(double time) | 右に動く．timeで指定した時間（単位は秒,ミリ秒も指定できる）動く． |
| void moveBack(double time) | 後進する．timeで指定した時間（単位は秒,ミリ秒も指定できる）動く． |
| void moveForwardLeft(double time) | 左斜め前に動く．timeで指定した時間（単位は秒,ミリ秒も指定できる）動く． |
| void moveForwardRight(double time) | 右斜め前に動く．timeで指定した時間（単位は秒,ミリ秒も指定できる）動く． |
| void moveBackLeft(double time) | 左斜め後ろに動く．timeで指定した時間（単位は秒,ミリ秒も指定できる）動く． |
| void moveBackRight(double time) | 右斜め後ろに動く．timeで指定した時間（単位は秒,ミリ秒も指定できる）動く． |
| void movejump(int times) | ジャンプする．timesで指定した回数分，連続でジャンプし続ける． |
| void setDash(void) | 常に移動がダッシュになる． |
| void resetDash(void) | 常に移動が歩くになる． |
| void cameraDown(double time) | カメラを下にtimeで指定した時間（単位は秒,ミリ秒も指定できる）動かす． |
| void cameraLeft(double time) | カメラを左にtimeで指定した時間（単位は秒,ミリ秒も指定できる）動かす． |
| void cameraRight(double time) | カメラを右にtimeで指定した時間（単位は秒,ミリ秒も指定できる）動かす． |
| void cameraUp(double time) | カメラを上にtimeで指定した時間（単位は秒,ミリ秒も指定できる）動かす． |
| void cameraCenter(void) | カメラ（カーソル）を中央に戻す． |

例えば以下のようなプログラムを記載するとc1-byod環境でF12キーを押さない限り前進し続けるプログラムとなる．

```C
#include <stdio.h>
#include <unistd.h>
#include "control.h"

int main(int argc, char *argv[]){
    init();          //Minecraftのゲームコントロール関数．ウィンドウサイズを設定する等を行う．
    setTime();       //Minecraft上で時間を夜にしてくれる．
    exePython();     //画像処理プログラムを実行する関数．実行結果はdetectZombie関数で取得できる．
    setSurvival();   //サバイバルモードにする．
    while(rk){       //無限loopする．rkはF12キーを押すと0となり，プログラムが停止します．
        /*ここからBotプログラムを書く*/

           moveForward(1.2); //1.2秒前進する 

        /*ここまでBotプログラムを書く*/
        sleep(0.1);
    }
    setCreative();  //クリエイティブモードにする．
    setMorning();   //朝にする．
}
```

### ゾンビ検出関数，detectZombie1関数の仕様

地平線にクロスヘアを合わせた状態で，地面部分にゾンビがいるかを検出を行う．detectZombie1関数はint型が戻り値である．int型の戻り値を2進数であり先頭のビットから画面左上，画面真ん中上・・・・というように各桁でゾンビがいるかの結果が格納されている．各値の詳細は以下の通り．

| 7桁目 | 6桁目 | 5桁目  | 4桁目 | 3桁目 | 2桁目 | 1桁目 |
| :- | - | - | - | - | - | - |
| 地面の左上にゾンビがいれば1，いなければ0となる． | 地面の真ん中上にゾンビがいれば1，いなければ0となる． | 地面の右上にゾンビがいれば1，いなければ0となる． | 地面の左下にゾンビがいれば1，いなければ0となる． | 地面の真ん中下にゾンビがいれば1，いなければ0となる． | 地面の右下にゾンビがいれば1，いなければ0となる． | 1であれば攻撃がヒット，0であれば攻撃が当たってない（精度はよくない） |

例えば，detectZombie関数の戻り値が1010000の場合，地面の左上，右上にゾンビがいるとなる．また，0100100となると画面中央にゾンビがいるため，ゾンビが接近している可能性がある．そのため攻撃できる可能性があるという事になる．ただし，画面中央にいるからと言って必ず攻撃ができるという訳では無い．

本ライブラリは単純に現状のゾンビ配置からゾンビを倒そうとするとかなり難易度が高いです．ゾンビの配置を何らかの手法で記憶させておく必要があります．

また，本ライブラリはC言語から無理やり動作させるため精度が悪いです．もし，精度に不満がある場合は下の「更なる改良を行いたい方へ」をお読みください．もちろん精度の悪いなり工夫を入れると事が本コンテストの目的でもあります．

### ゾンビ動態検出関数，detectZombie2関数の仕様

利用する場合は**明るさの設定を50**にして利用してください．

detectZombie2関数を呼び出すと0～4924までの値が戻り値として返却される．戻り値は2進数を10進数に返却した15bitの値である．画像検出は以下のURLにある画像の①～⑤ように画面右下，左下，右上，左上，中央の5カ所で動態検知をしています．更にそのエリアの中で遠距離，中距離，近距離の３段階を検出し，これらを15bitの値で管理されています．また，近距離にゾンビがいる場合は攻撃が当たる範囲にいる可能性が高いです．

[detectZombie2による動態検出画像](https://oskit-my.sharepoint.com/:i:/g/personal/masaki_obana_oit_ac_jp/EV6amJG9qf9IikoaKJ8ksM8BMUpeWLJ5TaZ6FPEEtqk7Fg?e=Pga4NH) （右クリックから「開く」を選択しないと見れないかも・・・）

戻り値を2進数に変換し，15bitを3bit区切りととなり先頭から右下 ， 左下 ， 右上 ， 左上 ， 中央の順番に遠距離を表記されています．具体的な例は以下の表を参考にして下さい．

| 状態 | 10進数 | 2進数 |
| :- | - | - |
| 全検出エリアの遠くにゾンビがいる | 18724 |100100100100100 |
| 全検出エリアの中距離にゾンビがいる | 9362 |010010010010010 |
| 全検出エリアの近距離にゾンビがいる | 4681 |001001001001001 |
| 検出エリア中央の近距離にゾンビがいる | 1 |000000000000001 |
| 検出エリア中央に近距離と画面左上の中距離にゾンビがいる | 17 | 000000000010001 |

また，detectZombie2関数には以下のような欠点が存在します．

- 動くものがあるとゾンビがいると検出される可能性があります．具体的にはスポーンブロックの炎のエフェクトや剣を振る動作等．  
- 同一の検出エリアにゾンビが複数体いると動態検出の面積が増えてしまい近距離にゾンビがいると検出してしまう  
- 画面中央の検出精度は高いが、周辺の検出精度は低い  
- 境界線をまたぐ場合、画面中央を優先した値を返却することが多い  

### ゾンビ検出関数，detectZombie3関数の仕様

### Mob検出関数，detectMobs関数の仕様

学習を用いたMob検出関数．学習ライブラリとしてYOLOを用いて画像からゾンビを検出する手法．運営から学習データが提供されているが，もし自分でも学習を行いたい場合は本ページの末尾にある[detectMobs関数で利用する学習データを自分で作成したい方へ](#detectmobs関数で利用する学習データを自分で作成したい方へ)を参照にしてください．

detectMobs関数を呼び出すとint型の6桁の値が戻り値として返却される．戻り値は検出したMobがいない場合は0，ゾンビを1，クリーパーを2として値を返す．検出結果は画面を縦に6分割し，分割した各領域にMobがいるかの判定を表記する．例えば画面の一番左にクリーパーがいて一番右にゾンビがいれば200001が返却される．

## 更なる改良を行いたい方へ

本ライブラリはpythonで書いたMinecraft操作プログラムをC言語から実行することでBotライブラリとしています．このような構造になっている目的はC演習Ⅰで学習した内容を用いてBotプログラムを作成しよーという事なのですが，もし力量があるかたはPythonプログラムの改変をして頂いても問題ありません．Pythonプログラムは以下のフォルダに存在します．また，新しいpythonプログラムを作成して頂いても問題ありません．ただし，コンテスト時には必ずC言語で書かれたコードを実行するようにしてください．

ただし，pythonプログラムを改変する場合，今後運営から提供されるアップデートは実行しないでください．実行すると最悪，改変したcontrol.cやpythonファイル等の全てモジュールが削除されてしまいます．

```file
  📁micr_cont      // 一番最初のフォルダ
   ┗📁python          // この上にはmicr_contフォルダがある．
     ┗📁minecraft       // このminecraftフォルダにpythonコードが入っている．
       ┗📃Control.py
         📃clickLeft.py
         📃clickRight.py
         :
         :
         :
         📃pushKey.py
         📃test.py
```

## detectMobs関数で利用する学習データを自分で作成したい方へ

detectMobs関数で利用している`detectMobs.py`は[YOLOv5](https://github.com/ultralytics/yolov5)という物体検出で敵を認識しています。
YOLOは**学習**させることにより、新しく物体を認識できるようになります。 自分でも学習を行いたい方は以下をお読みください。

1. 学習対象の画像を用意する  
   学習とは大量のデータから特徴を見つけ出す作業のことです。  
   YOLOv5は物体検出のAIですので学習には画像が必要です。  
   この用意する画像は10枚や20枚では全くデータが不足であり、最低でも100枚の画像が必要になります(ただし、それでも精度は悪いです)。  
   もちろん画像は大量にあればあるほど精度は良くなります。
   また、画像も同じシーン(背景や対象の向き等)ばかりだとAIはそのシーンでしか対象を認識できなくなるため、いろんなバリエーションの画像を用意する必要があります。  
   例えばゾンビの認識精度を向上させたいとします。  
   この場合、背景が白でゾンビが真正面を向いた画像を1000枚用意したところで、AIは背景が白でゾンビが真正面からしか認識できません。  
   背景が赤になれば認識できなくなり、ゾンビが90度方向転換すると認識できなくなります。  
   これを解消するためには様々な背景、明度、ゾンビとの距離、ゾンビの位置、ゾンビの向きなどを用意する必要があります。  
   また画像の取得方法ですが、Minecraft Education EditionではF2を押してもゲーム内でスクリーンショットが出来ないため、Windows標準のツールでスクリーンショットを取る必要があります（スクリーンショットは`Win + Shift + S`で撮れます）。  
   手動で何枚も撮影するのは大変だという人のために簡単なPythonスクリプトを用意しましたので参考にしてください。ただし、**動作する保証はありません**。  

---
   <details>
   <summary>Pythonのスクリプト</summary>

   以下のコマンドをc1-byod上で実行して必要なライブラリをインストールします。  

   ```sh
   pip install pywin32 Pillow tqdm
   ```

   次に以下のコードを`ss.py`などに保存します。  

   ```py
   import win32gui
   from PIL import ImageGrab
   import time
   from tqdm import tqdm
   import os
   import datetime

   LIMIT = 30
   SLEEP = 5
   date = datetime.datetime.now().strftime("%m%d")

   os.makedirs("img", exist_ok=True)

   for i in tqdm(range(1, LIMIT+1)):
       time.sleep(SLEEP)
       x0, y0, x1, y1 = win32gui.GetWindowRect(win32gui.GetForegroundWindow())
       im = ImageGrab.grab((x0, y0, x1, y1), all_screens=True)
       im.save(f"img/{date}_{str(i).zfill(3)}.png")
   ```

   Minecraft Education Editionを起動し、撮影したい場面になったら以下のコマンドを実行してください。  
   ただし、Minecraft Education Editionのウィンドウをアクティブに(単にマイクラの画面をクリックすれば良い)する必要があります。  

   ```sh
   python ss.py
   ```

   これで`ss.py`と同じディレクトリに`img`ディレクトリが生成され、そこに画像が保存されます。  
   撮影枚数を変更したい場合は`LIMIT`を、撮影間隔を変更したい場合は`SLEEP`の値を変更してください。  

   </details>

---

2. 用意した画像に対してアノテーションをする  
  撮影した画像はどこにゾンビがいるのか、何体いるのかなどを教える必要があります。  
  これは手動で指定する必要があります。  
  アノテーションするツールとしてはlabelImgやVoTT等がありますが、今回はlabelImgを紹介します。  
   1. Windows版のlabelImgは[ここをクリック](https://github.com/tzutalin/labelImg/files/2638199/windows_v1.8.1.zip)するとダウンロードできます。zip形式ですので展開しましょう。  
   2. `data/predefined_classes.txt`を開き、一旦中身を全て消します。  
   3. 学習対象のラベル名(ゾンビだと英語でzombie)を記述します。  
   4. labelImg.exeを起動し、`Open Dir`から画像が入っているディレクトリを選択し、```フォルダーの選択```をクリックします。  
   5. `Change Save Dir`から結果(アノテーションファイル)の保存先を変更します。(適当にわかりやすい場所へ)  
   6. 左のメニューバーの真ん中あたりにある`PascalVOC`と書いてる部分をクリックして`YOLO`に変更します。  
      **これをしないと全ての作業が無駄になります。必ず確認するように**  
   7. `Create RectBox`をクリックして選択ツールを起動します。  
   8. 学習対象の左上にマウスのカーソルを持っていき、左クリックするとそこが支点となり選択ツールが移動します。  
   9. 左クリックしたまま学習対象が全部覆われるようにカーソルを操作し、覆われたら左クリック離します。  
   10. ラベルを選ぶ画面が出るので適当なラベル(ゾンビならzombie)を選択し、OKを押します。  
   11. `Save`を押すと保存されます。  
   12. `Next Image`で次の画像へ、`Prev Image`で前の画像に移動します。  
   13. これを全ての画像に対して行います。  

---

   <details>
   <summary>(参考)labelImgのショートカットキー</summary>
  
   labelImgのショートカットキー  
   時短になるかも  

   - Ctrl + u : フォルダから全ての画像をロード
   - Ctrl + r : デフォルトのアノテーションターゲットディレクトリを変更
   - Ctrl + s : 保存
   - Ctrl + d : 現在のラベルと四角のボックスをコピー
   - Ctrl + Shift + d : 現在の画像を削除
   - Space : 現在の画像に確認済みフラグを付与
   - w : 短形を作成
   - d : 次の画像
   - a : 前の画像
   - del : 選択した短形を削除
   - Ctrl++ : ズームイン
   - Ctrl-- : ズームアウト
   - ↑↓→← : 選択した短形を移動

   </details>

---

3. YOLOに学習させる  
   1. c1-byodで適当なディレクトリ(`yolo`とか)を作成し、cdで移動します。  
   2. 以下のコマンドを打ち、yolov5をダウンロードします。  

      ```sh
      git clone https://github.com/ultralytics/yolov5
      ```

   4. 以下のコマンドを打ち、必要なPythonライブラリをインストールします。  

      ```sh
      cd yolov5
      pip install -r requirements.txt
      ```

   6. mkdirコマンド等を使用して、以下のようなディレクトリ構成にします。  

      ```txt
        yolov5
          ┠ data          ← 元からある
            ┠ train       ← 作る
            ┃   ┠ images  ← 作る
            ┃   ┗ labels  ← 作る
            ┠ valid       ← 作る
                ┗ images  ← 作る
      ```

   5. data/train/imagesディレクトリに学習させる画像を、labelsディレクトリにlabelImgで作ったアノテーションファイル(`Change Save Dir`で指定したフォルダに入ってる)を入れます。  
   6. data/valid/imagesには学習結果をテストする画像(train/imagesと同じでいい)を入れます。  
   7. dataディレクトリ内に```data.yaml```ファイルを作成し、以下の内容を記述します。  

      ```yaml
      train: data/train/images # 学習の画像のパス
      val: data/valid/images # 検証用画像のパス

      nc: 1 # (例)学習させるMobの数
      names: [ "zombie" ] # (例)学習対象のMob名
      ```

      ※ 提供しているプログラムではゾンビとクリーパーだけを認識の対象としているためdata.yamlの内容は[このようになっています](https://github.com/masaki555/Minecraft_Contest/blob/main/python/minecraft/yoloFiles/mobs.yaml)が、独自に認識するMobを増やしたり、減らしたりすると提供しているプログラムとの齟齬が発生し上手く動かない可能性があります。  
      不具合が発生した場合は各自でプログラムを改修して下さい。  

   8. 以下のコマンドを実行して学習を開始します。  
      ただし、CPUで計算を行うと莫大な時間かかります。  
      枚数やパソコンの性能にもよりますが数時間はかかります。  

      ```sh
      python train.py --data data/data.yaml --cfg yolov5s.yaml --weights '' --batch-size 8 --epochs 300
      ```

   9. 学習が完了すると、```runs/train/exp/weights```ディレクトリに```best.pt```ファイルが生成されます。  
      このbest.ptファイルを```Minecraft_Contest/python/minecraft/yoloFiles/best.pt```ファイルと差し替えるとAIに使用される学習データが差し替えられます。  

   (補足)上記の方法だとCPUしか使わないので遅いです(数時間かかる)。  
   学習を早くするにはGPUを使うことをおすすめしますが環境構築に手間がかかります。  
   Google Colabなどのサービスを利用すると環境構築なしでGPUを使えるようになるのでオススメです(無料で利用すると利用制限はありますが)。  
