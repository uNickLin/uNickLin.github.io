---
title: 用 Vultr + Gandi 註冊網站（二）
visible: true
date: 2018-08-11 19:55:24
tags: 
- Gandi
- Vultr
- DNS
- VPS
categories:
- Note
---

![](https://i.imgur.com/wUIr1QT.png)

<!-- more -->


第一步我們完成了 Vultr 的購買及設定，接著要開始採購自己專屬的 DNS 了。

## 購買 DNS

首先先到 [Gandi](https://www.gandi.net/zh-hant) 檢查一下自己享用的名字能不能註冊，就像角色 ID 如果先被取走了就不能重複使用，只能等他過期換域名才有機會拿到，所以如果早晚都打算買網域的話，不如早點先把域名註冊下來！
![](https://i.imgur.com/6BM6ktz.png)

域名是採年費制，每次購買可以選擇要用幾年，期限到後可以續租，但要注意續租費用可能會有浮動！
順便推廣一下 Gandi 在 2018/8/1 ~ 2018/9/30 兩個月的期間有 .tw 特價 $50 的活動，我也跟風一波買了 .tw 當第一個域名，由於是特價活動就只能買一年的租約，後續看是要續租還是改其他域名了

購買流程不需要做什麼特別的設定，就跟平常網購一樣 🛒，買完後臺就可以看到熱騰騰的網域了！
![](https://i.imgur.com/1Rg5S4Z.png)


## 設定 DNS

點進網域後總覽可以看到剛剛購買的一些明細資訊，包括租期等等，這邊我們直接進到「區域檔紀錄」
![](https://i.imgur.com/I4GwaEw.png)

這個 table 就是我們最主要設定 Domain 導向的地方，試著新增一筆 DNS 把指向你的 IP
![](https://i.imgur.com/LnHW4db.png)

在網址上輸入你的 DNS 如果成功指向 IP server 應該會看到 'Welcome to Nginx' 的畫面，這樣就成功囉！