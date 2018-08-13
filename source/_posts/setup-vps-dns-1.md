---
title: 用 Vultr + Gandi 註冊網站（一）
visible: true
date: 2018-08-10 19:55:38
tags: 
- Gandi
- Vultr
- DNS
- VPS
categories:
- Note
---


其實之前就有想要動手架設自己的網域了，但礙於麻煩跟 S3 首年免費的情況下，一直遲遲不肯動手，雖然 S3 網址醜歸醜還堪用，在下手之前也做了很久的功課，像是各家 VPS 的比較，哪邊的機
房比較好，DNS 代管服務誰比較優等等，終於就在今天一個下午有同事的 support 下完成了整個大工程 🙌，趕快趁熱熟記這一路艱辛的筆記。

![](https://i.imgur.com/wUIr1QT.png)

<!-- more -->


## 什麼是 VPS ?

Virtual private server 簡稱，也就是「虛擬專用伺服器」，根據我的解讀，可以把它想像成你在雲端買了一台主機，然後這跟你在本機 run localhost server 一樣，要把他分享給別人的話只要把自己的 IP 換上去(e.g 128.0.0.0:3000) 就可以連到你的 server 瀏覽，而你買的這台 VPS 沒有區網的限制，他是一台一直開著的 server (也可以手動關閉)，每個人都可以透過他的 IP 連上，至於連上後會出現什麼畫面後面會說明怎麼設定。

## 什麼是 DNS ?

Domain Name System，我們俗稱的網域，如果說 IP 是我們在地圖上的經緯度，DNS 就是地標名稱；不知道 (121.564101,25.033493) 但是知道 台北 101 在哪裡，簡言之 DNS 就是把 IP 轉化為我們可以解讀的字串，省去背一大串數字的困擾。

## 該怎麼選擇 ?

基本上選擇這塊我個人是比較看感覺的😅 首先 VPS，做功課表示目前線上 VPS 三巨頭分別是：
* [Linode](https://www.linode.com/)
* [Vultr](https://www.vultr.com/)
* [DigitalOcean](https://www.digitalocean.com/)

Linode 一直以來都是比較常聽到的老牌子，但我就是反骨不喜歡跟大多數人用的一樣，所以選擇 Vultr，另一方面也是有點因為 V 意志✌️
![](https://i.imgur.com/cLoET5K.png)

而沒選擇 DO 的原因我記得好像是因為他在亞洲機房比較差，Linode 跟 Vultr 在東京的機房算是差不多，還有一點是看到同事用的 Linode 介面好羊村 ... 沒有美感第一眼就不想用他😒，Vultr 在方案部分也比其他加多出了 $2.5 的選擇，很適合剛入手想嚐鮮的人購買，要注意的是現在 $2.5 取消了 IPv4 的支援，全面使用 IPv6，所以選擇前請先[測試](https://test-ipv6.com/index.html.zh_TW)你的網路支不支援 IPv6 喔！
> IPv6 與 IPv4 最大的差異在於他的 IP 組合，我們常看到的數字組 123.123.1.12 是 IPv4，IPv6 則是像 2001:DB8:2de:0:0:0:0:e13，雖然版本跟進是不可避免的，但目前礙於支援度的問題還是先評估自己的需求比較好

值得一提的是，Vultr 不定期會推出註冊優惠，像我這波就是註冊即贈 $25 ，以 $5/month 來說等於送你五個月喔！

結論 <font style='font-weight: bold; color: #1364b4; font-size: 20px;'> Vultr </font> 勝出
當然 VPS 服務不只這三家，像是 Bandwagon、AWS、UpCloud、HostUs 等等的都可以比較看看

> <font style='font-size: 10px; color: grey;'>如果你也想用 Vultr 可以幫我點這個 👉[連結](https://www.vultr.com/?ref=7501517)👈 註冊</font>

<br>
DNS 也找了兩家代管做比較：
* [GoDaddy](https://tw.godaddy.com/)
* [Gandi](https://www.gandi.net/zh-hant)

目前聽到大部分都是以 Linode 搭配 GoDaddy 做一個套餐組合，但反骨的我依舊是選擇我們的印度聖雄 Gandi，其一同樣也是因為它的介面比較對我的胃口(以及 V 的綠色✌️)，而當下果斷出手的原因則是正值為期兩個月的 .tw 破盤價 $50/year ! 所以腦波一弱就順手買了，整體算下來開銷就是這 $50 算是相當好的起步啊 😬


## 購買 VPS

Vultr 的購買步驟相當易懂，跟著流程走 server 就莫名其妙完成了呢！
> 以下選項都可依個人喜好做調整

機房可以選東京或新加坡，機房越近好像會比較快的樣子
![](https://i.imgur.com/kxjAzx9.png)
因為同事都是用 Ubuntu 系統，這樣也比較好問
![](https://i.imgur.com/wBtPydv.png)
像上面所提到的，目前 $2.5 只有 IPv6 版本，所以選擇 $5 方案就夠用了
![](https://i.imgur.com/x08VTkr.png)
4 - 6 這邊我沒有做其他設定，如果事後想增加 IPv6 也可以從後台更改
![](https://i.imgur.com/zYSmyTq.png)
最後就是設定主機名稱
![](https://i.imgur.com/CZvSWfO.png)

整個設定完成後在你的主頁就會看到他正在啟動你的 Server
![](https://i.imgur.com/fq6KmOQ.png)
點進來後需要注意的是紅色區塊的資訊，這些就是要登入遠端 server 的帳號密碼
![](https://i.imgur.com/NxKOTJR.png)

再來打開 terminal 輸入下方指令後再輸入密碼
```shell
$ ssh root@123.123.1.12
```
其中 root 即上圖的 Username，Password 因為是亂數產生，用複製貼上會比較快(我不清楚密碼部份能不能手動更改)
到這邊我們就成功進入 server 了，接下來的步驟可以安裝 [Filezilla](https://filezilla-project.org/) 來操作會比較直覺

點選左上角的 站台管理員 並將設定如下(port 部分一律為 22)
![](https://i.imgur.com/5A0Vbcu.png)
登入後看到就是 server 內的資料夾的結構，到這邊 VPS 就算建置成功了！