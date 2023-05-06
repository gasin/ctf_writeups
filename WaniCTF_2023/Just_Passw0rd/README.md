# Just_Password (forensics)

## 問題設定
バイナリファイルが与えられる
```
$ file just_password
just_password: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=e990f2bb14ee6e4e795426f11da51bf989ef1152, for GNU/Linux 3.2.0, not stripped
```

## 解法
`strings`コマンドを叩くとフラグが見つかる

```
$ strings just_password | grep FLAG
FLAG is FLAG{1234_P@ssw0rd_admin_toor_qwerty}
```
