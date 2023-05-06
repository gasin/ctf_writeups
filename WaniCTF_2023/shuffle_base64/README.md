# shuffle_base64 (misc)

## 問題設定
フラグをゴニョゴニョした上で`base64`に変換した結果とスクリプトが与えられる

## 解法
スクリプトを見ながら、ゴニョゴニョを復元していく

フラグが短く、処理も単純なので手作業できる

```python
import base64

cipher = 'fWQobGVxRkxUZmZ8NjQsaHUhe3NAQUch'

shuffled = base64.b64decode(cipher)
print(shuffled)
# }d(leqFLTff|64,hu!{s@AG!
# }d( leq FLT ff| 64, hu! {s@ AG!
# }d le FL ff 64 hu {s AG
# FLAG{s le ff 64 hu }
# FLAG{shuffle64}
```
