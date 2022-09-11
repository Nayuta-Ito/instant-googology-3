# 概要
第三回幻想巨大数へのエントリーです。

修正が多くなることが予想されるためバージョン管理にGitHubを使用します。

各修正のバージョンはコミットIDで一意に指定できます。

定義に対する修正案を提出したい場合はPRを作っていただいてもかまいません。

# エントリー

## 序文
0以上の整数のみを数として扱い、それ以外の数が必要になるときはその計算が「できない」、すなわち未定義とする。

ラテン文字小文字は数を表す変数である。

48個の計算可能関数い～す,京を以下のように定義する。その多くが単純な再帰により実装されていることに注意せよ。

再帰が必ず停止するとは限らないが、停止しない部分はこの巨大数のサポート範囲外(いわゆる非標準形)なので問題ない。

## 関数たち
### い
い(a,b) =
* b+1 (a=0のとき)
* い(a-1,b+2) (a>0のとき)

### ろ
ろ(a,b,c) =
* ろ(a,b+1,c-1) (c>0のとき)
* ろ(a-1,b,b) (c=0かつa>0のとき)
* b (c=0かつa=0のとき)

### は
は(a,b) =
* ろ(a,い(b,0),0)

### に
に(a,b,c) =
* に(a-1,b+1,c) (a>bかつa>0のとき)
* に(a,0,c+1) (a=bかつa>0のとき)
* c (a<bまたはa=0のとき)

「a>bかつa>0」は「a>b」と論理的に同値ですが、場合分けが包括的であることをわかりやすくするためにこうしている。

### ほ
ほ(a,b,c) =
* ほ(a-1,b+1,c) (a>bかつa>0のとき)
* ほ(a,0,c+1) (a=bかつa>0のとき)
* a (a<bまたはa=0のとき)

「a>bかつa>0」は「a>b」と論理的に同値ですが、場合分けが包括的であることをわかりやすくするためにこうしている。

### へ
へ(a,b,c,d) =
* 0 (b=0のとき)
* へ(a,b,c+1,0) (d>=b>0のとき)
* へ(a-1,b,c,d+1) (d<bかつa>0かつb>0のとき)
* は(c,d) (d<bかつa=0かつb>0のとき)

### と
と(a,b) = 
* 1 (ほ(へ(a,b,0,0),0,0)=0かつb>0のとき)
* 0 (ほ(へ(a,b,0,0),0,0)>0またはb=0のとき)

### ち
ち(a,b,c) = 
* ち(a,b-1,c+と(a,b)) (b>0のとき)
* c (b=0のとき)

### り
り(a) = 
* 1 (に(ち(a,a,0),0,0)=1かつほ(ち(a,a,0),0,0)=0のとき)
* 0 (それ以外のとき)

### ぬ
ぬ(a,b) = 
* b (り(b)>0かつa=0のとき)
* ぬ(a,b+1) (り(b)=0かつa=0のとき)
* ぬ(a-1,b+1) (り(b)>0かつa>0のとき)
* ぬ(a,b+1) (り(b)=0かつa>0のとき)

### る
る(a,b,c,d,e,f,g) =
* 0 (c=0かつa=0のとき)
* る(a,b,ぬ(b,0),d,e,f,g) (c=0かつa>0のとき)
* る(a,b,c,d,e,c,g) (c>0かつa>0かつe=0かつf=0のとき)
* る(a,b,c,d+1,0,f,g) (c>0かつa>0かつe>0かつf=0のとき)
* る(a-1,b,c,d,e+1,f-1,g) (c>0かつa>0かつf>0のとき)
* る(a,b,c,d+1,0,f,g) (c>0かつa=0かつe>0かつf=0のとき)
* る(d,b,c,0,0,0,g+1) (c>0かつa=0かつe=0のとき)
* g (c>0かつa=0かつe>0かつf>0のとき)

### を
を(a,b) =
* 0 (b=0のとき)
* b (b>0かつほ(へ(a,ぬ(b,0),0,0),0,0)=0のとき)
* を(a,b-1) (それ以外のとき)

### わ
わ(a,b) =
* b+1 (る(a,b,0,0,0,0,0)<る(a,を(a,a),0,0,0,0,0)のとき)
* 0 (る(a,b,0,0,0,0,0)>=る(a,を(a,a),0,0,0,0,0)かつb=0のとき)
* わ(a,b-1) (る(a,b,0,0,0,0,0)>=る(a,を(a,a),0,0,0,0,0)かつb>0のとき)

### か
か(a,b,c,d) =
* a (b=0のとき)
* か(a,b,ぬ(d,0),d) (a<=1かつb>0かつc=0のとき)
* か(a,b,ぬ(を(a,a)+1+d,0),d) (a>1かつb>0かつc=0のとき)
* か(a\*c,b-1,c,d) (b>0かつc>0のとき)

### よ
よ(a) =
* に(へ(a,ぬ(を(a,a),0),0,0),0,0)

### た
た(a,b,c,d,e) = 
* d (b>cのとき)
* た(a,b+1,c,d,e+1)　(b<=cかつる(a,b,0,0,0,0,0)=0のとき)
* た(a,b+1,c,か(d,る(a,b,0,0,0,0,0),0,e),0) (b<=cかつる(a,b,0,0,0,0,0)>0のとき)

### れ
れ(a,b,c) =
* a (b<=1のとき)
* れ(a,た(b,1,を(b,b),1,0),c+1) (に(b,0,0)=0かつb>1のとき)
* れ(か(a,に(b,0,0),0,c),た(b,1,を(b,b),1,0),0) (に(b,0,0)>0かつb>1のとき)

### そ
そ(a,b,c,d,e) =
* a (a<=1のとき)
* そ(a,b,よ(a),d,e) (a>1かつc=0のとき)
* そ(a,b,c,た(c,わ(a,a),を(a,a),1,を(a,a)-を(c,c)),b) (a>1かつc>0かつd=0のとき)
* そ(a,b,れ(c,d,0),d,e-1) (a>1かつc>0かつd>0かつe>0のとき)
* c (a>1かつc>0かつd>0かつe=0のとき)

## これ以下は使用しない。壊れてるから

### つ
つ(a,b,c,d) =
* つ(a,わ(a,a)+1,c,d) (b=0のとき)
* つ(a,b,る(a,を(a,a),0,0,0,0,0),d) (b>0かつc=0のとき)
* c (b=1かつc>0のとき)
* つ(a,b,c,る(a,b,0,0,0,0,0)) (b>1かつc>0かつd=0のとき)
* d-c (b>1かつc>0かつd>0のとき)

### ね
ね(a,b,c) =
* ね(a\*ぬ(c-1,0),b,c-1) (c>0のとき)
* ね(a,b-1,を(a,a)+1) (c=0かつb>0のとき)
* a (b=0かつc=0のとき)

### な
な(a,b,c,d,e,f) =
* a (a<=1のとき)
* な(a,b,よ(a),d,e,f) (a>1かつc=0のとき)
* な(a,b,c,た(c,わ(a,a),を(a,a),を(a,a)-を(c,c),0),e,b) (a>1かつc>0かつd=0のとき)
* な(a,b,c,ね(d,つ(d,0,0,0),0),つ(d,0,0,0),f) (a>1かつc>0かつd>0かつe=0のとき)
* な(a,b,れ(c,d,0),ね(d,e,0),e,f-1) (a>1かつc>0かつd>0かつe>0かつf>0のとき)
* c (a>1かつc>0かつd>0かつe>0かつf=0のとき)

## 巨大数
京(x,y) =
* 京(そ(x,y,0,0,0),y+1) (x>1のとき)
* y (x<=1のとき)

とする。このとき、京(5000兆,10)を「この中で禁止できるものがあるならしてみろ!」(英語名: Ban any of these if you dare!)とする。
