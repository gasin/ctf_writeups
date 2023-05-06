# Guess (Misc)

## 問題設定
以下のようなサービスが与えられる
```
$ nc guess-mis.wanictf.org 50018

1: peep
2: guess
> 1
index> 1 2 3
[5028, 9468, 7043]

1: peep
2: guess
> 2
Guess the list> 2 3 4
Incorrect!
```

配布されたコードを見ると、10000個の数字の配列を15回のクエリ以内に特定する必要があることが分かる。

クエリでは、任意個の数値を送ることで、そのインデックスに存在する数値の列をシャッフルしたものが取得できる

## 解法
全ての数値のうち、インデックスの`i`bitが立っているもの一覧を取得することができるので、二分探索のようにして復元できる

## Exploit

```python
from pwn import *

conn = remote('guess-mis.wanictf.org', 50018)

def recvline():
    r = conn.recvline()
    # print(r)
    return r.decode()

ps = [0] * 10000

for i in range(14):
    recvline()
    recvline()
    recvline()
    asks = " ".join([str(x) for x in range(10000) if x & (1<<i)])
    conn.send(b"1\n")
    conn.send(asks.encode() + b"\n")
    r = recvline()[len("> index> ["):-len("]\n")].replace(",", "").split()
    ns = [int(x) for x in r]
    for n in ns:
        ps[n] += (1<<i)

ans = [-1] * 10000
for i, p in enumerate(ps):
    assert ans[p] == -1
    ans[p] = i

recvline()
recvline()
recvline()

conn.send(b"2\n")
asks = " ".join([str(a) for a in ans])
conn.send(asks.encode() + b"\n")

conn.interactive()
```
