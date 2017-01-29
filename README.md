## 「もしもし」という音声を認識させてみる

※入力となるものは太字、出力となるものは斜体とした  

### STEP5 音素HMMを推定（この段階ではspは無視？）

HERest -C **config** -I **phones0.mlf** -t 250.0 150.0 1000.0 -S **trainlist.txt** -H **hmm0/macro** -H **hmm0/hmmdefs** -M *hmm1* **monophones0**

* sil.hed

```
AT 2 4 0.2 {sil.transP}
AT 4 2 0.2 {sil.transP}
AT 1 3 0.3 {sp.transP}
TI silst {sil.state[3],sp.state[2]}
```

* config  

* phones0.mlf  
前のSTEPで作ったもの

* trainlist.txt  
mfcのリスト

* hmm0/macro  
前のSTEP（ここではHCompV）で作ったもの

* hmm0/hmmdefs  
前のSTEP（ここではHCompV）で作ったもの

「hmm1」フォルダの下に下記ファイルができる

* macro

```
~o
<STREAMINFO> 1 39
<VECSIZE> 39<NULLD><MFCC_D_A_0><DIAGC>
~h "mo"
<BEGINHMM>
<NUMSTATES> 5
<STATE> 2
<MEAN> 39
 -6.481543e+00 -4.320533e+00 -5.185712e-01 -2.265626e+00 -5.894022e+00 1.388931e+00 1.147384e+00 -4.798144e+00 3.092593e+00 -4.408344e+00 7.945961e-01 -1.331538e+00 4.968016e+01 -1.381301e-02 4.907601e-02 -1.552674e-03 1.568830e-02 4.700995e-02 -1.348041e-03 1.581648e-02 2.071070e-02 -3.729460e-02 -7.039736e-03 7.745116e-03 5.697028e-03 -3.299749e-02 1.067933e-03 2.652938e-03 -1.046571e-03 -6.768662e-06 1.247257e-04 1.023248e-03 4.681133e-04 -9.192093e-05 1.749156e-04 -1.267839e-04 -7.073302e-04 -1.512176e-04 -1.148665e-03
<VARIANCE> 39
 1.017586e+02 4.411649e+01 2.211034e+01 4.737695e+01 5.650426e+01 3.372556e+01 2.588211e+01 3.052908e+01 4.605659e+01 3.290014e+01 1.950719e+01 1.987059e+01 1.616314e+02 3.236403e+00 1.609022e+00 1.238859e+00 1.974270e+00 2.117535e+00 2.316510e+00 1.558030e+00 2.016000e+00 2.530082e+00 1.685842e+00 1.479671e+00 1.507811e+00 1.482387e+00 3.865514e-01 2.905205e-01 2.135891e-01 3.311493e-01 3.627838e-01 3.890669e-01 2.993849e-01 3.642755e-01 4.702339e-01 3.004113e-01 2.870017e-01 2.849093e-01 1.798647e-01
<GCONST> 1.123745e+02
<STATE> 3
<MEAN> 39
 -6.557369e+00 -3.990957e+00 -5.336839e-01 -2.170715e+00 -5.631650e+00 1.409206e+00 1.252856e+00 -4.686622e+00 2.874421e+00 -4.452777e+00 8.379654e-01 -1.300707e+00 4.949437e+01 -9.117105e-03 5.834346e-02 -4.065338e-03 1.372083e-02 4.494422e-02 4.102655e-03 1.753508e-02 1.765032e-02 -3.765275e-02 -6.780053e-03 7.192734e-03 4.863546e-03 -3.417224e-02 7.063015e-04 1.508553e-03 -4.171385e-04 -4.367379e-04 -4.674163e-04 8.746919e-04 2.711218e-04 -6.448133e-04 -1.307943e-04 9.833045e-05 -7.492434e-05 -1.571621e-04 1.156039e-05
<VARIANCE> 39
 1.030486e+02 4.123187e+01 2.251454e+01 4.706364e+01 5.713811e+01 3.346571e+01 2.582796e+01 3.130943e+01 4.526794e+01 3.325710e+01 1.984836e+01 1.994303e+01 1.657648e+02 3.292361e+00 1.585432e+00 1.244553e+00 1.957937e+00 2.137926e+00 2.301161e+00 1.567504e+00 2.026967e+00 2.515295e+00 1.686103e+00 1.488842e+00 1.495607e+00 1.533017e+00 3.947141e-01 2.811638e-01 2.122236e-01 3.229215e-01 3.626191e-01 3.838812e-01 3.011371e-01 3.630203e-01 4.696110e-01 2.996581e-01 2.884571e-01 2.815656e-01 1.812957e-01
<GCONST> 1.123787e+02
<STATE> 4
<MEAN> 39
 -6.144247e+00 -2.343777e+00 -7.467741e-01 -2.316627e+00 -4.975558e+00 1.289559e+00 1.900157e+00 -4.547179e+00 1.651790e+00 -4.087105e+00 4.204414e-01 -1.595284e+00 4.736975e+01 -1.493336e-02 2.957694e-02 1.238701e-03 1.304664e-02 3.323473e-02 -2.306788e-03 1.089510e-02 1.197330e-02 -2.876474e-02 2.438490e-03 3.943921e-03 -2.694211e-03 -4.392653e-02 -1.355031e-04 -4.546219e-04 -7.268907e-05 -3.010994e-04 -2.360975e-04 -3.030870e-04 -2.424064e-04 -1.779470e-04 9.004396e-05 3.002829e-04 -1.006401e-04 -2.875096e-04 -4.181256e-04
<VARIANCE> 39
 9.375848e+01 2.718212e+01 2.147682e+01 4.846151e+01 6.224257e+01 3.165227e+01 2.523283e+01 2.831376e+01 4.355711e+01 3.360896e+01 1.961309e+01 1.932380e+01 1.601417e+02 2.750465e+00 1.192702e+00 1.119909e+00 1.763100e+00 2.013409e+00 2.090581e+00 1.487501e+00 1.820464e+00 2.331308e+00 1.684904e+00 1.459978e+00 1.396597e+00 1.204940e+00 3.093289e-01 2.124481e-01 1.905522e-01 2.757556e-01 3.432006e-01 3.432504e-01 2.852763e-01 3.299196e-01 4.265659e-01 2.974349e-01 2.810799e-01 2.602482e-01 1.306431e-01
<GCONST> 1.086113e+02
<TRANSP> 5
 0.000000e+00 1.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00
 0.000000e+00 8.349825e-01 1.650175e-01 0.000000e+00 0.000000e+00
 0.000000e+00 0.000000e+00 8.349823e-01 1.650177e-01 0.000000e+00
 0.000000e+00 0.000000e+00 0.000000e+00 9.742312e-01 2.576885e-02
 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00
<ENDHMM>
```

* vFloors
```
~o
<STREAMINFO> 1 39
<VECSIZE> 39<NULLD><MFCC_D_A_0><DIAGC>
~v "varFloor1"
<VARIANCE> 39
 8.820943e-01 3.861458e-01 2.029625e-01 4.565988e-01 5.828498e-01 3.247619e-01 2.395605e-01 2.902121e-01 4.258837e-01 3.167320e-01 1.901695e-01 1.848357e-01 1.582491e+00 2.507793e-02 1.302295e-02 1.097857e-02 1.644960e-02 1.892603e-02 2.083207e-02 1.524675e-02 1.840137e-02 2.125722e-02 1.673089e-02 1.448895e-02 1.457264e-02 1.123323e-02 3.027864e-03 2.330465e-03 1.922042e-03 2.741395e-03 3.304867e-03 3.573498e-03 2.958795e-03 3.398049e-03 3.900207e-03 3.005882e-03 2.781184e-03 2.784989e-03 1.345709e-03
```

"mo"のモデルのMEANタグ１行目を抜粋。学習によりモデルが置き換わっていることが確認できる  
[hmm0/macro]  
`-7.092237e+00 -3.793652e+00 -7.671155e-01 -2.177134e+00 -5.022529e+00 1.197599e+00 1.391658e+00 -4.522811e+00 2.475666e+00`  
[hmm1/macro]  
`-6.481543e+00 -4.320533e+00 -5.185712e-01 -2.265626e+00 -5.894022e+00 1.388931e+00 1.147384e+00 -4.798144e+00 3.092593e+00`  

これを何度か実行すると以下のようになった。読みこませているmfcはずっと同じものなのに、INPUTとなるモデルが変わることでOUTPUTのモデルも変わる。  
[hmm5/macro]  
`-7.641440e+00 -1.298460e+01 5.198019e-01 -7.609424e-01 -8.326129e+00 5.962046e-01 -2.009490e+00 -4.396775e+00 9.343076e+00`  


### STEP4 音素ラベルファイルを作成する（初期モデルを全音素の初期値とする）  

HLEd -l '\*' -d **dict** -i *phones0.mlf* **mkphones0.led** **words.mlf**

* dict  
前のSTEPで作ったもの

* mkphones0.led

```
EX
IS  sil sil
DE  sp
```

* words.mlf
```
#!MLF!#
"mosi1.lab"
MOSIMOSI
.
```
* mosi1.lab
```
mosi1.lab mo si mo si
````
下記のファイルができる

* phones0.mlf
```
#!MLF!#
"*/mosi1.lab"
sil
mo
si
mo
si
sil
.
```


 (INPUT)
   dict : 単語の音素ラベル辞書
   mkphones0.led : スクリプト
   words.mlf : 音声ファイル中に出現する単語を記述
 (OUTPUT)
   phones0.mlf


### STEP2 音響特徴抽出

HCopy -A -D -T 1 -C **config.hcopy** -S **script.hcopy**  

* config.hcopy

```
SOURCEFORMAT = WAV        # 
SOURCEKIND = WAVEFORM     # 波形ファイル
SOURCERATE = 625          # 16Khzの場合 [単位は100ns]　-> 0.062ミリ秒
TARGETKIND = MFCC_0_D_A   #
TARGETRATE = 100000.0     # フレーム周期 [単位は100ns]　-> 10ミリ秒
WINDOWSIZE = 250000.0     # 分析窓長 [単位は100ns]　-> 25ミリ秒
USEHAMMING = T            # 分析窓としてハミング窓
PREEMCOEF = 0.97          # プリエンファシス係数
NUMCHANS = 24             # メルフィルタバンクフィルタ数
NUMCEPS = 12              # 最終的なMFCCの次数
```

特徴量ファイルをテキストで見てみる  

HList -r **mosi1.mfc** | head -n 1
```
-2.697013e-01 -2.482266e+00 -2.217198e-01 -3.174071e+00 -4.147585e+00 6.754822e+00 3.201489e+00 -6.396346e+00 -9.431622e-01 -1.493355e+01 -2.665384e+00 -5.884359e+00 5.106169e+01 -2.713463e+00 -2.857542e+00 1.719417e-01 4.600145e-01 -1.084255e+00 3.815895e-01 -7.236861e-01 1.281893e+00 4.140334e+00 3.876592e+00 3.926499e-01 4.204645e-02 -1.445935e+00 1.516768e-01 3.470743e-01 -2.318866e-01 -3.735559e-01 -6.564298e-01 -1.790522e-01 2.597749e-01 -3.676489e-01 -5.843784e-01 -4.866700e-01 -1.565206e-01 1.550754e-01 2.862236e-01 
```

### STEP3 平均の初期モデルを作る

HCompV -C **config.HCompV** -f 0.01 -m -S **trainlist.txt** -M *hmm0* **proto**

* config.HCompV

* proto

hmm0フォルダの下に「macro」、「proto」、「vFloors」が作られる

* hmm0/macro
※「~h "mo"」のみ抜粋、音素の数だけセクションができる（si,sja,sil,sp）
```
~o
<STREAMINFO> 1 39
<VECSIZE> 39<NULLD><MFCC_D_A_0><DIAGC>
~h "mo"
<BEGINHMM>
<NUMSTATES> 5
<STATE> 2
<MEAN> 39
 -7.092237e+00 -3.793652e+00 -7.671155e-01 -2.177134e+00 -5.022529e+00 1.197599e+00 1.391658e+00 -4.522811e+00 2.475666e+00 -3.983630e+00 5.284266e-01 -1.634990e+00 4.749557e+01 -4.500959e-03 3.395595e-02 -3.449689e-03 -3.757998e-03 3.827242e-02 -1.681405e-03 1.282120e-02 1.411823e-02 -3.382706e-02 1.359682e-02 1.815424e-02 -5.942781e-03 -3.426783e-02 2.786980e-04 1.465090e-03 1.092864e-04 -1.989758e-05 1.961447e-03 2.404057e-03 1.864515e-03 -2.342970e-03 -1.394336e-03 1.911470e-03 -1.182858e-03 -6.819504e-04 -7.749228e-04
<VARIANCE> 39
 8.820943e+01 3.861458e+01 2.029625e+01 4.565988e+01 5.828498e+01 3.247619e+01 2.395605e+01 2.902121e+01 4.258837e+01 3.167321e+01 1.901695e+01 1.848357e+01 1.582491e+02 2.507793e+00 1.302295e+00 1.097857e+00 1.644960e+00 1.892603e+00 2.083208e+00 1.524675e+00 1.840137e+00 2.125722e+00 1.673089e+00 1.448895e+00 1.457264e+00 1.123323e+00 3.027864e-01 2.330465e-01 1.922042e-01 2.741396e-01 3.304867e-01 3.573498e-01 2.958795e-01 3.398049e-01 3.900207e-01 3.005882e-01 2.781184e-01 2.784989e-01 1.345709e-01
<GCONST> 1.084410e+02
<STATE> 3
<MEAN> 39
 -7.092237e+00 -3.793652e+00 -7.671155e-01 -2.177134e+00 -5.022529e+00 1.197599e+00 1.391658e+00 -4.522811e+00 2.475666e+00 -3.983630e+00 5.284266e-01 -1.634990e+00 4.749557e+01 -4.500959e-03 3.395595e-02 -3.449689e-03 -3.757998e-03 3.827242e-02 -1.681405e-03 1.282120e-02 1.411823e-02 -3.382706e-02 1.359682e-02 1.815424e-02 -5.942781e-03 -3.426783e-02 2.786980e-04 1.465090e-03 1.092864e-04 -1.989758e-05 1.961447e-03 2.404057e-03 1.864515e-03 -2.342970e-03 -1.394336e-03 1.911470e-03 -1.182858e-03 -6.819504e-04 -7.749228e-04
<VARIANCE> 39
 8.820943e+01 3.861458e+01 2.029625e+01 4.565988e+01 5.828498e+01 3.247619e+01 2.395605e+01 2.902121e+01 4.258837e+01 3.167321e+01 1.901695e+01 1.848357e+01 1.582491e+02 2.507793e+00 1.302295e+00 1.097857e+00 1.644960e+00 1.892603e+00 2.083208e+00 1.524675e+00 1.840137e+00 2.125722e+00 1.673089e+00 1.448895e+00 1.457264e+00 1.123323e+00 3.027864e-01 2.330465e-01 1.922042e-01 2.741396e-01 3.304867e-01 3.573498e-01 2.958795e-01 3.398049e-01 3.900207e-01 3.005882e-01 2.781184e-01 2.784989e-01 1.345709e-01
<GCONST> 1.084410e+02
<STATE> 4
<MEAN> 39
 -7.092237e+00 -3.793652e+00 -7.671155e-01 -2.177134e+00 -5.022529e+00 1.197599e+00 1.391658e+00 -4.522811e+00 2.475666e+00 -3.983630e+00 5.284266e-01 -1.634990e+00 4.749557e+01 -4.500959e-03 3.395595e-02 -3.449689e-03 -3.757998e-03 3.827242e-02 -1.681405e-03 1.282120e-02 1.411823e-02 -3.382706e-02 1.359682e-02 1.815424e-02 -5.942781e-03 -3.426783e-02 2.786980e-04 1.465090e-03 1.092864e-04 -1.989758e-05 1.961447e-03 2.404057e-03 1.864515e-03 -2.342970e-03 -1.394336e-03 1.911470e-03 -1.182858e-03 -6.819504e-04 -7.749228e-04
<VARIANCE> 39
 8.820943e+01 3.861458e+01 2.029625e+01 4.565988e+01 5.828498e+01 3.247619e+01 2.395605e+01 2.902121e+01 4.258837e+01 3.167321e+01 1.901695e+01 1.848357e+01 1.582491e+02 2.507793e+00 1.302295e+00 1.097857e+00 1.644960e+00 1.892603e+00 2.083208e+00 1.524675e+00 1.840137e+00 2.125722e+00 1.673089e+00 1.448895e+00 1.457264e+00 1.123323e+00 3.027864e-01 2.330465e-01 1.922042e-01 2.741396e-01 3.304867e-01 3.573498e-01 2.958795e-01 3.398049e-01 3.900207e-01 3.005882e-01 2.781184e-01 2.784989e-01 1.345709e-01
<GCONST> 1.084410e+02
<TRANSP> 5
 0.000000e+00 1.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00
 0.000000e+00 6.000000e-01 4.000000e-01 0.000000e+00 0.000000e+00
 0.000000e+00 0.000000e+00 6.000000e-01 4.000000e-01 0.000000e+00
 0.000000e+00 0.000000e+00 0.000000e+00 7.000000e-01 3.000000e-01
 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00
<ENDHMM>
```

* hmm0/proto
```
~o
<STREAMINFO> 1 39
<VECSIZE> 39<NULLD><MFCC_D_A_0><DIAGC>
~h "proto"
<BEGINHMM>
<NUMSTATES> 5
<STATE> 2
<MEAN> 39
 -7.092237e+00 -3.793652e+00 -7.671155e-01 -2.177134e+00 -5.022529e+00 1.197599e+00 1.391658e+00 -4.522811e+00 2.475666e+00 -3.983630e+00 5.284266e-01 -1.634990e+00 4.749557e+01 -4.500959e-03 3.395595e-02 -3.449689e-03 -3.757998e-03 3.827242e-02 -1.681405e-03 1.282120e-02 1.411823e-02 -3.382706e-02 1.359682e-02 1.815424e-02 -5.942781e-03 -3.426783e-02 2.786980e-04 1.465090e-03 1.092864e-04 -1.989758e-05 1.961447e-03 2.404057e-03 1.864515e-03 -2.342970e-03 -1.394336e-03 1.911470e-03 -1.182858e-03 -6.819504e-04 -7.749228e-04
<VARIANCE> 39
 8.820943e+01 3.861458e+01 2.029625e+01 4.565988e+01 5.828498e+01 3.247619e+01 2.395605e+01 2.902121e+01 4.258837e+01 3.167321e+01 1.901695e+01 1.848357e+01 1.582491e+02 2.507793e+00 1.302295e+00 1.097857e+00 1.644960e+00 1.892603e+00 2.083208e+00 1.524675e+00 1.840137e+00 2.125722e+00 1.673089e+00 1.448895e+00 1.457264e+00 1.123323e+00 3.027864e-01 2.330465e-01 1.922042e-01 2.741396e-01 3.304867e-01 3.573498e-01 2.958795e-01 3.398049e-01 3.900207e-01 3.005882e-01 2.781184e-01 2.784989e-01 1.345709e-01
<GCONST> 1.084410e+02
<STATE> 3
<MEAN> 39
 -7.092237e+00 -3.793652e+00 -7.671155e-01 -2.177134e+00 -5.022529e+00 1.197599e+00 1.391658e+00 -4.522811e+00 2.475666e+00 -3.983630e+00 5.284266e-01 -1.634990e+00 4.749557e+01 -4.500959e-03 3.395595e-02 -3.449689e-03 -3.757998e-03 3.827242e-02 -1.681405e-03 1.282120e-02 1.411823e-02 -3.382706e-02 1.359682e-02 1.815424e-02 -5.942781e-03 -3.426783e-02 2.786980e-04 1.465090e-03 1.092864e-04 -1.989758e-05 1.961447e-03 2.404057e-03 1.864515e-03 -2.342970e-03 -1.394336e-03 1.911470e-03 -1.182858e-03 -6.819504e-04 -7.749228e-04
<VARIANCE> 39
 8.820943e+01 3.861458e+01 2.029625e+01 4.565988e+01 5.828498e+01 3.247619e+01 2.395605e+01 2.902121e+01 4.258837e+01 3.167321e+01 1.901695e+01 1.848357e+01 1.582491e+02 2.507793e+00 1.302295e+00 1.097857e+00 1.644960e+00 1.892603e+00 2.083208e+00 1.524675e+00 1.840137e+00 2.125722e+00 1.673089e+00 1.448895e+00 1.457264e+00 1.123323e+00 3.027864e-01 2.330465e-01 1.922042e-01 2.741396e-01 3.304867e-01 3.573498e-01 2.958795e-01 3.398049e-01 3.900207e-01 3.005882e-01 2.781184e-01 2.784989e-01 1.345709e-01
<GCONST> 1.084410e+02
<STATE> 4
<MEAN> 39
 -7.092237e+00 -3.793652e+00 -7.671155e-01 -2.177134e+00 -5.022529e+00 1.197599e+00 1.391658e+00 -4.522811e+00 2.475666e+00 -3.983630e+00 5.284266e-01 -1.634990e+00 4.749557e+01 -4.500959e-03 3.395595e-02 -3.449689e-03 -3.757998e-03 3.827242e-02 -1.681405e-03 1.282120e-02 1.411823e-02 -3.382706e-02 1.359682e-02 1.815424e-02 -5.942781e-03 -3.426783e-02 2.786980e-04 1.465090e-03 1.092864e-04 -1.989758e-05 1.961447e-03 2.404057e-03 1.864515e-03 -2.342970e-03 -1.394336e-03 1.911470e-03 -1.182858e-03 -6.819504e-04 -7.749228e-04
<VARIANCE> 39
 8.820943e+01 3.861458e+01 2.029625e+01 4.565988e+01 5.828498e+01 3.247619e+01 2.395605e+01 2.902121e+01 4.258837e+01 3.167321e+01 1.901695e+01 1.848357e+01 1.582491e+02 2.507793e+00 1.302295e+00 1.097857e+00 1.644960e+00 1.892603e+00 2.083208e+00 1.524675e+00 1.840137e+00 2.125722e+00 1.673089e+00 1.448895e+00 1.457264e+00 1.123323e+00 3.027864e-01 2.330465e-01 1.922042e-01 2.741396e-01 3.304867e-01 3.573498e-01 2.958795e-01 3.398049e-01 3.900207e-01 3.005882e-01 2.781184e-01 2.784989e-01 1.345709e-01
<GCONST> 1.084410e+02
<TRANSP> 5
 0.000000e+00 1.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00
 0.000000e+00 6.000000e-01 4.000000e-01 0.000000e+00 0.000000e+00
 0.000000e+00 0.000000e+00 6.000000e-01 4.000000e-01 0.000000e+00
 0.000000e+00 0.000000e+00 0.000000e+00 7.000000e-01 3.000000e-01
 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00
<ENDHMM>
```

* hmm0/vFloors
```
~v varFloor1
<Variance> 39
 8.820943e-01 3.861458e-01 2.029625e-01 4.565988e-01 5.828498e-01 3.247619e-01 2.395605e-01 2.902121e-01 4.258837e-01 3.167320e-01 1.901695e-01 1.848357e-01 1.582491e+00 2.507793e-02 1.302295e-02 1.097857e-02 1.644960e-02 1.892603e-02 2.083207e-02 1.524675e-02 1.840137e-02 2.125722e-02 1.673089e-02 1.448895e-02 1.457264e-02 1.123323e-02 3.027864e-03 2.330465e-03 1.922042e-03 2.741395e-03 3.304867e-03 3.573498e-03 2.958795e-03 3.398049e-03 3.900207e-03 3.005882e-03 2.781184e-03 2.784989e-03 1.345709e-03
```

(memo)  
   「-f」を付けないとvFloorsができない  



### STEP6 spモデルを作成する

### STEP7 spの位置を推定

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
