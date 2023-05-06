# web_assembly (reversing)

## 問題設定
ユーザー名とパスワードを聞いてくるだけのwebサイトが与えられる

問題名にあるように、`wasm`で実装されているが、難読化されているのかかなり読みにくい

## 解法
`wadt`というツールを使って`wasm`を`c`や`wat`形式に変換して処理を追おうとしたが、難しかった

ディベロッパーツールでwasmのデバッグ実行ができることがわかり、それも行っていたが、それでも処理を追うのはしんどかった

諦めてデコンパイルした`wasm`を眺めていると、以下のようなデータが見える
```
data d_3rinfinityFebruaryJanuaryJul(offset: 65536) =
  "3r!}\00infinity\00February\00January\00July\00Thursday\00Tuesday\00Wed"
  "nesday\00Saturday\00Sunday\00Monday\00Friday\00May\00%m/%d/%y\004n_3x\00"
  "-+   0X0x\00-0X+0X 0X-0x+0x 0x\00Nov\00Thu\00unsupported locale for st"
  "andard input\00August\00Oct\00Sat\000us\00Apr\00vector\00October\00Nov"
  "ember\00September\00December\00ios_base::clear\00Mar\00p_0n_Br\00Sep\00"
  "3cut3_Cp\00%I:%M:%S %p\00Sun\00Jun\00Mon\00nan\00Jan\00Jul\00ll\00Apri"
  "l\00Fri\00March\00Aug\00basic_string\00inf\00%.0Lf\00%Lf\00true\00Tue\00"
  "false\00June\00Wed\00Dec\00Feb\00Fla\00ckwajea\00%a %b %d %H:%M:%S %Y\00"
  "POSIX\00%H:%M:%S\00NAN\00PM\00AM\00LC_ALL\00LANG\00INF\00g{Y0u_C\00012"
  "3456789\00C.UTF-8\00.\00(null)\00Incorrect!\00Pure virtual function ca"
  "lled!\00Correct!! Flag is here!!\00feag5gwea1411_efae!!\00libc++abi: \00"
```

目を凝らすと、フラグっぽいものの断片が見えるので、復元できる

`Flag{Y0u_C4n_3x3cut3_Cpp_0n_Br0us3r!}`
