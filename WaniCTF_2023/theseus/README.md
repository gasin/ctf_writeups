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
