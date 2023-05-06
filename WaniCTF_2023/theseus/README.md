# theseus (reversing)

## 問題設定
バイナリが与えられる
```
$ file chall
chall: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=bf4f26e63e621e4750dd3448a1a8fcfa6d1e6453, for GNU/Linux 3.2.0, not stripped
$ ./chall
Input flag: hoge
Incorrect.
```

## 解法
デコンパイラで`main`関数を見ると、ところどころデコンパイルに失敗している

失敗しているところは無視して処理を追っていくと、以下のような関数で入力のチェックをしていることがわかる
```c
bool compare(char param_1,int param_2)

{
  long in_FS_OFFSET;
  undefined8 local_38;
  undefined8 local_30;
  undefined8 local_28;
  undefined2 local_20;
  undefined local_1e;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_38 = 0x41456c6d47414c46;
  local_30 = 0x5f662c60692e6866;
  local_28 = 0x24635e573f72294e;
  local_20 = 0x786b;
  local_1e = 0;
  if (*(long *)(in_FS_OFFSET + 0x28) != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return param_1 == *(char *)((long)&local_38 + (long)param_2);
}
```

gdbで処理を追っていくとスタックにフラグが格納されるタイミングがあるのでそれを取る

```
0x007fffffffe060│+0x0000: 0x0000000000000000     ← $rsp
0x007fffffffe068│+0x0008: 0x0000004600000000
0x007fffffffe070│+0x0010: "FLAG{vKCsq3jl4j_Y0uMade1"
0x007fffffffe078│+0x0018: "sq3jl4j_Y0uMade1"
0x007fffffffe080│+0x0020: "Y0uMade1"
0x007fffffffe088│+0x0028: 0x0000000000000000
0x007fffffffe090│+0x0030: 0x0000000000000000
0x007fffffffe098│+0x0038: 0x2406165931708700
```