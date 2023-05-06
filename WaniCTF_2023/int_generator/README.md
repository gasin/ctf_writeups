# int_generator (misc)

## 問題概要
数値をごにょごにょ変換するスクリプトが与えられ、その出力結果が与えられるので入力を復元する問題

## 解法
入力に対してまず以下のような関数`f`が走る
```python
k = 36
maxlength = 16


def f(x, cnt):
    cnt += 1
    r = 2**k
    if x == 0 or x == r:
        return -x, cnt
    if x * x % r != 0:
        return -x, cnt
    else:
        return -x * (x - r) // r, cnt
```

これは`x*x`が`r`で割り切れるかどうかで処理が分岐するが、`x*x`が`r`で割り切れるということは`x`が`2^18`の倍数ということなので`x`の全探索が可能になる

実際にそのようなケースを全探索すると、今回与えられた出力は全部`x^18`の倍数の入力から生成されたことが分かり、フラグが復元できる

(個人的にはそうでない場合も復元処理書いたのだが....)

## Exploit
```python
import random

k = 36
maxlength = 16


def f(x, cnt):
    cnt += 1
    r = 2**k
    if x == 0 or x == r:
        return -x, cnt
    if x * x % r != 0:
        return -x, cnt
    else:
        return -x * (x - r) // r, cnt

def rev_f(x):
    r = 2**k
    if x == 0 or x == -r:
        return x


def g(x):
    ret = x * 2 + x // 3 * 10 - x // 5 * 10 + x // 7 * 10
    ret = ret - ret % 2 + 1
    return ret, x // 100 % 100

def rev_g(x, cnt):
    st = 0
    en = 2**(k-1)
    while en-st > 1:
        mid = (st+en) // 2
        if g(mid)[0] <= x:
            st = mid
        else:
            en = mid
    for i in range(st-100,st+100):
        if g(i) == (x, cnt):
            return i
    
    return -1


def digit(x):
    cnt = 0
    while x > 0:
        cnt += 1
        x //= 10
    return cnt


def pad(x, cnt):
    minus = False
    if x < 0:
        minus = True
        x, cnt = g(-x)
    #print(f"x: {x}, cnt: {cnt}, minus: {minus}")
    sub = maxlength - digit(x)
    ret = x
    for i in range(sub - digit(cnt)):
        ret *= 10
        if minus:
            ret += pow(x % 10, x % 10 * i, 10)
        else:
            ret += pow(i % 10 - i % 2, i % 10 - i % 2 + 1, 10)
    #print(cnt)
    ret += cnt * 10 ** (maxlength - digit(cnt))
    return ret


def int_generator(x):
    ret = -x
    x_, cnt = f(x, 0)
    #print(f"x_: {x_}, cnt: {cnt}")
    while x_ > 0:
        ret = x_
        x_, cnt = f(x_, cnt)
        #print(f"x_: {x_}, cnt: {cnt}")
    return pad(ret, cnt)

def simple_generator(x):
    x, cnt = g(x)
    sub = maxlength - digit(x)
    ret = x
    print(f"ret: {ret}")
    for i in range(sub - digit(cnt)):
        ret *= 10
        ret += pow(x % 10, x % 10 * i, 10)
    print(f"ret: {ret}, cnt: {cnt}")
    ret += cnt * 10 ** (maxlength - digit(cnt))
    return ret

def rev_simple_generator(_x):
    for cnt_len in [1,2]:
        for sub_len in range(5):
            x = _x
            cnt = x // (10**(maxlength-cnt_len))
            x -= cnt * (10**(maxlength-cnt_len))
            x //= (10**sub_len)
            print(f"x: {x}, cnt: {cnt}")
            ans = rev_g(x, cnt)
            if ans == -1:
                continue

            return ans
    
    return -1


num1 = random.randint(0, 2 ** (k - 1))
num2 = random.randint(0, 2 ** (k - 1))
num3 = random.randint(0, 2 ** (k - 1))
num3 = (1<<20)

# print(f"num1: {num1}")
# print("int_generator(num1):{}".format(int_generator(num1)))
# print("simple_generator(num1):{}".format(simple_generator(num1)))
# print(rev_simple_generator(int_generator(num1)))
# 
# print(f"num2: {num2}")
# print("int_generator(num2):{}".format(int_generator(num2)))
# print("simple_generator(num2):{}".format(simple_generator(num2)))
# print(rev_simple_generator(int_generator(num2)))

print(f"num3: {num3}")
print("int_generator(num3):{}".format(int_generator(num3)))
print("simple_generator(num3):{}".format(simple_generator(num3)))
print(rev_simple_generator(int_generator(num3)))

flag1 = 1008844668800884
flag2 = 2264663430088446
flag3 = 6772814078400884

print(f"FLAG{{{rev_simple_generator(flag1)}_{rev_simple_generator(flag2)}_{rev_simple_generator(flag3)}}}")

for i in range(1<<18):
    x = int_generator(i * (1<<18))
    if x == flag1:
        print(f"flag1: {i*(1<<18)}")
    if x == flag2:
        print(f"flag2: {i*(1<<18)}")
    if x == flag3:
        print(f"flag3: {i*(1<<18)}")
```
