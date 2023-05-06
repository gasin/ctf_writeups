# Apocalypse (forensics)

## 問題設定
壊れたpngファイルが渡されるので、ここからフラグを抽出したい

![chall](chall.png)

## 解法
以下のようにCRCが壊れているらしいので、自動で修復してくれるツールをいくつか試したが、上手くいかなかった
```bash
$ pngcheck chall.png
chall.png  CRC error in chunk IDAT (computed 0699358e, expected cda0e401)
```

ググって出てきた https://online.officerecovery.com/jp/pixrecovery/ というサイトに画像を投げたら、うっすらフラグが見えたので、目を凝らしながら何とか復元した。

![demo](demo.png)

（これを僕はなかなか読めなくて別の方針を探してました）
