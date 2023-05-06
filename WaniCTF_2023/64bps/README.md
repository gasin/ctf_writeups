# 64bps (web)

## 問題設定
大きいサイズのファイルの末尾にフラグが付与されている

`Dockerfile`(抜粋)
```Dockerfile
RUN cd /usr/share/nginx/html && \
    dd if=/dev/random of=2gb.txt bs=1M count=2048 && \
    cat flag.txt >> 2gb.txt && \
    rm flag.txt
```
ただし、以下のようにngixの通信速度がめちゃくちゃ遅いので単純に`curl`で取得することはできない

`ngix.conf`(抜粋)
```
    keepalive_timeout  65;
    gzip               off;
    limit_rate         8; # 8 bytes/s = 64 bps
```

## 解法
`curl`のオプションで、末尾のみ取得するというものがあるそれを使うとフラグが取れる

```curl -r -100 https://64bps-web.wanictf.org/2gb.txt```
