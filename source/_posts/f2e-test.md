---
title: 來了解一下前端測試
visible: true
date: 2018-07-16 15:04:07
tags:
- cypress
- jest
categories:
- Note
---


被 Typescript 弄到 ㄎㄧ 肖後🤬，突然想學測試，因為俗話說
> 測試做得好，完全不怕型別鬧；
> 型別一旦鬧，測試就是寫到老。

根據我調查，前端分為兩種測試：
1. end to end test
2. unit test

既然要學就兩個都學一點吧！

<!--more-->

## E2E test

第一個 E2E(end to end) test 其實就是指畫面 UI 的測試，像是點擊後哪個 element 必須顯示/消失、call api 後的資料預期長度以及各種 element 的表現都可以經由 e2e 抓出來。

比較常見的應該是 Nightwatch.js ，還記得在建立 vue-cli(2.x) 時會有一條是
![](https://i.imgur.com/Uj4qitQ.png)
但詳細比較後我選擇使用 [Cypress](https://www.cypress.io/) 而非 Nightwatch，因為 Nightwatch 需要的附件太多了 😐(Selenium 跟 Phantom 還是其他有的沒的 ... )，而 Cypress 就是 Cypress 很容易上手！

這個時間點剛好可以用在最近六角活動的 (Filter demo)[https://github.com/uNickLin/vue-filter-demo] 上，要搞懂它的 api 確實是需要一點時間，但大致上有點類似 jQuery 的語意，加上一個口令一個動作的語法也增加可讀性，repo 裡面只稍微點幾個測試項目，Cypress 也有內建 example 的測試資料夾供參考喔！

## Unit test

再來就是單元測試啦，鑑於近幾年的網頁模式風向逐漸轉向前端，因此面對一大坨的邏輯處理，單元測試應該會越來越被重視，會接觸單元測試的原因其實是因為有一次同事在寫 async await 時遇到 localstorage 存取速度不一的問題，導致發生錯誤還要碰運氣，只能靠不停的 F5 來偵錯 😓，當時才想說「要是我會寫測試就可以讓他自動 refresh 100 次來看到底會不會錯」，於是便踏進了 unit test 的領域，跟 e2e 一樣，vue-cli 也有內推的 unit test
![](https://i.imgur.com/yda0eWA.png)

由於在這之前蠻常聽到 mocha 跟 jasmine ，就從他們先下手了解，但據我所知跟 Nightwatch 一樣太多東西要相輔相成了 ... 於是矛頭轉向 Facebook 的 Jest(好像也可以測試 UI ?)，優點如下

* 內置Jasmin語法
* 內置auto mock
* 自帶mock API
* 前端友好（集成JSDOM）
* 支持直接使用Promise和async/await書寫異步代碼
* 支持對 React 組件進行快照監控
* 擴展和集成 Babel 等常用工具集也很方便
* 自動環境隔離

當然我自己也只試過 jest，所以大部分我都不懂是啥意思 😅，選擇的原因只是單純因為
1. 使用面向多元
2. 有 FB 背書

去年在 ALPHA Camp 學 ROR 時也有學到一點測試，unit test 就跟當時寫的東西很雷同，現在有點邏輯基礎了也比較了解單元測試的用意及語法，但實際使用上其實也是點點水，還沒摸到很深入的功能，同樣的 api 也是要花點功夫，尤其一次學 e2e test 跟 unit test ，彼此的 api 都記得很亂 ... ，大型一點的專案或是長期開發的產品還有機會用到 CI ，CI 也是測試學完後的目標，之後看有沒有機會在自己的 side project 上實戰囉！