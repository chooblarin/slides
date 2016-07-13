class: center, middle, inverse
## Bayesian Statistics Introduction
@chooblarin

2016/07/14
---

class: inverse

# 3 steps in Parametric model

```
1. パラメータを含むモデルを設定する

2. モデルの評価基準を定める

3. 評価結果が最良となるパラメータをもとめる
```

---
class: inverse
## 評価する基準

- "誤差"を定義して，誤差が最小になるようパラメータを決める
- ある分布から「データセットが得られる確率」が最大になるようパラメータを決める（最尤推定法）
- それ以外の方法　-> *ベイズ推定*

---
class: inverse
## ベイズ推定の考え方

**分布のパラメータが確率的に揺らぐ**

最尤推定法ではパラメータの値は1つに決まるが，ベイズ推定の場合は分布で扱う（事後分布）．

---
class: inverse
## ベイズの定理を復習する (1)

ランダムにボールを1つ出る玩具がある．玩具の中には大きいボールと小さいボールが入っている．ボールの色は赤と白の2種類がある．

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/bayes/01random-box.png)

1. ボールの色が赤である確率は？
2. ボールが「大きい」とわかっている場合，それが赤である確率は？
3. 出たボールが「大きい赤」である確率は？

---
class: inverse
## ベイズの定理を復習する (2)

$$
(1) \;\; P({\rm Red})=\frac{7}{12}
$$

$$
(2) \;\; P({\rm Red}|{\rm Large})=\frac{1}{4}
$$

$$
(3) \;\; P({\rm Red}, {\rm Large})=\frac{1}{12}
$$

(2)は条件付き確率，(3)は同時確率．

$$
P({\rm Red}, {\rm Large})=P({\rm Red}|{\rm Large})P({\rm Large})=P({\rm Large}|{\rm Red})P({\rm Red})
$$

---
class: inverse

## ベイズの定理を復習する (2)

ベイズの定理

$$
P({\rm Y}|{\rm X})=\frac{P({\rm X}|{\rm Y})}{P({\rm X})}P({\rm Y})
$$

or

$$
P({\rm Y}|{\rm X})=\frac{P({\rm X}|{\rm Y})}{\sum_{Y'} P({\rm X}|{\rm Y'})P({\rm Y'})}P({\rm Y})
$$

（周辺確率の公式）
---
class: center, middle, inverse

# Thanks!

---
class: normal
## References
