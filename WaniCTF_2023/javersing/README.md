# javersing (rev)

## 問題設定
`jar`ファイルが与えられる
```
$ java -jar javersing.jar
Input password:
hoge
Incorrect...
```

## 解法
`jar`を展開して、`class`ファイルを http://www.javadecompilers.com/ に投げるとデコンパイルしてくれる

```java
import java.util.Scanner;

// 
// Decompiled by Procyon v0.5.36
// 

public class javersing
{
    public static void main(final String[] array) {
        final String s = "Fcn_yDlvaGpj_Logi}eias{iaeAm_s";
        boolean b = true;
        final Scanner scanner = new Scanner(System.in);
        System.out.println("Input password: ");
        final String replace = String.format("%30s", scanner.nextLine()).replace(" ", "0");
        for (int i = 0; i < 30; ++i) {
            if (replace.charAt(i * 7 % 30) != s.charAt(i)) {
                b = false;
            }
        }
        if (b) {
            System.out.println("Correct!");
        }
        else {
            System.out.println("Incorrect...");
        }
    }
}
```
やってることは単純なので、復元するコードを投げるとフラグが取れる

## Exploit
```python
from Crypto.Util.number import inverse

s = "Fcn_yDlvaGpj_Logi}eias{iaeAm_s"

flag = ""
for i in range(30):
    flag += s[i * inverse(7,30) % 30]

print(flag)
```
