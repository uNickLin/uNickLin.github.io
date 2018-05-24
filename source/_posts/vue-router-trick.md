---
title: 如何讓 Vue 記住「上一頁」的位置
visible: true
date: 2018-05-19 21:14:54
tags: 
- Vue
categories:
- Tricks
---

在做自己第一個專案，同時也是第一個 Vue 專案時，就曾經被 PM 的一個需求卡住：「使用者回上一頁要知道他之前在哪」，這個問題據我所知之前好像都是交由後端來處理，像是會用 url 帶入某些參數來讓頁面去做回復視窗位置的動作，但 Vue ?

<!--more-->

在那個專案中我採用的做法是將 components keep-alive (沒錯幾乎全部)，來讓頁面一直保持離開時的位置，但這樣還不夠，還必須再透過一個瀏覽器偵測「上一頁」動作的 function: popstate(有點忘了)，來判斷是不是要回到頁面最頂部。

問題可想而知，每個頁面都不會被 destroy 的情況下，載入速度好像就有這麼一點差，慶幸的是這個專案規模還不算大，不至於讓人有過長的等待時間，而今天在進修 Maximilian Schwarzmüller 大神的 [Vue 2](https://www.udemy.com/vuejs-2-the-complete-guide/learn/v4/overview) 教學時剛好看到了這個部分！

一般而言 Vue router 再切換 router-view 時並不會幫你把視窗拉到最頂開始瀏覽(就算已經被 rebuild 的 component 也是)，所以我都會在每次 router 切換時將 html, body 拉到 (0, 0) 的位置，但其實 Vue router 已經有一個 [scrollBehavior](https://router.vuejs.org/en/advanced/scroll-behavior.html) 的 callback 給我們操作了：

```javascript=
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return desired position
  }
})
```
像剛剛回到最頂的問題就可以透過 return {x: 0, y: 0} 來完成
```javascript=
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    return {x: 0, y: 0}
  }
})
```

而回到這篇文章的重點，可以看到 scrollBehavior 第三個參數 savedPosition，就是這次的 key player，剛開始看完整份 doc 還是不知道要怎麼存取 savedPosition，但其實不用，實際做過一遍後只要像這樣：
```javascript=
scrollBehavior (to, from, savedPosition) {
  if (savedPosition) return savedPosition
  return {x: 0, y: 0}
}
```

也不需要透過 keep-alive，Vue router 便會自動幫你抓取瀏覽器所存的「上一頁」位置，同理「下一頁」亦可！看到 doc 裡面這段

> Note: this feature only works if the browser supports history.pushState.

也就是：只有在支援歷史紀錄的瀏覽器才適用（不負責翻譯）
這邊的歷史紀錄並不是指瀏覽過哪些網頁的紀錄，而是像在 Chrome 的主控台可以呼叫出 history 的指令的這個功能

看完這門課突然覺得腦門頓開絕頂升天，本來還在研究怎麼在 component 中寫紀錄滾動行為的 function ... 
<small><i>(沒錯還有動 localStorage 或 VueX 的歪腦筋)</i></small>