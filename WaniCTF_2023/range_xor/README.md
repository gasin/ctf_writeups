# range_xor (misc)

## 問題（原文）
整数列`A`の任意の要素`a_i(0<=a_i<=1000,i=1,2...N)`に対して操作`f`を次のように定める

`f(a_i)=min(a_i, 1000-a_i)`

操作`f`を好きな回数行った後の整数列`B={b_1,b_2...b_N}`に対して

`X = b_1 xor b_2 xor ... xor b_N`

とするとき、`X`を最小にするような整数列`B`の種類数を`10^9+7`で割った余りを`FLAG`とする

## 解法
普通に競技プログラミングをする

状態は`2^10 > 1000`通りしかないので、前から順にその状態に到達できる通り数を管理してDPをすればよい

`a_i`が`500`以下の時は変更できず、`a_i`が`501`以上のときは`a_i`か`1000-a_i`に遷移できる

## Exploit
```python
dp = [0 for _ in range(1<<10)]

with open("input.txt", "r") as f:
    nums = [int(s) for s in f.readline().split()]

mod = pow(10,9) + 7

dp[0] = 1

for n in nums:
    dp2 = [0 for _ in range(1<<10)]
    for i in range(1<<10):
        if n <= 500:
            dp2[i] = dp[i^n]
        else:
            dp2[i] = (dp[i^n] + dp[i^(1000-n)]) % mod
    dp = dp2

for i in range(1<<10):
    if dp[i]:
        print(f"{i} {dp[i]}")
        break
```
