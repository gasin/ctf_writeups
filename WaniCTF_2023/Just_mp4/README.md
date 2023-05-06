# Just_mp4 (forensics)

## 問題設定
`mp4`ファイルが与えられる

## 解法
動画を見ても何も有益そうな情報はない

`exiftool`の情報を見ていたらそれっぽい情報があり、`base64`デコードするとフラグが取れる
```
$ exiftool chall.mp4 | grep flag
Publisher                       : flag_base64:RkxBR3tINHYxbl9mdW5fMW5uMXR9
$ echo RkxBR3tINHYxbl9mdW5fMW5uMXR9 | base64 -d
FLAG{H4v1n_fun_1nn1t}
```
