# PGM-index

## 概要
- PLA-model(区分的線形近似モデル)
- 再帰的なindex構造

### PLA-modelについて
key k-->配列Aのpositionを誤差εで予測
誤差±εの範囲を二分探索すれば、正確な位置を求めることができる. log(ε)

### 再帰的なindex構造について
1つのセグメントが、定数時間と空間量で、任意の長さの配列をindexできることを活用
配列AのPLA-modelをkeysの部分集合とみなして、これからPLA-modelを作る
それのPLA-modelを作る
を繰り返す　

## PLA-modelについて
segment $s := (key_s,slope_s,intercept_s)$
とし、key kのpositionは、
$$f_s(k) = k*slope_s + intecept_s $$
で与えられる.
![alt text](images/image.png)
**Def 1.**
- A: ソート済み、keyはn個
- ε: 整数、１以上

ある$k_i,k_{i+r} \in \mathcal{U} $が存在して, $k_{i}\leq x \leq k_{i+r}$を満たす任意のxに対して
$|f_s(x)-rank(x)| \leq \epsilon$
が成り立つとき、

セグメントsが$[k_i,k_{i+r}]$の範囲のすべてのkeyをε-approximate indexingするという.

**Def 2.**
$piecewise$ $linear$ $\epsilon$ $- approximation$ $problem$は、セグメント{ $s_0,....,s_{m-1}$ }の数を最小化するPLA-modelを計算する問題

### 凸包
$piecewise$ $linear$ $\epsilon$ $- approximation$ $problem$は集合の要素の凸包を形成する問題に還元できる

集合$\lbrace ( k_i,rank(k_i) ) \rbrace$ $i = 0,...,n-1$に対して

- 高さ$2\epsilon$の長方形で囲める場合、$i$をインクリメント
- 高さが$2\epsilon$を超えると、構築を中止し、その矩形を分割する直線をセグメントとして決定する。
- 集合を空にして、残りの入力についてアルゴリズムを再開する

このgreedy algorithmは最適なセグメント数を求めることができ、線形時間と空間量を要する

今回の場合に言い換えると、

**Lem.1**

ある列$\lbrace (x_i,y_i) \rbrace$ (但し、$x$は広義単調増加)が与えられたとき、線形時間・空間量で、$y$を$\pm \epsilon$で近似する最小の数のセグメントを求めるアルゴリズムが存在する

dictionaryの場合、

$x_i$は$k_i$に対応し、$y_i$は$position$に対応する

### 1つのセグメントがカバーできるkeyの数について

**Lem.2**

直交座標系上の点の列$\lbrace (k_i,i) \rbrace$が与えられたとき、(但し、$x,y$軸について広義単調増加)
Lem.1のアルゴリズムは、$m_{opt} \le \frac{n}{2\epsilon}$を満たす、セグメント数$m_{opt}$を決定し、1つのセグメントは少なくとも$2\epsilon$個の点をカバーする

proof.
![alt text](images/image-2.png)
よって各セグメントは最低でも$2\epsilon$個のkeyをカバーする.

つまり、極論
segment $s = (k_i, 0,i+\epsilon)$とすれば, このセグメントは$\pm \epsilon$の誤差を供すると正しい.
分布に従った、直線を見つけてくると、よりたくさんのkeyをカバーできる.

### 対応するsegmentを求める

Lem.1のアルゴリズムは、入力Aの最適PLA-modelとしてセグメント列[$s_0,...,s_{m-1}$]を求める.

完全にindex可能な辞書問題を解くには,
query key $k$の 近似推定位置 $pos$ を求める segment $s_j$ を見つける必要がある

すなわち、
$$ s_j .key \le k$$
を満たす最大の $j$ である.

(理由:Lem.1のアルゴリズム)

### strategy
1. 列 $M$ を入力 $A$で構成する.
2. 各セグメントの先頭のkeyを取り出し、この取りだされたkeyの集合に対して、最適PLA-modelを構成する.
3. 以上を、セグメント数が1になるまで繰り返す

