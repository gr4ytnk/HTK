## 「もしもし」という音声を認識させてみる

※入力となるものは太字、出力となるものは斜体とした  

### STEP1 音素のリストと辞書を作成する

HDMan -m -w **wlist** -n *monophones1* -l *dlog* *dict* **beep**

* wlist
```
MOSIMO 
MOSIMOSI
SISHAMO
```

* beep  
```
MOSIMO          mo si mo sp
MOSIMOSI        mo si mo si sp
SISHAMO         si sja mo sp
silence         sil
```

* global.ded  
```
AS  sp
```

実行すると下記ファイルができる  

* monophones1  
```
mo
si
sp
sja
```

* dlog  
(要確認)beep.ded  
```
WARNING: no script file beep.ded

Dictionary Usage Statistics
---------------------------
  Dictionary    TotalWords WordsUsed  TotalProns PronsUsed
        beep         3          3          3          3
        dict         3          3          3          3

3 words required, 0 missing

New Phone Usage Counts
---------------------
  1. mo    :     5
  2. si    :     4
  3. sp    :     3
  4. sja   :     1

Dictionary dict created
```

* dict 
```
MOSIMO      mo si mo
MOSIMOSI	mo si mo si  
SISHAMO     si sja mo
```

### STEP2 音響特徴抽出

HCopy -C *config* hoge.wav hoge.mfc  
あるいは  
HCopy -A -D -T 1 -C *config* -S *hoge.script*  

 (INPUT)
   config : 
   script : 
 (OUTPUT)


### STEP3 平均の初期モデルを作る

HCompV -D *config* -f 0.01 -m -S *trainlist* -M hmm00 proto hai.hmm

 (INPUT)
   config : 
   trainlist : 
   hai.hmm : 
   proto : 
 (OUTPUT)
   hmm00 : 出力先フォルダ
 (memo)
   「-f」を付けないとvFloorsができない

### STEP4 音素ラベルファイルを作成する（初期モデルを全音素の初期値とする）

HLEd -l '*' -d dict -i phones0.mlf mkphones0.led words.mlf

 (INPUT)
   dict : 単語の音素ラベル辞書
   mkphones0.led : スクリプト
   words.mlf : 音声ファイル中に出現する単語を記述
 (OUTPUT)
   phones0.mlf

### STEP5 音素HMMを推定（この段階ではspは無視？）

HERest -C <config> -I <phones0.mlf> -t 250.0 150.0 1000.0 -S <trainlist.txt>
       -H hmm0/macro
       -H hmm0/hmmdefs
       -M hmm1 monophones0

 (INPUT)
   macro : 
   hmmdefs : 
   monophones0 : HDMan で作ったもの
 (OUTPUT)

### STEP6 spモデルを作成する

### STEP7 spの位置を推定
