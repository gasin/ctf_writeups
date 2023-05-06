# EZDORSA_Lv1 (crypto)

## 問題概要
はじめまして！RSA暗号の世界へようこそ！

この世界ではRSA暗号と呼ばれる暗号がいたるところで使われておる！

それでは手始めに簡単な計算をしてみよう！

```
p = 3
q = 5
n = p*q
e = 65535
c ≡ m^e (mod n) ≡ 10 (mod n)
```
以上を満たす最小の`m`は何でしょう？

`FLAG{THE_ANSWER_IS_?}`の`？`に`m`の値を入れてください。

## Exploit 
```python
p = 3
q = 5
n = p*q
e = 65535

for m in range(n):
    if pow(m,e,n) == 10:
        print(f"FLAG{{THE_ANSWER_IS_{m}}}")
```
