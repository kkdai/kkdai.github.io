---
layout: post
title: "[Jekyll] 取決於要不要跳到Hugo，後來決定修改樣板"
description: ""
category: 
- jekyll
tags: 
---

## 本來是想要換到Hugo的

其實早就想換到[Hugo](https://gohugo.io/)了，新版的[Hugo](https://gohugo.io/)中的[Hugo Import Jekyll](https://gohugo.io/commands/hugo_import_jekyll/) 也算是順利．但是卡在permalink一直設定不成功．  先來改改Jekyll的theme，不然現在的已經醜到我有點受不了．

## 記錄下jekyll整合到hugo過程

- 記得要更新到Hugo 1.5之後，才有[Hugo Import Jekyll](https://gohugo.io/commands/hugo_import_jekyll/)
- 跑過之後，發現會有一堆error `Parse filename error: .DS_Store`
	- 解法: 使用最新的source code，需要有這個[fixed](https://github.com/spf13/hugo/issues/1705)


###  Hugo轉換目前遇到問題

我希望能把整個permalink跟原先的jeky來對應．成為`permalink: \:title\` 會一直無法修改permalink． 這邊卡了一下，決定把他弄好再轉到[Hugo](https://gohugo.io/)，先來換Jekyll的theme

後來問題發現是在`hugo jekyll import`裡面有預設的`url:`．也就是會跑成`/2016/02/02/title/`這樣，還是要找個方法來改改．

[更新 2016/02/22]
透過改code，現在可以透過import跑完所有jekyll文章也有正確的url. Refer [issue #1887](https://github.com/spf13/hugo/issues/1887)． 但是又卡在原先部落格的設定有問題，我使用了多個category (照理說category 只能有一個，複數個應該要用tag)，這邊就有點難改了.....

#### 剩下的問題:

-  `hugo import jekyll`: Categories在轉換的時候會掉(應該是issue 要修)
-  需要另外準備Archive跟Category 的頁面，就像接著我要在jekyll裡面準備的一樣．
-  找個一漂亮一點，實用一點的樣板．（就像女人的衣服，可能永遠少一個?)


## 來換 Jekyll 的 Theme

把theme 換成[dbyll](https://github.com/dbtek/dbyll)，簡潔我又喜愛． 不過由於本來有使用到JB (Jekyll Bootstrap)，所以要做一些修改．快速紀錄一下:


- 把JB的相關設定移掉(Remove specfic string in all file)

```
sed -i.bak '/include JB/d' *
rm *.bak
```

- 移除`layout: post` (Remove layout post)

```
sed -i.bak '/layout: post/d' *
rm *.bak
```


- 糟糕，錯誤移除．在用到gsed把他加回去(Oops, Use gsed  to insert it back)

```
sed -i "2i layout: post" *
```


- [dbyll](https://github.com/dbtek/dbyll)只有類別的分類，但是沒有(Add archive without plugin by month and year refer [here](http://stackoverflow.com/questions/19086284/jekyll-liquid-templating-how-to-group-blog-posts-by-year)) 程式碼列在後面．

<script src="https://gist.github.com/kkdai/d6563c00ab4336d3939f.js"></script>

- 發現試著要連`categories.html`跟`archive.html`會跳出404，原來要改成`/categories/`跟`/archive/`就好了．


- 發現到了 jekyll 3.0 原先的 `categories.html`會有問題，於是得把原先裡面的`categories.html`語法改一下．

<script src="https://gist.github.com/kkdai/e668c7d934257386374c.js"></script>

- 相同的東西也改了 `tags.html`

<script src="https://gist.github.com/kkdai/f2300bccb5cfc4adbf2f.js"></script>

這樣改...應該可以撐一下． 等我把Hugo問題修好.. 馬上換到Hugo...