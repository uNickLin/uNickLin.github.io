---
title: VS Code terminal 版本錯誤
visible: true
date: 2018-06-07 18:00:03
tags:
- VS Code
- nvm
categories:
- Tricks
---

公司電腦一直有一個問題困擾我，就是 VS Code 裡面的 node 版本，當初安裝時是使用 v6.11.0，但現在在自己電腦上都已經更新到 v8.9.4 了，雖然可以透過 nvm use 8 來改變，但每次建立專案要是沒有切換到就要多等一個錯誤時間，之前也嘗試過設定預設版本未果。

<!--more-->

根據 google 結果，要設定預設版本只要輸入指令
```shell
$ nvm alias default 8
```

這時候電腦就會自動使用 8.9.4 了，這個方法在 OS terminal 上沒問題，就是進了 VS Code 都會跳回 6.11.0 🤬
就在剛才發現每次開 VS Code 都會有一條被我忽略的訊息

```shell
nvm is not compatible with the npm config "prefix" option: currently set to "/usr/local"
Run `npm config delete prefix` or `nvm use --delete-prefix v8.9.0 --silent` to unset it.
```

可想而知，這麼明確的訊息我兩個都 run 過了就是沒反應，最後輾轉了好幾個 stackoverflow 才找到[神解](https://github.com/creationix/nvm/issues/1652)
<small color='grey'>(還找了一下怎麼開 ./bash_profile)</small>

```shell
$ touch ~/.bash_profile; open ~/.bash_profile
```
這時候會開啟文字編輯器，只要在最上面加入
```shell
PATH="/usr/local/bin:$(getconf PATH)"
```

存擋，完成，收工 🤘
