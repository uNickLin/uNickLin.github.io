---
title: 用 Vultr + Gandi 註冊網站（三）
visible: true
date: 2018-08-11 23:09:16
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

買完 VPS 跟 DNS 就要來把兩個合併了，要讓我們的 Domain 直接連到你的頁面上，專案用 Vue 來舉 🌰 

首先先在我們的 server 上安裝 nginx

> nginx 可以把它視為幫你負責路由導向的人，以我的域名為例，當它收到 unick.tw 這個網址過來時要讓他打開首頁資料夾裡面的檔案呈現給 browser

```shell
$ apt-get install nginx
```

因為 nginx 讀檔有他特定的路徑放資料夾，雖然很臭長還是要記 ... 有兩個地方分別是
/etc/nginx/sites-available : 主要放設定檔的地方
/etc/nginx/sites-enabled : 設定檔會被編譯放到這

所以我們要做的就是把設定檔寫好，執行一道神秘的指令讓 server 把它轉進 sites-enabled

到這我們先說明幾個指令：
```shell
<!-- vim 修改文檔 filename -->
$ nano [filename]

<!-- 退出 -->
ctrl + X

<!-- 儲存 -->
ctrl + X > ENTER
```

```shell
<!-- 登錄路由檔案 filename -->
$ sudo ln -s /etc/nginx/sites-available/[filename] /etc/nginx/sites-enabled
```

```shell
<!-- 重開機 -->
$ sudo service nginx restart
```

在 /etc/nginx/sites-available 裡面可以看到他有一個預設的 default 可以參考設定，這時候如果你有裝 Filezilla 可以在自己電腦寫完後再拉 ftp ，又或者直接用 terminal 新增檔案並用 nano 編輯

```shell
server {
  listen 80;              // 基本上應該不會動到這個
  root /home/page;        // 視你的檔案位置而定
  index index.html;       // 要開那個檔案
  server_name unick.tw;   // 當來源的 Domain 等於這個就打開上述的檔案
}
```
儲存好後執行上面那行
```shell
$ sudo ln -s /etc/nginx/sites-available/[filename] /etc/nginx/sites-enabled
```
接著重啟 server

```shell
$ sudo service nginx restart
```

這樣你的 DNS 就可以正確地開啟網頁了 ！

-----------------------------------------

後記：

深入一點還有 sub domain 的設定，就是可以讓你一個 Domain 當多個用（當然主要 domain 都不會變）以及子資料夾的路由設定，像是
sub domain: sub.unick.tw
sub folder: unick.tw/anotherfile

這部分等我實作過後再來更新吧

總之有一個自己網站了，真爽！