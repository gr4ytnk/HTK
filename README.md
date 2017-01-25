## 「もしもし」という音声を認識させてみる

※入力となるものは太字、出力となるものは斜体とした  


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

HCompV -C **config_for_HCompV** -f 0.01 -m -S **trainlist.txt** -M *hmm0* **proto**

* config_for_HCompV

* proto

hmm0フォルダの下に「macro」、「proto」、「vFloors」が作られる

* hmm0/macro
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
~h "si"
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
~h "sja"
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
~h "sil"
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
~h "sp"
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
