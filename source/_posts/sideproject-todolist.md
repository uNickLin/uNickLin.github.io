---
title: 修煉前端 Week 1 - Todo List
visible: true
date: 2018-06-07 09:54:00
tags:
- Vue 2.x
- Vuex
- PWA
- Localstorage
categories:
- Side-projects
---

前陣子在 Facebook 上被廣吿推薦了一個「精神時光屋」的社團，原來裡面是六角學院的練功場，就毫不遲疑地加入，也很快迎來第迎來第一個活動：連續九週前端挑戰，第一週就是那個世人稱為最初的起點也是最後的終點的 Todo List (即興的霸氣 slogan)

<!--more-->

Todo List 真要說起來也可以做得很大，還記得剛接觸框架時很多教學就是帶你手把手的建立一個 Todo List

<iframe height='418' scrolling='no' title='[Vue] Todolist' src='//codepen.io/uNickHow/embed/MmzKQr/?height=418&theme-id=0&default-tab=result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/uNickHow/pen/MmzKQr/'>[Vue] Todolist</a> by uNickHow (<a href='https://codepen.io/uNickHow'>@uNickHow</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

以及面試某家公司時被要求的 [Angular Todo List](https://unicklin.github.io/Todolist_angular/)

![](https://i.imgur.com/JO9QmMH.png)

這些入門款的待辦事項不需要太多功能，只要可以 C R <small style='text-decoration: line-through; color: grey'>U(沒有你)</small> D 就好，但要做大還可以像 app 一樣有日期、提醒、上傳照片、分類等等，所以這次參加這項活動我就拿來實驗 Vue PWA

社團有提供基本的 user story 跟 [Wireframe](https://hexschool.github.io/THE_F2E_Design/todolist/)，接著就自行發揮囉！

![](https://i.imgur.com/bTBQKcG.png)

稍做一點微調後，呈現的 UI 變成[這樣](http://vtodo.s3-website-ap-northeast-1.amazonaws.com/)
![](https://i.imgur.com/qWRUNxv.png)

因為我使用的是 Localstorage 暫存，所以偏向文字紀錄而不套用檔案上傳的功能<small style='text-decoration: line-through; color: grey'>(也是可以做圖片上傳存成 base64 啦只是我懶 ...)</small>

也順手練習一下之前 Max 教的 transition，主要用在新增跟列表的 component 轉場，整體質感上升！
因為這次是以 PWA 為出發點，所以手機排版的部分就依照我自己覺得使用順手的仿 app Layout

![](https://i.imgur.com/veh6sHn.png)

還很貼心的告訴使用者可以用 Safari 加到桌面喔！
雖然他是壞的但還是很大心

------

這次的作品還有很大一部份的重心是自己真正實作 Vuex，之前都是看影片教學哦哦哦地點頭，好像很懂但其實不然，套用 Vuex 後終於告別 components 間的 emit hell 😭，不然那一坨一坨的 emit 每次看每次懷疑人生 ...

接下來就像 Todo 所寫的，要解決 Muuri.js 的套用，如果不行還是繼續使用 vue-draggable，然後<strong>解決 PWA 暫存問題！</strong>，順利的話說不定還可以離線使用，最後再來處理圖片上傳<small style='text-decoration: line-through; color: grey'>(如果有空, if, might, perhaps)</small>