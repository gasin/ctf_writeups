# what_happening (forensics)

## 問題設定
以下のようなファイルを与えられる
```
$ file updog
updog: ISO 9660 CD-ROM filesystem data 'ISO Label'
```

## 解法
`binwalk -e updog`を叩くとフラグを含む画像が出力された

![flag](flag.png)
