# fermat (reversing)

## 問題概要
バイナリが与えられる

実行すると以下のようになる
```
$ ./fermat
Input a> 1
Input b> 2
Input c> 3
(a, b, c) = (1, 2, 3)
Invalid value :(
```

## 解法
ghedraでデコンパイルをすると以下のようなチェックがされていることが分かる
```c
undefined8 check(uint param_1,uint param_2,uint param_3)

{
  undefined8 uVar1;
  
  if (((param_1 < 3) || (param_2 < 3)) || (param_3 < 3)) {
    uVar1 = 0;
  }
  else if (param_1 * param_1 * param_1 + param_2 * param_2 * param_2 == param_3 * param_3 * param_ 3)
  {
    uVar1 = 1;
  }
  else {
    uVar1 = 0;
  }
  return uVar1;
}
```

フェルマーの最終定理だから無理にみえるが、`uint32`型であるから、それぞれを3乗したら0になる数にしてあげればチェッカーを通せる

```
$ ./fermat
Input a> 1048576
Input b> 1048576
Input c> 1048576
(a, b, c) = (1048576, 1048576, 1048576)
wow :o
FLAG{you_need_a_lot_of_time_and_effort_to_solve_reversing_208b47bd66c2cd8}
```
