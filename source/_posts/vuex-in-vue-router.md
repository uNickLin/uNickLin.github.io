---
title: 如何在 Vue Router 中使用 Vuex
visible: true
date: 2018-05-29 14:09:30
tags:
- Vue 2.x
categories:
- Tricks
---

最近在練習的時候有遇到一個功能需求是：在 router 切換時都要關閉側邊欄 menu 的狀態(也就是隱藏)，因為 router view 本身只會做 component 抽換的動作，所以在 router view 之外的狀態都不會被更新。

<!--more-->

這個動作在點擊切換時沒有問題，我們可以在 menu 的 ul 加上 @click function 來關閉側邊欄。但如果使用者操作「上一頁」「下一頁」時就會出現問題。

![](https://i.imgur.com/7clnDHB.gif)

因為這次將這種全站式的功能寫進 Vuex 來操作，這樣如果其他 component 也需要用到 menu 開關的功能就會方便很多(不需要 props 來 emit 去了 ...)

```javascript
// store.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export const store = new Vuex.Store ({
  state: {
    isMenuOpen: false
  },
  mutations: {
    toggleMenu(state, boolean) {
      if (boolean === true || boolean === false) state.isMenuOpen = boolean
      else state.isMenuOpen = !state.isMenuOpen
    }
  }
})

```

根據這種需求看來，就可以使用到 router guard 中的 beforeEach 操作 Vuex 中的狀態，我們只消在 router 的檔案中引入存放 Vuex store 的檔案(e.g store.js)
```javascript
import { store } from '../store.js'
```

由於我們使用的 hook 是 beforeEach ，這時候 Vue 還沒掛載，如果使用 Vue.store.commit 等等的做法會導致抓不到物件(undefined)，因此這邊直接以 store 操作即可。
```javascript
import { store } from '../store.js'

router.beforeEach((to, from, next) => {
  store.commit('toggleMenu', false)
  next()
})
```

完成！
![](https://i.imgur.com/TReVGiL.gif)