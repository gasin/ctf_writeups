# Lua (reversing)

## 問題概要
難読化が施されている`Lua`ファイルが与えられる
```
$ lua main.lua
Input FLAG : a
Incorrect
```

## 解法
それっぽい箇所に`print`を入れて出力を見たら以下のような出力が得られた
```lua
local CRYPTEDlIIllIII = "NGI2d3Q8YSp3KmsvYWc9K0c6dw=="
  local CRYPTEDlIIlIIlI = function(a, b)
    local c = CRYPTEDlIIlIlIl(CRYPTEDlIIlIllI(a))
    local d = c["\99\105\112\104\101\114"](c, CRYPTEDlIIlIllI(b))
    print(c) -- <- これを追加
    print(d) -- <- これを追加
    return CRYPTEDlIIlIllI(d)
  end
  local CRYPTEDlIIllIll =
    "\97\121\107\116\88\49\78\108\75\108\112\53\99\106\86\111\100\106\111\114\78\107\66\79\77\119\61\61"
  local CRYPTEDlIIllIll =
    "\97\121\107\116\88\49\78\108\75\108\112\53\99\106\86\111\100\106\111\114\78\107\66\79\77\119\61\61"
  local CRYPTEDlIIlIIII = "OS5nRkJxRlY8XydZaSZ2OXdEb3t7I2EkNmcvbyxdZVYvZy86Mjg="
  function CRYPTEDlIIlIlll(a, b)
    local c = CRYPTEDlIIlIllI(a, b)
    local d = CRYPTEDlIIllIlI
    return c, d
  end
  return CRYPTEDlIIlIlII(CRYPTEDlIIlIIlI(CRYPTEDlIIllIll, CRYPTEDlIIlIIIl), getfenv(0))()
```
```
$ lua main.lua
table: 0x55c5a1b9d1c0
G0x1YVEAAQQIBAgABQAAAAAAAABnZ195AAAAAAAAAAAAAAACBBQAAAAFAAAABkBAAEGAAAAcQAABBQAAAAbAQAALAEEAgUABAByAgAFBgAEAF0AAABbAAICFwAEAwQACAJxAAAEWgACAhcABAMFAAgCcQAABHgCAAAoAAAAEAwAAAAAAAABpbwAEBgAAAAAAAAB3cml0ZQAEDgAAAAAAAABJbnB1dCBGTEFHIDogAAQGAAAAAAAAAHN0ZGluAAQFAAAAAAAAAHJlYWQABAYAAAAAAAAAKmxpbmUABEMAAAAAAAAARkxBR3sxdWFfMHJfcHk0aDBuX3doNHRfZDBfeTB1XzNheV93NGVuXzQza2VkX3doMWNoXzBuZV8xc19iZTQ0ZXJ9AAQGAAAAAAAAAHByaW50AAQIAAAAAAAAAENvcnJlY3QABAoAAAAAAAAASW5jb3JyZWN0AAAAAAAUAAAAAQAAAAEAAAABAAAAAQAAAAIAAAACAAAAAgAAAAIAAAACAAAAAwAAAAQAAAAEAAAABQAAAAUAAAAFAAAABQAAAAcAAAAHAAAABwAAAAgAAAACAAAAAgAAAAAAAABhAAkAAAATAAAAAgAAAAAAAABiAAoAAAATAAAAAAAAAA==
Input FLAG : a
Incorrect
```
最後に`==`があることから、おそらく`base64`なので、デコードしてみたらフラグが含まれていた
```
$ echo G0x1YVEAAQQIBAgABQAAAAAAAABnZ195AAAAAAAAAAAAAAACBBQAAAAFAAAABkBAAEGAAAAcQAABBQAAAAbAQAALAEEAgUABAByAgAFBgAEAF0AAABbAAICFwAEAwQACAJxAAAEWgACAhcABAMFAAgCcQAABHgCAAAoAAAAEAwAAAAAAAABpbwAEBgAAAAAAAAB3cml0ZQAEDgAAAAAAAABJbnB1dCBGTEFHIDogAAQGAAAAAAAAAHN0ZGluAAQFAAAAAAAAAHJlYWQABAYAAAAAAAAAKmxpbmUABEMAAAAAAAAARkxBR3sxdWFfMHJfcHk0aDBuX3doNHRfZDBfeTB1XzNheV93NGVuXzQza2VkX3doMWNoXzBuZV8xc19iZTQ0ZXJ9AAQGAAAAAAAAAHByaW50AAQIAAAAAAAAAENvcnJlY3QABAoAAAAAAAAASW5jb3JyZWN0AAAAAAAUAAAAAQAAAAEAAAABAAAAAQAAAAIAAAACAAAAAgAAAAIAAAACAAAAAwAAAAQAAAAEAAAABQAAAAUAAAAFAAAABQAAAAcAAAAHAAAABwAAAAgAAAACAAAAAgAAAAAAAABhAAkAAAATAAAAAgAAAAAAAABiAAoAAAATAAAAAAAAAA== | base64 -d
uaQgg_y@@A�@�@
                         A�@��A�@������@�����@�@�
iowriteInput FLAG : stdinread*lineCFLAG{1ua_0r_py4h0n_wh4t_d0_y0u_3ay_w4en_43ked_wh1ch_0ne_1s_be44er}printCorrect
Incorrecta    b
```
